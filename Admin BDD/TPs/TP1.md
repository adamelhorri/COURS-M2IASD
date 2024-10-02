---
tags:
  - AdminBDD
  - tps
---

## PL/SQL (Séance Sécurité des Schémas)

### 1. Schéma de Base de Données
Dans ce TP, nous allons travailler sur le schéma relationnel **Employé**, qui est un schéma de test classique pour Oracle. Le script de création des tables est fourni dans le fichier **ScriptCreation.sql**. Voici la structure des tables :

- **Dept(n_dept, nom, lieu)**
- **Emp(nom, num, fonction, n_sup, embauche, salaire, com, n_dept)**

Avec les relations :
- **Emp(n_dept) ⊆ Dept(n_dept)**
- **Emp(n_sup) ⊆ Emp(num)**

### 2. Contraintes Clés Primaires et Étrangères
Vous exécuterez le script de création une fois connecté à Oracle (base de données master).

**Exemple de connexion :**
```plaintext
rlwrap sqlplus exxxxxx/yyyy@oracle.etu.umontpellier.fr:1523/pmaster
```

Si vous n'avez pas de compte ou rencontrez un problème, consultez le site suivant :  
[https://sapiens.umontpellier.fr/](https://sapiens.umontpellier.fr/)

**Ajout des contraintes :**
```sql
ALTER TABLE emp ADD CONSTRAINT emp_pk PRIMARY KEY (num);
ALTER TABLE dept ADD CONSTRAINT dept_pk PRIMARY KEY (n_dept);
ALTER TABLE emp ADD CONSTRAINT emp_fk1 FOREIGN KEY (n_sup) REFERENCES emp(num) ON DELETE CASCADE;
ALTER TABLE emp ADD CONSTRAINT emp_fk2 FOREIGN KEY (n_dept) REFERENCES dept(n_dept) ON DELETE CASCADE;
```

**Explication :**
Ces commandes définissent les clés primaires pour les tables `emp` et `dept`, ainsi que des clés étrangères pour assurer l'intégrité référentielle. L'option `ON DELETE CASCADE` permet de supprimer automatiquement les enregistrements d'employés associés à un département qui a été supprimé.

### 3. Triggers LMD
Une partie des questions est à traiter en non présentiel.

#### 1. Construction d'un Trigger `rennesMille`
Vous allez construire un trigger nommé `rennesMille` qui vérifie que le salaire des employés est toujours supérieur à 1000 euros, tant à l'insertion qu'à la mise à jour, mais uniquement pour les employés travaillant dans le département situé à Rennes.

**Code du Trigger :**
```sql
CREATE OR REPLACE TRIGGER rennesMille
AFTER INSERT OR UPDATE OF salaire ON emp
FOR EACH ROW
DECLARE
    n_d dept.n_dept%TYPE;
BEGIN
    SELECT n_dept INTO n_d FROM dept WHERE lieu = 'Rennes';
    IF :new.n_dept = n_d AND :new.salaire < 1000 THEN 
        RAISE_APPLICATION_ERROR(-20100, 'Forbidden: salaire inférieur à 1000 euros');
    END IF;
EXCEPTION
    WHEN NO_DATA_FOUND THEN 
        RAISE_APPLICATION_ERROR(-20001, 'Aucun département à Rennes ' || SQLCODE);
    WHEN TOO_MANY_ROWS THEN 
        RAISE_APPLICATION_ERROR(-20002, 'Plusieurs départements à Rennes ' || SQLCODE);
    WHEN OTHERS THEN 
        RAISE_APPLICATION_ERROR(-20003, 'Problème survenu avec le code et le message : ' || SQLCODE || ' ' || SQLERRM);
END;
/
```

**Explication :**
Ce trigger vérifie, après une insertion ou une mise à jour, si le département de l'employé est à Rennes et si son salaire est inférieur à 1000 euros. Si c'est le cas, il génère une erreur. Les exceptions gèrent les cas où le département n'est pas trouvé ou s'il y a plusieurs départements à Rennes.

#### 2. Procédure `JoursEtHeuresOuvrables`
Vous allez définir une procédure qui vérifie que la date du jour n'est pas un samedi ni un dimanche.

**Code de la Procédure :**
```sql
CREATE OR REPLACE PROCEDURE ouvrable IS
BEGIN
    IF TRIM(TO_CHAR(SYSDATE, 'DY')) = 'JEU.' THEN
        RAISE_APPLICATION_ERROR(-20100, 'Forbidden: Jour non ouvrable');
    END IF;
END;
/
```

**Redéfinition du Trigger :**
```sql
CREATE OR REPLACE TRIGGER t_ouvrable
AFTER INSERT OR UPDATE OR DELETE ON emp
BEGIN
    ouvrable;
END;
/
```

**Explication :**
La procédure `ouvrable` vérifie si le jour courant est un jour ouvrable. Si ce n'est pas le cas, une erreur est levée. Le trigger `t_ouvrable` appelle cette procédure pour s'assurer que les opérations sur la table `emp` ne se déroulent pas les jours non ouvrables.

#### 3. Table Historique et Trigger `monitor`
Vous allez créer une table historique qui enregistre toutes les opérations sur la table `Dept`.

**Code de la Table Historique :**
```sql
CREATE TABLE historique (dateOperation DATE, usager VARCHAR(20), typeOperation VARCHAR(20));
```

**Code du Trigger :**
```sql
CREATE OR REPLACE TRIGGER monitor
BEFORE INSERT OR UPDATE OR DELETE ON dept
FOR EACH ROW
BEGIN
    IF INSERTING THEN 
        INSERT INTO historique VALUES (SYSDATE, USER, 'Insertion');
    ELSIF DELETING THEN 
        INSERT INTO historique VALUES (SYSDATE, USER, 'Suppression');
    ELSE 
        INSERT INTO historique VALUES (SYSDATE, USER, 'Modification');
    END IF;
END;
/
```

**Explication :**
Le trigger `monitor` enregistre chaque opération (insertion, suppression, modification) effectuée sur la table `dept` dans la table `historique`, en stockant la date de l'opération, l'utilisateur et le type d'opération.

#### 4. Trigger `cascade` pour Suppressions et Modifications
Vous allez construire un trigger qui gère les suppressions et modifications dans la table `Dept`.

**Code du Trigger :**
```sql
CREATE OR REPLACE TRIGGER t_updateCascade
AFTER DELETE OR UPDATE ON dept
FOR EACH ROW
BEGIN
    IF DELETING THEN
        DELETE FROM emp WHERE n_dept = :OLD.n_dept;
    ELSIF UPDATING THEN
        UPDATE emp SET n_dept = :NEW.n_dept WHERE n_dept = :OLD.n_dept;
    END IF;
END;
/
```

**Explication :**
Ce trigger s'assure que lorsque un département est supprimé ou modifié, les enregistrements correspondants dans la table `emp` sont également supprimés ou mis à jour. Cela garantit l'intégrité référentielle dans la base de données.

### 4. Triggers LDD
Vous allez créer un trigger qui s'exécute chaque fois qu'un nouvel objet (table, etc.) est créé dans votre schéma.

**Code du Trigger :**
```sql
CREATE OR REPLACE TRIGGER creation 
BEFORE CREATE ON schema_name
BEGIN
    DBMS_OUTPUT.PUT_LINE('Création aboutie le ' || TO_CHAR(SYSDATE, 'DAY'));
END;
/
```

**Explication :**
Ce trigger affiche un message chaque fois qu'un nouvel objet est créé dans le schéma, permettant de suivre les changements dans la structure de la base de données.

### 5. Triggers d'Instance
Vous allez créer un trigger qui se déclenche à chaque connexion utilisateur.

**Code de la Table `QuiSeConnecte` :**
```sql
DROP TABLE QuiSeConnecte;
CREATE TABLE QuiSeConnecte (
    c_user VARCHAR2(60),
    os_user VARCHAR2(60),
    ip_address VARCHAR2(20),
    c_date DATE
);
```

**Code du Trigger :**
```sql
CREATE OR REPLACE TRIGGER logon_db
AFTER LOGON ON DATABASE
DECLARE
    o_user QuiSeConnecte.os_user%TYPE;
    i_address QuiSeConnecte.ip_address%TYPE;
    session_user QuiSeConnecte.c_user%TYPE;
BEGIN
    SELECT SYS_CONTEXT('USERENV', 'SESSION_USER'), 
           SYS_CONTEXT('USERENV', 'OS_USER'),
           SYS_CONTEXT('USERENV', 'IP_ADDRESS') 
    INTO session_user, o_user, i_address 
    FROM dual;
    
    INSERT INTO QuiSeConnecte VALUES (session_user, o_user, i_address, SYSDATE);
    COMMIT;
END;
/
```

**Explication :**
Ce trigger `logon_db` enregistre des informations sur chaque connexion utilisateur, telles que le nom de l'utilisateur, l'utilisateur du système d'exploitation, l'adresse IP, et la date de connexion. Ces données sont stockées dans la table `QuiSeConnecte` pour un suivi ultérieur.
