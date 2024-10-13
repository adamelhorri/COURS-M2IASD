---

tags:

- AdminBDD

- tps

---
---

tags:

- AdminBDD

- tps

---

### Exercice 4 : Tailles table et index

  

## 4.1 Question 1

  

### Création de la table ABC

#### petit drop :

  

```sql

DROP TABLE ABC;

```

#### Creation

```sql

CREATE TABLE ABC (

A NUMBER,

B VARCHAR2(20),

C VARCHAR2(20)

);

```

  

```

Result:

Table creee.

  

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

  

```

Result:

Procedure PL/SQL terminee avec succes.

  

```

#### Tester la taille

```sql

select count(*) from ABC;

```

  

```

Result:

  

COUNT(*)

----------

1000000

  

```

### Définir les statistiques

  

```sql

BEGIN

DBMS_STATS.GATHER_TABLE_STATS(ownname => 'e20230003536', tabname => 'ABC');

END;

/

```

  

```

Result:

Procedure PL/SQL terminee avec succes.

  

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

  

```

Result:

BLOCK_SIZE AVG_ROW_LEN PCT_FREE

---------- ----------- ----------

8192 47 10

  

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

  

```

Result:

  

BLOCK_SIZE AVG_ROW_LEN PCT_FREE BLOCK_SIZE_EXPLOITABLE BLOCKING_FACTOR

---------- ----------- ---------- ---------------------- ---------------

8192 47 10 7372 156

```

### Récupérer le nombre total de tuples et le nombre de blocs

  

#### Récupérer le nombre total de tuples

  

```sql

SELECT COUNT(*) AS total_tuples FROM ABC;

```

  

```

Result:

TOTAL_TUPLES

------------

1000000

  

```

#### Récupérer le nombre de blocs alloués à la table

  

```sql

SELECT blocks AS total_blocks FROM user_tables WHERE table_name = 'ABC';

```

  

```

Result:

  

TOTAL_BLOCKS

------------

7300

```

## 4.2 Question 2

  

### Créer l'index

  

```sql

CREATE INDEX idx_abc_a ON ABC(A);

```

  

```

Result:

  

Index cree.

```

### Récupérer les statistiques de l'index B+

  

```sql

SELECT leaf_blocks, blevel

FROM user_indexes

WHERE index_name = 'IDX_ABC_A';

```

  

```

Result:

  

LEAF_BLOCKS BLEVEL

----------- ----------

2226           2

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

  

```

Result:

  

ORDER_BPLUS_TREE TREE_HEIGHT

---------------- -----------

491,466667 3

```

  

## 4.3 Question 3 : Calcul du facteur de blocage, taille de l'index, hauteur et ordre de l'arbre B+

  

#### Calcul du facteur de blocage pour les enregistrements d'index

  

```sql

SELECT

TO_NUMBER(p.value) AS block_size,

t.pct_free,

SUM(tc.data_length) AS index_record_size,

FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) AS block_size_exploitable,

FLOOR(FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / SUM(tc.data_length)) AS index_blocking_factor

FROM

v$parameter p

CROSS JOIN user_tables t

JOIN user_indexes ui ON ui.table_name = t.table_name

JOIN user_ind_columns ind_cols ON ind_cols.index_name = ui.index_name

JOIN user_tab_columns tc ON tc.table_name = t.table_name AND tc.column_name = ind_cols.column_name

WHERE

p.name = 'db_block_size'

AND t.table_name = 'ABC'

AND ui.index_name = 'IDX_ABC_A'

GROUP BY

p.value, t.pct_free;

```

  

```

Result:

  

BLOCK_SIZE PCT_FREE INDEX_RECORD_SIZE BLOCK_SIZE_EXPLOITABLE INDEX_BLOCKING_FACTOR

---------- ---------- ----------------- ---------------------- ---------------------

8192        10       22                  7372                   335

```

#### Calcul du nombre de blocs nécessaires au stockage de l'index

  

##### Calcun taille colonnes

  

```sql

SELECT

SUM(tc.data_length) AS index_record_size

FROM

user_tab_columns tc

JOIN user_ind_columns ind_cols ON tc.table_name = ind_cols.table_name AND tc.column_name = ind_cols.column_name

WHERE

ind_cols.index_name = 'IDX_ABC_A'

AND tc.table_name = 'ABC';

  

```

  

```

result:

INDEX_RECORD_SIZE

-----------------

22

  

```

  

```sql

SELECT

CEIL(COUNT(*) / FLOOR(FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / SUM(tc.data_length))) AS total_index_blocks,

CEIL(COUNT(*) / FLOOR(FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / SUM(tc.data_length))) * TO_NUMBER(p.value) AS total_index_size

FROM

ABC

CROSS JOIN v$parameter p

CROSS JOIN user_tables t

JOIN user_ind_columns ind_cols ON ind_cols.index_name = 'IDX_ABC_A'

JOIN user_tab_columns tc ON tc.table_name = t.table_name AND tc.column_name = ind_cols.column_name

WHERE

p.name = 'db_block_size'

AND t.table_name = 'ABC'

GROUP BY

p.value, t.pct_free;

```

  

```

Result:

TOTAL_INDEX_BLOCKS TOTAL_INDEX_SIZE

------------------ ----------------

2986 24461312

  

```

#### Calcul de l'ordre et de la hauteur de l'arbre B+

  

```sql

SELECT

FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / SUM(tc.data_length) AS order_bplus_tree,

blevel + 1 AS tree_height

FROM

user_indexes ui

CROSS JOIN v$parameter p

CROSS JOIN user_tables t

JOIN user_ind_columns ind_cols ON ind_cols.index_name = ui.index_name

JOIN user_tab_columns tc ON tc.table_name = t.table_name AND tc.column_name = ind_cols.column_name

WHERE

ui.index_name = 'IDX_ABC_A'

AND p.name = 'db_block_size'

AND t.table_name = 'ABC'

GROUP BY

p.value, t.pct_free, ui.blevel;

```

  

```

Result:

ORDER_BPLUS_TREE TREE_HEIGHT

---------------- -----------

335,090909 3

  

```

  

## 4.4 Question 4 : Coût de la consultation sur clé (avec un index B+)

  

#### Coût en nombre de blocs pour obtenir un tuple existant

  

```sql

EXPLAIN PLAN FOR SELECT * FROM ABC WHERE A = 500000;

SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);

```

  

```

Result:

  

Explicite.

  

SQL>

PLAN_TABLE_OUTPUT

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Plan hash value: 519569139

  

-------------------------------------------------------------------------------------------------

| Id | Operation | Name | Rows | Bytes | Cost (%CPU)| Time |

-------------------------------------------------------------------------------------------------

| 0 | SELECT STATEMENT | | 1 | 47 | 4 (0)| 00:00:01 |

| 1 | TABLE ACCESS BY INDEX ROWID BATCHED| ABC | 1 | 47 | 4 (0)| 00:00:01 |

|* 2 | INDEX RANGE SCAN | IDX_ABC_A | 1 | | 3 (0)| 00:00:01 |

-------------------------------------------------------------------------------------------------

  

Predicate Information (identified by operation id):

  

PLAN_TABLE_OUTPUT

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

---------------------------------------------------

  

2 - access("A"=500000)

  

14 lignes selectionnees.

  

```

  

#### Coût en nombre de blocs pour un tuple inexistant

  

```sql

EXPLAIN PLAN FOR SELECT * FROM ABC WHERE A = 2000000;

SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);

```

  

```

Result:

  

Explicite.

  

SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);

  

PLAN_TABLE_OUTPUT

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Plan hash value: 519569139

  

-------------------------------------------------------------------------------------------------

| Id | Operation | Name | Rows | Bytes | Cost (%CPU)| Time |

-------------------------------------------------------------------------------------------------

| 0 | SELECT STATEMENT | | 1 | 47 | 4 (0)| 00:00:01 |

| 1 | TABLE ACCESS BY INDEX ROWID BATCHED| ABC | 1 | 47 | 4 (0)| 00:00:01 |

|* 2 | INDEX RANGE SCAN | IDX_ABC_A | 1 | | 3 (0)| 00:00:01 |

-------------------------------------------------------------------------------------------------

  

Predicate Information (identified by operation id):

  

PLAN_TABLE_OUTPUT

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

---------------------------------------------------

  

2 - access("A"=2000000)

  

14 lignes selectionnees.

  

```

  
  
  

## 4.5 Question 5 : Calcul du nombre de tuples supplémentaires pour augmenter la hauteur de l'arbre B+

  

```sql

SELECT

(FLOOR(TO_NUMBER(p.value) * (100 - t.pct_free) / 100) / SUM(tc.data_length)) * COUNT(*) AS additional_tuples_needed

FROM

ABC

CROSS JOIN v$parameter p

CROSS JOIN user_tables t

JOIN user_indexes ui ON ui.table_name = t.table_name

JOIN user_ind_columns ind_cols ON ind_cols.index_name = ui.index_name

JOIN user_tab_columns tc ON tc.table_name = t.table_name AND tc.column_name = ind_cols.column_name

WHERE

p.name = 'db_block_size'

AND t.table_name = 'ABC'

AND ui.index_name = 'IDX_ABC_A'

GROUP BY

p.value, t.pct_free;

```

  

```

Result:

  

ADDITIONAL_TUPLES_NEEDED

------------------------

335,090909

```