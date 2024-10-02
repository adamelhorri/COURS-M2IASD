---
tags:
  - AdminBDD
  - cours
---
### PL/SQL : Généralités

PL/SQL est une surcouche procédurale d’Oracle qui permet de renforcer la sécurité des bases de données, superviser et évaluer leur performance, et gérer les applications directement au sein du SGBD. Ce langage inclut des fonctionnalités essentielles comme les procédures, les fonctions, les triggers (déclencheurs) et supporte les types de données utilisateur, ce qui en fait un outil clé pour les développeurs Oracle.

#### Principales fonctions :

1. **Renforcement de la sécurité** : Contrôle des accès et gestion des triggers.
2. **Supervision et performance** : Gestion et optimisation des requêtes via l’utilisation de curseurs.
3. **Langage transactionnel** : Gère les transactions via des blocs atomiques, garantissant l'intégrité des données.

### Syntaxe et Composants de PL/SQL

Le langage PL/SQL est composé de plusieurs éléments :

- **Programme principal** : Déclaré avec `DECLARE`, il contient des variables, curseurs et exceptions, et se termine par un bloc `BEGIN...END`.
    
- **Curseurs** : Utilisés pour manipuler des ensembles de résultats d'une requête. Il existe deux types de curseurs :
    
    - **Curseur implicite** : Pour une requête retournant un seul tuple.
    - **Curseur explicite** : Pour une requête retournant plusieurs tuples.
    
    **Exemple d’utilisation :**
    
    - Implicite :
        
        sql
        
        Copier le code
        
        `DECLARE   name emp.nom%type;   numero emp.num%type; BEGIN   SELECT num, nom INTO numero, name FROM emp WHERE num=79;   DBMS_OUTPUT.PUT_LINE('Numéro et nom : ' || numero || ' ' || name); END;`
        
    - Explicite :
        
        sql
        
        Copier le code
        
        `DECLARE   CURSOR mon_curseur IS SELECT num, nom, fonction FROM EMP; BEGIN   FOR emp_rec IN mon_curseur   LOOP     DBMS_OUTPUT.PUT_LINE(emp_rec.num || ' ' || emp_rec.nom || ' ' || emp_rec.fonction);   END LOOP; END;`
        
- **Exceptions** : Utilisées pour gérer les anomalies dans le programme. Elles sont déclarées, levées puis traitées. **Exemple :**
    
    sql
    
    Copier le code
    
    `DECLARE   mon_exception EXCEPTION; BEGIN   IF condition THEN     RAISE mon_exception;   END IF; EXCEPTION   WHEN mon_exception THEN     DBMS_OUTPUT.PUT_LINE('Erreur personnalisée'); END;`
    
- **Itérations et conditions** : PL/SQL permet des boucles `FOR` et des conditions `IF` pour itérer et contrôler le flux des instructions.
    

### Paquetages

PL/SQL offre des paquetages (ensembles de procédures et fonctions) qui permettent de modulariser le code. Des paquetages pré-définis comme `DBMS_OUTPUT` ou `DBMS_STATS` sont disponibles, mais il est aussi possible de définir des paquetages spécifiques à un métier.

#### Exemple de paquetage :

Le paquetage `DBMS_OUTPUT` inclut des procédures comme `PUT_LINE` pour afficher du texte ou des variables sur le périphérique de sortie standard.

### Exceptions prédéfinies

PL/SQL fournit des exceptions prédéfinies pour gérer des erreurs courantes, comme :

- **NO DATA FOUND** : Aucune donnée n'a été trouvée lors de l’exécution d’une requête.
- **ZERO DIVIDE** : Division par zéro.
- **VALUE ERROR** : Erreur de type de valeur.
- **TOO MANY ROWS** : Trop de résultats retournés par une requête.
- **CURSOR ALREADY OPEN** : Tentative d'ouverture d'un curseur déjà ouvert.

Ces exceptions sont gérées via la procédure `RAISE_APPLICATION_ERROR`, qui permet de lever des erreurs spécifiques avec des codes d'erreur et des messages personnalisés.

### Structures conditionnelles

PL/SQL utilise des structures conditionnelles comme :

- **IF** : Pour les conditions simples et imbriquées (`ELSIF`).
- **CASE** : Permet de tester plusieurs conditions dans une syntaxe plus lisible.

**Exemple d'IF imbriqué** :

sql

Copier le code

`IF sal < 1000 THEN prime:=300; ELSIF sal < 1500 THEN prime:=300; ELSIF sal < 2000 THEN prime:=200; ELSE prime:=100; END IF;`

### Boucles itératives

Trois types de boucles sont disponibles :

- **Boucle infinie** : Utilise `loop ... end loop` avec une condition `exit` pour arrêter la boucle.
- **Boucle for** : Utilise un compteur pour itérer dans une plage de valeurs.
- **Boucle while** : Continue tant qu'une condition donnée est vraie.

**Exemple d'une boucle while** :

sql

Copier le code

`DECLARE   i INTEGER := 20000; BEGIN   WHILE i <= 20010 LOOP     INSERT INTO emp (nom, num) VALUES ('pierrot', i);     i := i + 1;   END LOOP; END;`

### Procédures et fonctions

Les **procédures** et **fonctions** permettent de modulariser le code dans PL/SQL. Elles sont définies au niveau du schéma et peuvent être appelées par les utilisateurs.

- **Procédure** : Ne retourne pas de valeur, mais peut avoir des arguments en entrée (`IN`), en sortie (`OUT`), ou les deux (`IN OUT`).
    
    **Exemple d'une procédure de conversion monétaire** :
    
    sql
    
    Copier le code
    
    `CREATE OR REPLACE PROCEDURE P_CEF (euros IN NUMBER, francs OUT NUMBER) IS BEGIN   francs := euros * 6.559; END;`
    
- **Fonction** : Retourne une valeur et peut être utilisée directement dans une requête SQL.
    
    **Exemple d'une fonction de conversion monétaire** :
    
    sql
    
    Copier le code
    
    `CREATE OR REPLACE FUNCTION F_CEF (euros IN NUMBER) RETURN NUMBER IS   francs NUMBER; BEGIN   francs := euros * 6.559;   RETURN (francs); END;`
    

## Déclencheurs : 
### Généralités

Les **déclencheurs** sont une forme de **programmation événementielle** en PL/SQL, activés lorsqu'un événement spécifique se produit dans la base de données. Ils suivent une structure **ECA (Événement-Condition-Action)**, permettant d'automatiser des actions en réponse à des modifications de données.

Il existe plusieurs types de déclencheurs :

- **Déclencheurs LMD** (Langage de Manipulation de Données) : Surviennent lors des opérations de manipulation des données comme `INSERT`, `UPDATE`, ou `DELETE`.
- **Déclencheurs LDD** (Langage de Définition de Données) : Surviennent lors des modifications de la structure des bases de données (ajout, suppression d'objets).
- **Déclencheurs d'instance** : Activés lors d'événements au niveau de l'instance de base de données (ex : connexion d'un utilisateur).

### Déclencheurs LMD

Les déclencheurs LMD peuvent être exécutés **avant (before)** ou **après (after)** un événement de manipulation de données. Ils peuvent également s’appliquer à chaque ligne (`for each row`) ou à l'ensemble des lignes affectées.

Les déclencheurs peuvent manipuler les valeurs **old** (anciennes) et **new** (nouvelles) des tuples affectés.

**Exemple de déclencheur LMD** :

sql

Copier le code

`CREATE OR REPLACE TRIGGER Alert AFTER DELETE OR INSERT OR UPDATE ON EMP FOR EACH ROW BEGIN   DBMS_OUTPUT.PUT_LINE('Fin de l''opération'); END;`

### Déclencheurs avec conditions

Les déclencheurs peuvent inclure des conditions spécifiques via la clause `WHEN`. Ils peuvent ainsi vérifier des valeurs avant de déclencher une action.

**Exemple de vérification de salaire** :

sql

Copier le code

`CREATE TRIGGER verif_salaire BEFORE INSERT OR UPDATE OF salaire ON EMP FOR EACH ROW WHEN (new.fonction != 'president') BEGIN   IF (:new.salaire < 1000) THEN     RAISE_APPLICATION_ERROR(-20022, :new.nom || ' pas assez payé');   ELSIF (:new.salaire > 5000) THEN     RAISE_APPLICATION_ERROR(-20023, :new.nom || ' trop payé');   END IF; END;`

### Déclencheurs avant/après

Les déclencheurs **before** modifient souvent les valeurs avant leur insertion dans la base, tandis que les déclencheurs **after** exécutent des actions une fois les données modifiées.

**Exemple before** : Modification d'une chaîne de caractères avant insertion :

sql

Copier le code

`CREATE OR REPLACE TRIGGER Before_D BEFORE INSERT ON EMP FOR EACH ROW BEGIN   :new.nom := UPPER(:new.nom);   DBMS_OUTPUT.PUT_LINE('Insertion du tuple ' || :new.nom); END;`

### Déclencheurs globaux

Un déclencheur global peut suivre toutes les mises à jour sur une table et enregistrer ces événements dans une autre table.

**Exemple de déclencheur global** :

sql

Copier le code

`CREATE OR REPLACE TRIGGER Global_update BEFORE UPDATE ON EMP BEGIN   INSERT INTO log VALUES ('update trigger', SYSDATE, USER); END;`

### Déclencheurs d'instance

Les déclencheurs d'instance sont utilisés pour surveiller des événements au niveau de l'instance de base de données, comme la connexion des utilisateurs.

**Exemple de déclencheur de connexion** :

sql

Copier le code

`CREATE OR REPLACE TRIGGER logon_db AFTER LOGON ON DATABASE DECLARE   o_user VARCHAR2(60); BEGIN   SELECT SYS_CONTEXT('USERENV', 'OS_USER') INTO o_user FROM dual;   INSERT INTO QuiSeConnecte VALUES (USER, o_user, SYSDATE);   COMMIT; END;`

### Gestion des déclencheurs

Les déclencheurs peuvent être manipulés via des commandes spécifiques :

- **Lister les déclencheurs** : `SELECT trigger_name FROM user_triggers;`
- **Supprimer un déclencheur** : `DROP TRIGGER nom_declencheur;`
- **Désactiver un déclencheur** : `ALTER TRIGGER nom_declencheur DISABLE;`
- **Réactiver un déclencheur** : `ALTER TRIGGER nom_declencheur ENABLE;`

