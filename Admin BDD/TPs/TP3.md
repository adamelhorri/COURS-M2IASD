---
tags:
  - AdminBDD
  - tps
---

## PL/SQL (Séance sur l'Architecture Oracle)

### 1. Les Tables et les Blocs (ou Pages) Oracle
Le système Oracle alloue un certain nombre de blocs de données à chaque table en fonction des besoins de stockage. L'objectif est de trouver le meilleur compromis pour accéder rapidement aux enregistrements au sein des blocs. Nous allons explorer comment les blocs sont alloués en fonction des activités sur les tables et examiner l'organisation logique (tablespace, segment, extent) qui facilite l'accès aux blocs de données selon les besoins.

### 1.1 Exploiter les Vues USER_TABLES et DBA_TABLES
Les informations concernant l'organisation logique et physique des tables de votre schéma utilisateur sont accessibles via les vues système `user_tables` et `dba_tables`. Les attributs suivants fournissent des indications sur le nombre de blocs alloués à chaque table :

- **NUM_ROWS** : Nombre de lignes dans la table.
- **BLOCKS** : Nombre de blocs utilisés par la table.
- **EMPTY_BLOCKS** : Nombre de blocs alloués mais non utilisés.
- **AVG_SPACE** : Espace libre moyen dans les blocs utilisés.
- **AVG_ROW_LEN** : Longueur moyenne des lignes (tuples) en octets.

#### 1.1.1 Questions Préalables

1. **Espace de Table Cible**
   - **Question** : Vérifiez que toutes vos tables sont situées dans le même espace de tables (attribut `TABLESPACE_NAME` dans `USER_TABLES` et `DBA_TABLES`). Quel est le nom de ce tablespace ?
   - **Code SQL** :
   ```sql
   COLUMN tablespace_name FOR a30
   COLUMN table_name FOR a30
   SELECT tablespace_name, table_name FROM dba_tables;
   ```
   - **Explication** : Cette requête permet de lister les tables ainsi que leur tablespace associé, afin de vérifier qu'elles sont toutes regroupées dans le même tablespace, par exemple `DATA_ETUD`, qui stocke les données et les index des utilisateurs.

2. **Taille du Bloc Oracle**
   - **Question** : Quelle est la taille en octets du bloc Oracle (multiple de 1024 octets) ?
   - **Code SQL** :
   ```sql
   SHOW PARAMETER
   SHOW PARAMETER db_block_size
   SELECT name, value FROM v$parameter WHERE name LIKE 'db_block%';
   ```
   - **Explication** : Ces commandes permettent de vérifier la taille du bloc dans Oracle. La taille par défaut pour la majorité des tablespaces est de 8192 octets (environ 8 Ko), ce qui optimise les performances d'accès aux données.

3. **Nombre de Blocs Oracle pour une Table d’un Schéma**
   - **Question** : Combien de blocs de données sont utilisés pour contenir les données d'une de vos tables au choix ?
   - **Code SQL** :
   ```sql
   EXECUTE ANALYZE TABLE emp COMPUTE STATISTICS;
   SELECT table_name, num_rows * avg_row_len AS octetsRequis,
          blocks + empty_blocks AS nombreBlocs,
          (blocks + empty_blocks) * 8192 AS octetsAlloues
   FROM user_tables 
   WHERE table_name = 'EMP';
   ```
   - **Explication** : Cette requête permet de calculer le nombre total de blocs alloués à la table `EMP` ainsi que l'espace occupé en octets. La commande `ANALYZE` met à jour les statistiques sur la table pour obtenir des informations précises.

### 1.2 Mécanismes d'Allocation
Nous allons définir une table de test, puis l'alimenter à différents moments à l'aide d'une procédure PL/SQL fournie dans le fichier `remplissage.sql`.

- **Création de la Table `TEST`** :
```sql
CREATE TABLE test(num CHAR(3), commentaire CHAR(97));
```
- **Contrainte de Domaine** : Appliquez une contrainte pour que `num` contienne uniquement des nombres entiers compris entre 0 et 999.

#### 1.2.1 Question 1

- **Question** : Est-ce que la longueur des tuples de cette table est variable ? Relevez le nombre de blocs utilisés, alloués mais inutilisés, après :
  1. **La création de la table**
     - **Code SQL** :
     ```sql
     SELECT num_rows, blocks, empty_blocks, avg_space, avg_row_len 
     FROM user_tables 
     WHERE table_name = 'TEST';
     ```
     - **Résultat** :
     ```
     NUM_ROWS BLOCKS EMPTY_BLOCKS AVG_SPACE AVG_ROW_LEN
     --------- ---------- ------------ ---------- -----------
     0        0          0            0          0
     ```
     - **Explication** : Lors de la création de la table sur Oracle 10g, aucun tuple n'est présent, et 8 blocs sont alloués mais non utilisés. Sur Oracle 19, aucun bloc n'est alloué initialement.

  2. **L’insertion de 50 lignes**
     - **Code SQL** :
     ```sql
     ANALYZE TABLE test COMPUTE STATISTICS;
     SELECT num_rows, blocks, empty_blocks, avg_space, avg_row_len 
     FROM user_tables 
     WHERE table_name = 'TEST';
     ```
     - **Résultat** :
     ```
     NUM_ROWS BLOCKS EMPTY_BLOCKS AVG_SPACE AVG_ROW_LEN
     --------- ---------- ------------ ---------- -----------
     51       5          3            6982       105
     ```
     - **Explication** : Après l'insertion de 51 tuples, la taille est fixe (car `CHAR` est utilisé, entraînant une longueur fixe). 8 blocs sont alloués dont 3 vides, avec 6982 octets d'espace libre dans les blocs écrits.

  3. **L’insertion de 100 lignes supplémentaires**
     - **Code SQL** :
     ```sql
     ANALYZE TABLE test COMPUTE STATISTICS;
     SELECT num_rows, blocks, empty_blocks, avg_space, avg_row_len 
     FROM user_tables 
     WHERE table_name = 'TEST';
     ```
     - **Résultat** :
     ```
     NUM_ROWS BLOCKS EMPTY_BLOCKS AVG_SPACE AVG_ROW_LEN
     --------- ---------- ------------ ---------- -----------
     151      5          3            4840       105
     ```
     - **Explication** : Après l'insertion de 100 tuples supplémentaires, le nombre de blocs alloués reste à 8, mais l'espace libre dans les blocs écrits diminue à 4840 octets.

  4. **L’insertion de 100 lignes supplémentaires**
     - **Code SQL** :
     ```sql
     ANALYZE TABLE test COMPUTE STATISTICS;
     SELECT num_rows, blocks, empty_blocks, avg_space, avg_row_len 
     FROM user_tables 
     WHERE table_name = 'TEST';
     ```
     - **Résultat** :
     ```
     NUM_ROWS BLOCKS EMPTY_BLOCKS AVG_SPACE AVG_ROW_LEN
     --------- ---------- ------------ ---------- -----------
     250      5          3            2721       105
     ```
     - **Explication** : L'ajout de 100 tuples supplémentaires maintient 8 blocs alloués. L'espace libre dans les blocs diminue encore, atteignant 2721 octets.

  5. **L’insertion de 100 lignes supplémentaires**
     - **Code SQL** :
     ```sql
     ANALYZE TABLE test COMPUTE STATISTICS;
     SELECT num_rows, blocks, empty_blocks, avg_space, avg_row_len 
     FROM user_tables 
     WHERE table_name = 'TEST';
     ```
     - **Résultat** :
     ```
     NUM_ROWS BLOCKS EMPTY_BLOCKS AVG_SPACE AVG_ROW_LEN
     --------- ---------- ------------ ---------- -----------
     350      13         3            5191       105
     ```
     - **Explication** : Après avoir ajouté encore 100 tuples, 8 blocs sont alloués. Il y a maintenant un total de 13 blocs en raison de l'allocation de nouveaux blocs pour gérer l'augmentation des données, avec 5191 octets d'espace libre moyen.

**Comparaison de l'espace** :
L'espace de stockage pour 350 tuples de 105 octets chacun nécessite des blocs pour contenir ces données. Cependant, la gestion des blocs entraîne souvent une allocation d'espace plus importante que nécessaire pour éviter la réorganisation fréquente des données.

#### 1.2.2 Question 2

- **Question** : Supprimez un tiers des tuples de la table (par exemple, ceux dont le num est divisible par 3).
   - **Code SQL** :
   ```sql
   SELECT num FROM test WHERE MOD(num, 3) = 0;
   ```
   - **Explication** : Cette requête permet d'identifier les tuples à supprimer.

   - **Code SQL pour suppression** :
   ```sql
   DELETE FROM test WHERE MOD(num, 3) = 0;
   ```
   - **Valider et Tester** : Après suppression, réévaluez les statistiques.
   ```sql
   ANALYZE TABLE test COMPUTE STATISTICS;
   ```

   - **Explication** : La suppression d'un tiers des tuples met à jour les statistiques de la table. Vous constaterez que les blocs attribués restent, mais l'espace occup

é par les tuples est réduit à zéro.

   - **Code SQL pour supprimer tous les tuples** :
   ```sql
   DELETE FROM test;
   COMMIT;
   ```
   - **Explication** : En validant la suppression, les blocs restent attribués même après un commit, ce qui signifie que l'espace n'est pas immédiatement récupéré.

   - **Code SQL pour utiliser TRUNCATE** :
   ```sql
   TRUNCATE TABLE test;
   ```
   - **Explication** : La commande `TRUNCATE` libère immédiatement l'espace alloué et effectue un commit implicite, ce qui désalloue les blocs et laisse le segment et un extent.

### 1.3 Vues Système Associées à l’Organisation Logique
Vous allez réalimenter la table `TEST` en insérant 600 tuples. Ensuite, vous testerez les requêtes suivantes pour explorer l'organisation logique des tables :

- **Requête sur les Segments** :
```sql
SELECT tablespace_name, segment_name, blocks, bytes / 1024 AS Koctets, extents 
FROM user_segments 
ORDER BY segment_name;
```
- **Requête sur les Extents** :
```sql
SELECT segment_type, extent_id, bytes / 1024 AS enKo, blocks 
FROM user_extents 
WHERE segment_name = 'TEST';
```

**Questions à Répondre** :
- Quel est le nombre de segment(s) associé(s) à la table `TEST` ?
  - **Réponse** : Un seul segment qui a le même nom que la table.

- Est-ce que le segment associé à la table `TEST` organise logiquement l’information d’une autre table de la base ?
  - **Réponse** : Dans le cas habituel, la dépendance est forte entre un segment et une table, donc le segment ne contient que des blocs de la table.

- Quel est le nom du segment associé à la table `TEST` ?
  - **Réponse** : `TEST`.

- Quel est le type du segment associé à la table `TEST` ?
  - **Réponse** : `TABLE`.

- Quel est le nom de l’espace de tables dans lequel se range ce segment ?
  - **Réponse** : `DATA_ETUD`.

- De combien d’extensions (extents) se compose le segment ? Ce nombre d’extensions peut-il évoluer avec l’ajout de nouveaux tuples dans la table ?
  - **Réponse** : Deux extents de 8 blocs chacun. Oui, le nombre d’extents est fonction des besoins, un nouvel extent avec un nombre de blocs en multiple de 8 peut être ajouté.

---

Ce TP vous permet d'approfondir vos connaissances sur l'architecture des bases de données Oracle, en explorant les mécanismes d'allocation, les vues système, et en manipulant des données à l'aide de requêtes SQL. L'exploration des tables, des blocs, et de l'organisation logique vous aidera à mieux comprendre le fonctionnement interne d'Oracle.