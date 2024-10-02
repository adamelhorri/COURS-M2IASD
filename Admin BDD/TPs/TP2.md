---
tags:
  - AdminBDD
  - tps
---

## PL/SQL (Appropriation Suite)

### 1. Schéma de Base de Données
Le TP2 est la suite du TP1, et nous continuerons à utiliser le même schéma relationnel portant sur les employés et les départements.

### 2. Trigger, Procédure et SQL Dynamique
Dans cette section, vous allez utiliser certaines fonctionnalités du langage SQL dynamique, en particulier `EXECUTE IMMEDIATE`, pour supprimer tous les triggers définis jusqu'à présent. Vous allez créer une procédure nommée `suppTriggers` qui exploitera la vue `user_triggers`.

**Code de la Procédure :**
```sql
CREATE OR REPLACE PROCEDURE DELETE_ALL_TRIGGERS AS
    CURSOR curseur IS SELECT trigger_name FROM user_triggers;
BEGIN
    FOR rec IN curseur LOOP
        EXECUTE IMMEDIATE 'DROP TRIGGER ' || rec.trigger_name;
    END LOOP;
END;
/
EXEC DELETE_ALL_TRIGGERS;
```

**Explication :**
Cette procédure, `DELETE_ALL_TRIGGERS`, parcourt tous les triggers définis pour l'utilisateur courant à l'aide d'un curseur. Pour chaque trigger, elle construit une commande `DROP TRIGGER` et l'exécute dynamiquement. Cela permet de supprimer tous les triggers sans avoir à les lister manuellement.

### 3. Curseurs, Procédures et Fonctions

#### 3.1 Traitements Métiers

##### 3.1.1 Procédure `EmployesDuDepartement`
Vous devez définir une procédure qui prend en entrée un numéro de département et retourne une chaîne de caractères listant les employés de ce département. Vous incluez également une gestion des exceptions.

**Code de la Procédure :**
```sql
SET SERVEROUTPUT ON
SET LINESIZE 200

CREATE OR REPLACE PROCEDURE EmployesDuDepartement (numDep IN INTEGER, lesEmployes OUT VARCHAR) IS
    CURSOR C IS SELECT num, nom, salaire, fonction FROM emp WHERE n_dept = numDep;
    vide EXCEPTION;
BEGIN
    lesEmployes := '';
    FOR c_t IN C LOOP
        lesEmployes := lesEmployes || c_t.num || ' ' || c_t.nom || ' ' || c_t.salaire || ' ' || c_t.fonction || CHR(10);
    END LOOP;
    
    IF lesEmployes = '' THEN 
        RAISE vide;
    END IF;
EXCEPTION
    WHEN vide THEN 
        DBMS_OUTPUT.PUT_LINE('Pas de numéro correspondant');
    WHEN OTHERS THEN 
        DBMS_OUTPUT.PUT_LINE('Survenue du problème suivant: ' || SQLERRM);
END;
/

-- Programme Principal
DECLARE
    employes VARCHAR(1000);
BEGIN
    EmployesDuDepartement(10, employes);
    DBMS_OUTPUT.PUT_LINE(employes);
END;
/
```

**Explication :**
Cette procédure `EmployesDuDepartement` utilise un curseur pour récupérer les employés d'un département spécifié. Elle construit une chaîne contenant les informations sur chaque employé. Si aucun employé n'est trouvé, une exception personnalisée est levée et gérée, avec un message informatif affiché à l'utilisateur.

##### 3.1.2 Fonction `CoutSalarialDuDepartement`
Cette fonction prend en entrée un numéro de département et retourne le coût salarial total de ce département.

**Code de la Fonction :**
```sql
CREATE OR REPLACE FUNCTION CoutSalarialDuDepartement (numDep IN INTEGER) RETURN FLOAT IS
    somme FLOAT;
    erreur EXCEPTION;
BEGIN
    SELECT SUM(salaire) INTO somme FROM emp WHERE n_dept = numDep;
    IF somme IS NULL THEN 
        RAISE erreur; 
    END IF;
    RETURN somme;
EXCEPTION
    WHEN erreur THEN 
        RETURN -1;
END;
/

-- Utilisation de la Fonction
SELECT CoutSalarialDuDepartement(10) AS CoutEuros FROM dual;
SELECT CoutSalarialDuDepartement(2) AS CoutEuros FROM dual;

-- Programme Principal
DECLARE
    budget FLOAT;
BEGIN
    budget := CoutSalarialDuDepartement(10);
    DBMS_OUTPUT.PUT_LINE('Coût de fonctionnement: ' || budget || ' euros');
END;
/
```

**Explication :**
La fonction `CoutSalarialDuDepartement` calcule la somme des salaires pour le département donné. Si aucun salaire n'est trouvé (i.e., la somme est nulle), une exception est levée et la fonction retourne -1 pour indiquer une erreur. Le programme principal utilise cette fonction pour afficher le coût salarial d'un département spécifique.

### 3.2 Supervision Utilisateurs de la Base
Vous allez construire un paquetage nommé **Supervision** contenant plusieurs éléments pour surveiller l'utilisation de la base de données.

1. **Fonction pourcentageConnexions** : Renvoie le taux d'utilisation de la base master.
2. **Procédure lesConnectes** : Affiche le nombre de tables de chaque utilisateur connecté et le nombre total de tuples.
3. **Procédure objetLePlusVieux** : Affiche l'utilisateur possédant l'objet le plus ancien créé.
4. **Procédure lesDroitsUtilisateur** : Affiche les rôles et privilèges d'un utilisateur donné.

**Code du Paquetage :**
```sql
CREATE OR REPLACE PACKAGE supervise AS
    FUNCTION pourcentageConnexions RETURN NUMBER;
    PROCEDURE lesConnectes;
    PROCEDURE lesTablesDesConnectes;
    PROCEDURE objetLePlusVieux;
    PROCEDURE lesDroitsUtilisateur(usager IN VARCHAR);
END;

CREATE OR REPLACE PACKAGE BODY supervise AS
    FUNCTION pourcentageConnexions RETURN NUMBER AS
        nbre1 INTEGER;
        nbre2 INTEGER;
    BEGIN
        SELECT COUNT(DISTINCT username) INTO nbre1 FROM dba_users;
        SELECT COUNT(username) INTO nbre2 FROM v$session WHERE type = 'USER';
        RETURN nbre2 / nbre1 * 100;
    EXCEPTION 
        WHEN OTHERS THEN 
            DBMS_OUTPUT.PUT_LINE(SQLCODE || ' ' || SQLERRM);
    END;

    PROCEDURE lesConnectes IS
        CURSOR c IS 
            SELECT RPAD(s.username, 14) AS usager, s.module AS logiciel,
                   RPAD(s.osuser, 40) AS osUsager, p.program,
                   TO_CHAR(s.logon_time, 'DD-MM-YY---HH-MM-SS') AS dateConnexion, 
                   s.machine AS machina,
                   p.pid AS oracleProcess, p.spid AS osProcess
            FROM v$session s, v$process p 
            WHERE s.paddr = p.addr AND s.type = 'USER';
    BEGIN
        FOR t IN c LOOP
            DBMS_OUTPUT.PUT_LINE(t.usager || ' ' || t.osUsager || ' Processus Système ' || t.osProcess || ' le ' || RPAD(t.dateConnexion, 16));
        END LOOP;
    END;

    PROCEDURE lesTablesDesConnectes IS
        CURSOR c1 IS 
            SELECT COUNT(table_name) AS nbTables, SUM(num_rows) AS nbTuples, owner 
            FROM dba_tables d, v$session v 
            WHERE d.owner = v.username AND v.type = 'USER' 
            GROUP BY owner ORDER BY owner;
    BEGIN
        FOR t1 IN c1 LOOP
            DBMS_OUTPUT.PUT_LINE(RPAD(t1.owner, 14) || ' pour ' || t1.nbTables || ' tables et ' || t1.nbTuples || ' tuples ');
            DBMS_OUTPUT.PUT_LINE('============================');
            FOR t2 IN (SELECT table_name AS tbl, owner FROM dba_tables WHERE owner = t1.owner) LOOP
                DBMS_OUTPUT.PUT_LINE(RPAD(t2.tbl, 36) || ' ' || RPAD(t2.owner, 14));
            END LOOP;
        END LOOP;
    END;

    PROCEDURE objetLePlusVieux IS
        CURSOR c1 IS 
            SELECT object_name, object_type, created, owner 
            FROM dba_objects d, v$session v 
            WHERE d.owner = v.username AND v.type = 'USER' 
            AND created <= ALL (SELECT created FROM dba_objects d, v$session v WHERE d.owner = v.username AND v.type = 'USER');
    BEGIN
        FOR t1 IN c1 LOOP
            DBMS_OUTPUT.PUT_LINE(RPAD(t1.owner, 14) || ' pour objet ' || t1.object_name || ' de type ' || t1.object_type || ' création ' || t1.created);
            DBMS_OUTPUT.PUT_LINE('============================');
        END LOOP;
    END;

    PROCEDURE lesDroitsUtilisateur(usager IN VARCHAR) IS
        CURSOR c1 IS 
            SELECT granted_role, admin_option FROM dba_role_privs WHERE grantee = usager;
    BEGIN
        FOR t1 IN c1 LOOP
            DBMS_OUTPUT.PUT_LINE(RPAD(t1.granted_role, 14) || ' ' || t1.admin_option);
            DBMS_OUTPUT.PUT_LINE('============================');
        END LOOP;
    EXCEPTION
        WHEN OTHERS THEN 
            DBMS_OUTPUT.PUT_LINE(SQLCODE || ' ' || SQLERRM);
    END;
END;
/

-- Exécution des Fonctions et Procédures
SELECT supervise.pourcentageConnexions FROM dual;
EXECUTE supervise.lesConnectes;
EXECUTE supervise.lesTables

DesConnectes;
EXECUTE supervise.lesDroitsUtilisateur(user);
```

**Explication :**
Ce paquetage, **Supervision**, permet au DBA de surveiller l'activité des utilisateurs de la base de données. Il comprend des fonctions et des procédures qui permettent d'obtenir des statistiques sur les connexions, de visualiser les tables et les droits d'utilisateur, et d'afficher l'objet le plus ancien créé par les utilisateurs connectés. Le paquetage utilise des vues dynamiques du dictionnaire de données pour extraire des informations pertinentes.

---

Ce TP vous permet de manipuler des triggers, de créer des procédures et des fonctions en PL/SQL, ainsi que d'utiliser des paquetages pour gérer et surveiller les utilisateurs d'une base de données Oracle.