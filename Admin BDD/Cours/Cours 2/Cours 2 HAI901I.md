---
tags:
  - AdminBDD
  - cours
---

### Paquetages PL/SQL : Généralités

Un **paquetage** en PL/SQL est une structure modulaire qui regroupe des constantes, des variables, des procédures, des fonctions, et parfois des exceptions. Il offre une approche organisée et centralisée pour structurer les codes, facilitant la réutilisabilité et la maintenance du code métier.

Les principaux avantages des paquetages sont :

1. **Réutilisabilité** : Une fois un paquetage défini, ses composants peuvent être réutilisés à différents endroits dans l’application.
2. **Maintenance** : Le regroupement logique des composants liés à un domaine spécifique améliore la lisibilité et l'entretien du code.
3. **Testabilité** : Grâce à la structure modulaire, les différentes fonctions et procédures peuvent être testées et validées indépendamment.
4. **Vision modulaire** : Chaque paquetage outille un domaine métier spécifique, facilitant ainsi l’organisation du code métier.

### Paquetages prédéfinis

PL/SQL propose des **paquetages prédéfinis** qui couvrent une vaste gamme de fonctionnalités. Voici quelques exemples de paquetages standard :

- **DBMS_OUTPUT** : Permet d'envoyer des messages à la sortie standard en stockant les informations dans un tampon mémoire. Il est souvent utilisé pour déboguer des procédures stockées.
    
    - **Exemple d'utilisation** :
        
        sql
        
        Copier le code
        
        `EXEC DBMS_OUTPUT.PUT_LINE('Message de débogage');`
        
- **DBMS_METADATA** : Fournit des informations sur la structure des objets de la base de données (tables, vues, index, etc.). Il interroge le **dictionnaire de données** pour obtenir ces informations.
    
- **UTL_FILE** : Permet la lecture et l'écriture de fichiers externes au SGBD. Cela est utile pour exporter ou importer des données dans des fichiers texte.
    
- **DBMS_ALERT** : Sert à notifier les utilisateurs d’événements dans la base de données, souvent utilisé pour la gestion des alertes dans un environnement transactionnel.
    
- **DBMS_SQL** : Utilisé pour l'exécution dynamique de requêtes SQL. Cela est utile pour les scénarios où la structure de la requête n'est pas connue à l'avance et doit être générée dynamiquement à l’exécution.
    
- **DBMS_STATS** : Collecte des statistiques sur les objets de la base de données, très utile pour l'optimisation des requêtes par l’optimiseur Oracle. Par exemple, il permet de maintenir des statistiques sur les index et les tables afin de garantir des performances optimales.
    

### Construction d'un Paquetage

Un paquetage PL/SQL est composé de deux parties :

1. **Spécification du paquetage** : Contient les déclarations des variables, des constantes, des types, des fonctions et des procédures accessibles à l'extérieur du paquetage. C’est la partie visible du paquetage.
    
2. **Corps du paquetage** : Définit l’implémentation des fonctions, procédures, et des éléments privés. Le corps du paquetage contient la logique métier, mais certains éléments peuvent rester privés et invisibles depuis l'extérieur.
    

**Exemple de déclaration d'un paquetage :**

sql

Copier le code

`CREATE OR REPLACE PACKAGE Finances AS   vTx_EF CONSTANT NUMBER := 6.55957;  -- Constante pour conversion Euro-Franc   vTx_ED CONSTANT NUMBER := 1.3926;   -- Constante pour conversion Euro-Dollar   FUNCTION conversionF_EF (euros IN NUMBER) RETURN NUMBER;  -- Conversion Euro en Franc   PROCEDURE conversionP_EF (euros IN NUMBER, francs OUT NUMBER); -- Conversion avec retour OUT   FUNCTION conversionF_ED (euros IN NUMBER) RETURN NUMBER;  -- Conversion Euro en Dollar END Finances;`

Dans cet exemple, le paquetage **Finances** contient des constantes et des fonctions/procédures pour la conversion monétaire. La spécification contient les éléments accessibles publiquement, comme les constantes de conversion et les signatures des fonctions.

**Exemple du corps du paquetage** :

sql

Copier le code

`CREATE OR REPLACE PACKAGE BODY Finances AS   FUNCTION conversion (montant IN NUMBER, taux IN NUMBER) RETURN NUMBER IS   BEGIN     RETURN (ROUND(montant * taux, 2));  -- Arrondit le résultat à 2 décimales   EXCEPTION     WHEN OTHERS THEN RETURN NULL;  -- Gestion des erreurs   END;    FUNCTION conversionF_ED (euros IN NUMBER) RETURN NUMBER IS   BEGIN     RETURN conversion(euros, vTx_ED);  -- Utilise le taux Euro-Dollar   END;    FUNCTION conversionF_EF (euros IN NUMBER) RETURN NUMBER IS   BEGIN     RETURN conversion(euros, vTx_EF);  -- Utilise le taux Euro-Franc   END;    PROCEDURE conversionP_EF (euros IN NUMBER, francs OUT NUMBER) IS   BEGIN     francs := ROUND(euros * vTx_EF, 2);  -- Procédure avec retour par paramètre OUT   EXCEPTION     WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE('Erreur dans les arguments');   END; END Finances;`

Ici, le **corps** implémente la logique de conversion. Il inclut une gestion d'erreurs pour capturer tout problème potentiel dans les calculs.

### Utilisation d’un Paquetage

Une fois créé, un paquetage peut être appelé depuis des requêtes SQL ou des blocs PL/SQL.

**Exemple d’utilisation des fonctions du paquetage Finances** :

sql

Copier le code

`SELECT salaire,         Finances.conversionF_ED(salaire) AS salaire_en_dollars,        Finances.conversionF_EF(salaire) AS salaire_en_francs FROM emp;`

Il est également possible de partager un paquetage avec d'autres utilisateurs de la base en accordant des droits spécifiques :

sql

Copier le code

`GRANT EXECUTE ON Finances TO public; CREATE PUBLIC SYNONYM calculetteFinanciere FOR Finances;`

Cela permet à d'autres utilisateurs d'exécuter les fonctions du paquetage sans avoir à se soucier du schéma d'origine.

### Exceptions en PL/SQL

Les exceptions permettent de gérer les erreurs et les événements inattendus dans les programmes PL/SQL. Une **exception** est levée lorsqu'une condition d'erreur survient, interrompant ainsi l'exécution normale du programme. Chaque exception a un **code d'erreur** associé (`SQLCODE`) et un **message** (`SQLERRM`).

**Exemple de gestion d'exception** :

sql

Copier le code

`BEGIN   UPDATE emp SET salaire = NULL WHERE n_dept = 10; EXCEPTION   WHEN OTHERS THEN     DBMS_OUTPUT.PUT_LINE('Code erreur : ' || SQLCODE);     DBMS_OUTPUT.PUT_LINE('Message erreur : ' || SQLERRM); END;`

Dans cet exemple, si l'instruction `UPDATE` génère une erreur (par exemple, en essayant de définir un `NULL` sur une colonne non nullable), l'exception est capturée, et le code et le message d'erreur sont affichés.

**Cas pratique des exceptions** : Les exceptions sont souvent utilisées pour intercepter des erreurs de contraintes (par exemple, une tentative de mise à jour d'une colonne `NOT NULL` avec une valeur `NULL`). Dans ce cas, la gestion des erreurs permet de capturer l'exception et d'afficher un message explicatif tout en évitant une interruption brutale de l'exécution.

### Exceptions en PL/SQL

Les **exceptions** permettent de gérer les erreurs qui se produisent pendant l'exécution des programmes PL/SQL. Lorsqu'une erreur survient, elle interrompt l'exécution normale et déclenche une exception. Ces erreurs peuvent être capturées et gérées via des blocs d'exception `EXCEPTION WHEN ...`.

#### Types d'Exceptions

1. **Exceptions système** : Ce sont les exceptions prédéfinies par Oracle, comme :
    
    - **NO_DATA_FOUND** : Levée lorsqu'aucune donnée n'est trouvée par une requête.
    - **ZERO_DIVIDE** : Levée lorsqu’une division par zéro est tentée.
    - **OTHERS** : Permet de capturer toute autre exception non spécifiée.
2. **Exceptions définies par l'utilisateur** : Les développeurs peuvent définir leurs propres exceptions pour gérer des cas spécifiques. Ces exceptions sont levées via l’instruction `RAISE`.
    

**Exemple d’exception utilisateur :**

sql

Copier le code

`DECLARE   monExc EXCEPTION;   montant NUMBER(7,2); BEGIN   montant := &montant;   IF montant < 800 THEN     RAISE monExc;   END IF;   UPDATE emp SET salaire = montant WHERE n_dept = 10; EXCEPTION   WHEN monExc THEN     DBMS_OUTPUT.PUT_LINE('Code erreur : ' || SQLCODE || ' et message erreur : ' || SQLERRM); END;`

Dans cet exemple, si le montant est inférieur à 800, l'exception utilisateur `monExc` est levée et traitée dans le bloc d'exception.

#### Directive PRAGMA EXCEPTION_INIT

La directive `PRAGMA EXCEPTION_INIT` permet d’associer un code d'erreur Oracle à une exception définie par l’utilisateur. Cela facilite la gestion de certaines erreurs spécifiques.

**Exemple avec PRAGMA EXCEPTION_INIT** :

sql

Copier le code

`DECLARE   monExc EXCEPTION;   PRAGMA EXCEPTION_INIT(monExc, -20100);  -- Associe l'erreur -20100 à monExc   montant NUMBER(7,2); BEGIN   montant := &montant;   IF montant < 800 THEN     RAISE_APPLICATION_ERROR(-20100, 'Montant trop faible');   END IF;   UPDATE emp SET salaire = montant WHERE n_dept = 10; EXCEPTION   WHEN monExc THEN     DBMS_OUTPUT.PUT_LINE('Code erreur : ' || SQLCODE || ' et message erreur : ' || SQLERRM); END;`

Dans cet exemple, si le montant est trop faible, l'erreur personnalisée `-20100` est levée avec un message explicatif.

### SQL Dynamique

Le **SQL dynamique** est une fonctionnalité qui permet de construire et d'exécuter des instructions SQL à la volée, en fonction de données ou de conditions qui ne sont connues qu'à l'exécution. Ce type de SQL est souvent utilisé dans deux situations principales :

1. **Requêtes répétitives** avec des valeurs qui varient à l'exécution. Cela est utile pour améliorer la **performance** des accès et pour sécuriser les accès en évitant les injections SQL.
2. **Recours aux commandes DDL** (ex : `CREATE`, `DROP`, `ALTER`) qui modifient la structure de la base de données.

#### Exemple d'utilisation de `EXECUTE IMMEDIATE`

`EXECUTE IMMEDIATE` permet d’exécuter une commande SQL dynamique construite au moment de l’exécution.

**Exemple : Suppression de table avec SQL dynamique** :

sql

Copier le code

`CREATE OR REPLACE PROCEDURE supp (tble_name IN VARCHAR) IS BEGIN   EXECUTE IMMEDIATE 'DROP TABLE ' || tble_name || ' CASCADE CONSTRAINTS';   DBMS_OUTPUT.PUT_LINE('Table ' || tble_name || ' supprimée'); EXCEPTION   WHEN OTHERS THEN     DBMS_OUTPUT.PUT_LINE('Problème lors de la suppression'); END;`

Dans cet exemple, la procédure `supp` permet de supprimer dynamiquement une table dont le nom est passé en paramètre. `EXECUTE IMMEDIATE` est utilisé pour construire la commande `DROP TABLE` à la volée.

#### Gestion des variables liées (bind variables)

Les variables liées permettent de sécuriser les requêtes SQL dynamiques en évitant les injections SQL.

**Exemple d'utilisation des variables liées** :

sql

Copier le code

`CREATE OR REPLACE FUNCTION get_num_of_employees (p_job VARCHAR2) RETURN NUMBER IS   v_query_str VARCHAR2(1000);   v_num_of_employees NUMBER; BEGIN   v_query_str := 'SELECT COUNT(*) FROM emp WHERE fonction = :bind_job';   EXECUTE IMMEDIATE v_query_str INTO v_num_of_employees USING p_job;   RETURN v_num_of_employees; END;`

Ici, la variable `p_job` est liée à la requête SQL dynamique via `:bind_job`, ce qui empêche les injections SQL. La fonction `get_num_of_employees` renvoie le nombre d'employés ayant une fonction spécifique.

#### Exécution d’une commande DDL avec SQL dynamique

Les commandes **DDL** (Data Definition Language) comme `CREATE`, `DROP`, ou `ALTER` ne peuvent pas être directement exécutées à l'intérieur de blocs PL/SQL statiques. Elles nécessitent l'utilisation de SQL dynamique via `EXECUTE IMMEDIATE`.

**Exemple :**

sql

Copier le code

`CREATE TABLE test (a VARCHAR(5));  EXECUTE IMMEDIATE 'DROP TABLE test';`

### Prévention des Injections SQL

Les **injections SQL** surviennent lorsque des utilisateurs malveillants passent des commandes SQL dans des champs d'entrée. L'utilisation de SQL dynamique avec des variables liées (`bind variables`) aide à prévenir ces attaques.

**Exemple de prévention des injections SQL** :

sql

Copier le code

`CREATE OR REPLACE FUNCTION get_safe_num_of_employees (p_job VARCHAR2) RETURN NUMBER IS   v_query_str VARCHAR2(1000);   v_num_of_employees NUMBER; BEGIN   v_query_str := 'SELECT COUNT(*) FROM emp WHERE fonction = :bind_job';   EXECUTE IMMEDIATE v_query_str INTO v_num_of_employees USING p_job;   RETURN v_num_of_employees; END;`

En utilisant `:bind_job` comme variable liée, on empêche l’injection de commandes SQL malveillantes dans la requête.



### **Dictionnaire de Données et Méta-schéma**

Le **dictionnaire de données** est une composante essentielle du SGBD Oracle. Il s'agit d'un ensemble de tables et de vues internes qui stockent des informations sur la structure de la base de données, les objets qu'elle contient, les utilisateurs, les privilèges, les statistiques, et bien plus encore. Ces métadonnées sont cruciales pour le fonctionnement interne du SGBD et pour les administrateurs de bases de données (DBA).

#### **Contenu du Dictionnaire de Données**

- **Tables du Dictionnaire** : Stockent des informations détaillées sur tous les objets de la base de données, y compris les tables, les index, les vues, les séquences, les synonymes, les procédures, les fonctions, les déclencheurs, les paquets, etc.

- **Vues du Dictionnaire** : Oracle fournit des vues prédéfinies qui permettent d'accéder aux informations du dictionnaire de données de manière sécurisée et structurée. Ces vues sont classées en trois catégories principales :
  - **USER\_*** : Informations sur les objets possédés par l'utilisateur actuel.
  - **ALL\_*** : Informations sur tous les objets accessibles à l'utilisateur, qu'il en soit le propriétaire ou non.
  - **DBA\_*** : Informations sur tous les objets de la base de données. Ces vues sont généralement accessibles uniquement aux utilisateurs avec des privilèges d'administrateur.

#### **Exemples de Vues Importantes**

- **USER\_TABLES** : Contient des informations sur les tables appartenant à l'utilisateur.
- **USER\_TAB_COLUMNS** : Fournit des détails sur les colonnes des tables de l'utilisateur.
- **USER\_INDEXES** : Informations sur les index créés par l'utilisateur.
- **USER\_CONSTRAINTS** : Détails sur les contraintes définies sur les tables de l'utilisateur.
- **USER\_TRIGGERS** : Liste les déclencheurs (triggers) définis par l'utilisateur.
- **USER\_PROCEDURES** : Informations sur les procédures stockées et fonctions créées par l'utilisateur.

#### **Utilisation du Dictionnaire de Données**

Les DBA et les développeurs peuvent interroger ces vues pour :

- **Obtenir des métadonnées** : Comprendre la structure des tables, des vues, des index, etc.
- **Vérifier les contraintes et les relations** : Assurer l'intégrité référentielle et les règles métiers.
- **Analyser les performances** : Examiner les statistiques pour l'optimisation des requêtes.
- **Gérer les privilèges** : Vérifier les droits d'accès et les rôles des utilisateurs.
- **Audit et Sécurité** : Surveiller les modifications et les accès aux objets sensibles.

**Exemple d'interrogation du dictionnaire** :

```sql
SELECT table_name, tablespace_name, num_rows
FROM user_tables;
```

Cela permet d'obtenir la liste des tables de l'utilisateur, le tablespace associé et le nombre de lignes.

#### **Le Méta-schéma**

Le **méta-schéma** représente la structure qui décrit la base de données elle-même. Il s'agit essentiellement du schéma des métadonnées stockées dans le dictionnaire de données. En comprenant le méta-schéma, les administrateurs peuvent naviguer et manipuler les informations du dictionnaire de manière plus efficace.

#### **Paquetages Associés au Dictionnaire de Données**

- **DBMS_METADATA** : Ce paquetage permet d'extraire les métadonnées des objets de la base de données sous forme de scripts DDL (Data Definition Language). Il est très utile pour générer des scripts de création ou de recréation des objets.

  **Exemple d'utilisation** :

  ```sql
  SELECT DBMS_METADATA.GET_DDL('TABLE', 'EMP') FROM dual;
  ```

  Cela génère le script DDL pour recréer la table `EMP`.

- **DBMS_UTILITY** : Fournit des fonctions utilitaires pour des opérations générales, comme l'analyse du schéma, la gestion des dépendances, etc.

  **Exemple** :

  ```sql
  EXEC DBMS_UTILITY.ANALYZE_SCHEMA(user, 'COMPUTE');
  ```

  Cette commande met à jour les statistiques pour tous les objets du schéma de l'utilisateur, ce qui est essentiel pour l'optimiseur de requêtes.

#### **SQL Dynamique et le Dictionnaire de Données**

Le **SQL dynamique** permet d'exécuter des commandes SQL dont le contenu est déterminé au moment de l'exécution. Ceci est particulièrement utile lorsqu'il s'agit d'interagir avec le dictionnaire de données pour :

- **Générer des requêtes dynamiques** : En fonction des objets présents dans la base.
- **Automatiser des tâches d'administration** : Comme la création ou la suppression d'objets, la mise à jour des privilèges, etc.

**Exemple d'utilisation du SQL dynamique avec le dictionnaire** :

```sql
DECLARE
  v_sql VARCHAR2(1000);
BEGIN
  v_sql := 'CREATE TABLE backup_emp AS SELECT * FROM ' || user || '.EMP';
  EXECUTE IMMEDIATE v_sql;
END;
```

Ce code crée une copie de la table `EMP` pour l'utilisateur courant.

#### **Sécurité et Précautions**

- **Privilèges** : L'accès aux vues du dictionnaire est contrôlé par les privilèges de l'utilisateur. Les vues `DBA_` sont généralement réservées aux administrateurs.
- **Confidentialité** : Il est important de protéger les informations sensibles contenues dans le dictionnaire, comme les mots de passe ou les configurations critiques.
- **Gestion des Erreurs** : Lors de l'utilisation de SQL dynamique ou de l'interrogation du dictionnaire, il est essentiel de gérer les exceptions pour éviter les fuites d'informations ou les plantages de l'application.

#### **Audit et Supervision**

Les administrateurs peuvent utiliser le dictionnaire de données pour :

- **Surveiller les connexions des utilisateurs** : Via des vues comme `DBA_AUDIT_SESSION`.
- **Tracer les modifications** : Enregistrées dans des vues d'audit pour suivre les opérations DDL et DML.
- **Analyser les performances** : En examinant les statistiques d'utilisation des objets.

