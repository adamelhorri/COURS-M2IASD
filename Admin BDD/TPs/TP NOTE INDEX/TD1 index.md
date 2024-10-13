Voici la résolution numérotée selon les questions de l'énoncé de l'exercice 4 :

### Exercice 4 : Tailles table et index

## 4.1 Question 1

### Création de la table ABC

```sql
CREATE TABLE ABC (
    A NUMBER,
    B VARCHAR2(20),
    C VARCHAR2(20)
);
```

### Remplir la table

```sql
DECLARE
    v_b VARCHAR2(20);
    v_c VARCHAR2(20);
BEGIN
    FOR i IN 1..1000000 LOOP
        v_b := LPAD('B_' || i, 20, 'X');
        v_c := LPAD('C_' || i, 20, 'Y'); 
        -- On insère 20 caractères dans B et C, A est l'id i
        INSERT INTO ABC (A, B, C) VALUES (i, v_b, v_c);
    END LOOP;
    COMMIT;
END;
/
```

### Définir les statistiques

```sql
BEGIN
    DBMS_STATS.GATHER_TABLE_STATS(ownname => 'e20230003536', tabname => 'ABC');
END;
/
```

### Récupérer la taille des blocs et la taille moyenne des lignes

```sql
SELECT 
    TO_NUMBER(p.value) AS block_size,
    t.avg_row_len,
    t.pct_free
FROM 
    v$parameter p
    CROSS JOIN user_tables t
WHERE 
    p.name = 'db_block_size'
    AND t.table_name = 'ABC';
```

### Calculer la taille exploitable du bloc et le facteur de blocage

```sql
SELECT 
    TO_NUMBER(p.value) AS block_size,
    t.avg_row_len,
    t.pct_free,
    FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) AS block_size_exploitable,
    FLOOR(FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / t.avg_row_len) AS blocking_factor
FROM 
    v$parameter p
    CROSS JOIN user_tables t
WHERE 
    p.name = 'db_block_size'
    AND t.table_name = 'ABC';
```

### Récupérer le nombre total de tuples et le nombre de blocs

#### Récupérer le nombre total de tuples

```sql
SELECT COUNT(*) AS total_tuples FROM ABC;
```

#### Récupérer le nombre de blocs alloués à la table

```sql
SELECT blocks AS total_blocks FROM user_tables WHERE table_name = 'ABC';
```

## 4.2 Question 2

### Créer l'index

```sql
CREATE INDEX idx_abc_a ON ABC(A);
```

### Récupérer les statistiques de l'index B+

```sql
SELECT leaf_blocks, blevel 
FROM user_indexes 
WHERE index_name = 'IDX_ABC_A';
```

### Calculer l'ordre et la hauteur de l'arbre B+

```sql
SELECT 
    FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / 15 AS order_bplus_tree, 
    blevel + 1 AS tree_height
FROM 
    user_indexes ui 
    CROSS JOIN v$parameter p 
    CROSS JOIN user_tables t
WHERE 
    ui.index_name = 'IDX_ABC_A'
    AND p.name = 'db_block_size'
    AND t.table_name = 'ABC';
```

## 4.3 Question 3

### Plan d'exécution d'une requête (sélection sur clé)

#### Quand le tuple existe

```sql
EXPLAIN PLAN FOR SELECT * FROM ABC WHERE A = 500000;
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

#### Quand le tuple n'existe pas

```sql
EXPLAIN PLAN FOR SELECT * FROM ABC WHERE A = 2000000;
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

## 4.4 Question 4

### Plan d'exécution d'une requête (sélection sur un attribut non clé)

#### Quand le tuple existe

```sql
EXPLAIN PLAN FOR SELECT * FROM ABC WHERE B = 'B_500000';
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

#### Quand le tuple n'existe pas

```sql
EXPLAIN PLAN FOR SELECT * FROM ABC WHERE B = 'B_2000000';
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

## 4.5 Question 5

### Calculer le nombre de tuples supplémentaires pour augmenter la hauteur de l'arbre B+

```sql
SELECT 
    (FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / 15) * 1000000 - COUNT(*) AS additional_tuples_needed
FROM 
    user_indexes ui 
    CROSS JOIN v$parameter p 
    CROSS JOIN user_tables t
WHERE 
    ui.index_name = 'IDX_ABC_A'
    AND p.name = 'db_block_size'
    AND t.table_name = 'ABC';
```