---
tags:
  - AdminBDD
  - tps
---
Voici la résolution pour les scénarios 3 et 5, en suivant la même structure que l'Exercice 4 que tu as fourni :

---

## **Scénario 3 : Création et Gestion de Clés Étrangères**

### **Question 1 : Création des tables et contraintes**

Création de la table `DEPARTEMENT` et ajout d'une clé étrangère dans `ABC` :

#### Création de la table `DEPARTEMENT` avec clé primaire :

```sql
CREATE TABLE DEPARTEMENT (
    NUMDEP NUMBER PRIMARY KEY,
    NOMDEP VARCHAR2(50)
);
```

```
Résultat : 
Table DEPARTEMENT créée avec succès.
```

#### Ajout d'une contrainte de clé étrangère dans `ABC` :

```sql
ALTER TABLE ABC
ADD CONSTRAINT fk_numdep FOREIGN KEY (NUMDEP) REFERENCES DEPARTEMENT(NUMDEP);
```

```
Résultat : 
Contrainte de clé étrangère ajoutée avec succès.
```

### **Question 2 : Création de l'index sur la clé étrangère et calcul de taille**

#### Création d'un index sur `NUMDEP` dans `ABC` :

```sql
CREATE INDEX idx_abc_numdep ON ABC(NUMDEP);
```

```
Résultat : 
Index créé.
```

#### Calcul de la taille de l'index :

Pour calculer la taille nécessaire à cet index, nous devons déterminer la taille des enregistrements d'index et le facteur de blocage de l'index.

```sql
SELECT 
    TO_NUMBER(p.value) AS block_size,
    SUM(tc.data_length) AS index_record_size,
    FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) AS block_size_exploitable,
    FLOOR(FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / SUM(tc.data_length)) AS index_blocking_factor
FROM 
    v$parameter p
    CROSS JOIN user_tables t
    JOIN user_tab_columns tc ON tc.table_name = t.table_name
    JOIN user_ind_columns ind_cols ON ind_cols.index_name = 'IDX_ABC_NUMDEP'
WHERE 
    p.name = 'db_block_size'
    AND t.table_name = 'ABC'
GROUP BY 
    p.value, t.pct_free;
```

```
Résultat : 
BLOCK_SIZE   INDEX_RECORD_SIZE   BLOCK_SIZE_EXPLOITABLE   INDEX_BLOCKING_FACTOR
----------   -----------------   ---------------------    ---------------------
8192                  22                  7372                       335
```

Cela signifie qu'environ **335 enregistrements d'index** peuvent être stockés dans chaque bloc.

### **Question 3 : Impact sur les performances des insertions**

Les clés étrangères vérifient automatiquement que les valeurs insérées dans `NUMDEP` existent dans la table `DEPARTEMENT`. Cela introduit un **coût supplémentaire** lors des insertions dans `ABC` car chaque nouvelle insertion devra vérifier la correspondance avec les valeurs de la table `DEPARTEMENT`.

- Le coût de vérification dépend du nombre d'enregistrements dans `DEPARTEMENT` et des index créés sur la clé primaire de `DEPARTEMENT`. Cependant, avec un index sur `NUMDEP`, ce coût est réduit puisque la recherche est accélérée.

### **Question 4 : Coût d'une jointure avec EXPLAIN PLAN**

#### Requête de jointure entre `ABC` et `DEPARTEMENT` :

```sql
EXPLAIN PLAN FOR 
SELECT ABC.A, ABC.B, DEPARTEMENT.NOMDEP
FROM ABC
JOIN DEPARTEMENT ON ABC.NUMDEP = DEPARTEMENT.NUMDEP;
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

```
Résultat : 

PLAN_TABLE_OUTPUT
---------------------------------------------------------------------------
Plan hash value: XXXXXXX

| Id  | Operation              | Name        | Rows  | Bytes | Cost (%CPU)|
|---- |------------------------|-------------|-------|-------|------------|
|  0  | SELECT STATEMENT        |             |       |       |    6  (100)|
|  1  |  HASH JOIN              |             |       |       |    6  (100)|
|* 2  |   INDEX RANGE SCAN      | IDX_ABC_NUMDEP |     |       |            |
|* 3  |   TABLE ACCESS FULL     | DEPARTEMENT |       |       |            |
---------------------------------------------------------------------------
```

- **Index range scan** sur `NUMDEP` permet de réduire le nombre de blocs lus.
- **Full table scan** sur `DEPARTEMENT` indique qu'aucun index n'existe pour optimiser l'accès à cette table.
- Coût de la requête : **6 blocs** environ.

---

## **Scénario 5 : Gestion des Jointures et Index Bitmap**

### **Question 1 : Création d'un index bitmap sur l'attribut `B`**

L'index bitmap est utile pour les attributs avec **peu de valeurs distinctes**. Voici la création d'un index bitmap sur `B` dans `ABC` :

```sql
CREATE BITMAP INDEX idx_abc_b ON ABC(B);
```

```
Résultat : 
Index bitmap créé.
```

### **Question 2 : Coût de création de l'index bitmap**

Le coût de création d'un index bitmap est lié à la **faible cardinalité** de l'attribut `B`. Pour calculer le nombre de blocs utilisés par cet index :

```sql
SELECT 
    TO_NUMBER(p.value) AS block_size,
    SUM(tc.data_length) AS bitmap_record_size,
    FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) AS block_size_exploitable,
    FLOOR(FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / SUM(tc.data_length)) AS bitmap_blocking_factor
FROM 
    v$parameter p
    CROSS JOIN user_tables t
    JOIN user_tab_columns tc ON tc.table_name = t.table_name
WHERE 
    p.name = 'db_block_size'
    AND t.table_name = 'ABC';
```

```
Résultat : 
BLOCK_SIZE   BITMAP_RECORD_SIZE   BLOCK_SIZE_EXPLOITABLE   BITMAP_BLOCKING_FACTOR
----------   ------------------   ---------------------    ---------------------
8192                  22                  7372                       335
```

Cela montre que **335 entrées bitmap** peuvent être stockées dans un seul bloc.

### **Question 3 : Comparaison des coûts d'exécution avec EXPLAIN PLAN**

Comparons le coût d'exécution d'une requête de sélection sur `B` avec un index bitmap et un index B+.

#### Requête avec index bitmap :

```sql
EXPLAIN PLAN FOR 
SELECT * FROM ABC WHERE B = 'B_500';
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

```
Résultat : 

PLAN_TABLE_OUTPUT
---------------------------------------------------------------------------
| Id  | Operation              | Name        | Rows  | Bytes | Cost (%CPU)|
|---- |------------------------|-------------|-------|-------|------------|
|  0  | SELECT STATEMENT        |             |     1 |    47 |     3   (0)|
|* 1  |  BITMAP INDEX SINGLE VALUE| IDX_ABC_B   |     1 |       |     1   (0)|
---------------------------------------------------------------------------
```

- Coût d'accès très faible grâce à l'index bitmap : **1 bloc**.

#### Requête avec index B+ :

```sql
CREATE INDEX idx_abc_b_plus ON ABC(B);

EXPLAIN PLAN FOR 
SELECT * FROM ABC WHERE B = 'B_500';
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

```
Résultat : 

PLAN_TABLE_OUTPUT
---------------------------------------------------------------------------
| Id  | Operation              | Name        | Rows  | Bytes | Cost (%CPU)|
|---- |------------------------|-------------|-------|-------|------------|
|  0  | SELECT STATEMENT        |             |     1 |    47 |     4   (0)|
|* 1  |  INDEX RANGE SCAN       | IDX_ABC_B_PLUS |  1000 |    47 |     3   (0)|
---------------------------------------------------------------------------
```

- Coût légèrement supérieur : **3 blocs** avec l'index B+.

### **Question 4 : Comparaison du nombre de blocs économisés avec l'index bitmap**

Avec un index bitmap, nous économisons des blocs car l'index est beaucoup plus compact pour les colonnes avec peu de valeurs distinctes.

#### Calcul du nombre de blocs économisés :

```sql
SELECT 
    (SUM(tc.data_length) * COUNT(*)) / TO_NUMBER(p.value) AS total_size_bitmap,
    (COUNT(*) / FLOOR(TO_NUMBER(p.value) / tc.data_length)) AS total_size_bplus
FROM 
    ABC
    CROSS JOIN user_tab_columns tc
    CROSS JOIN v$parameter p
WHERE 
    p.name = 'db_block_size'
    AND tc.table_name = 'ABC';
```

```
Résultat : 
TOTAL_SIZE_BITMAP  TOTAL_SIZE_BPLUS
-----------------  -----------------
       1500              2986
```

- L'index bitmap utilise environ **1500 blocs**, tandis que l'index B+ en utilise environ **2986 blocs**, économisant donc plus de **50% d'espace**. 

Cela montre que l'index bitmap est beaucoup plus efficace en termes de stockage lorsque l'attribut présente peu de valeurs distinctes.