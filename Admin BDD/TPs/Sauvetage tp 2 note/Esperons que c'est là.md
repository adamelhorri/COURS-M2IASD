
# CHEAT SHEET COMPLET – INDEXES, OPTIMISATION, TRANSACTIONS

### Base de Données de Référence

- **Schéma :** GANDALF
- **Tables :** DEPARTEMENT(NUMDEP PK, NOMDEP), COMMUNE(CODEINSEE PK, NOMCOMMAJ, NUMDEP, FONCTION, FK(NUMDEP)->DEPARTEMENT(NUMDEP))
- **Exemples tuples :**
    - DEPARTEMENT : (34, 'Hérault'), (30, 'Gard')
    - COMMUNE : ('34172','MONTPELLIER',34,'ouvrier'), ('34129','LATTES',34,'ouvrier'), ('30123','NIMES',30,'technicien'), ('34100','SETE',34,'ouvrier')
- **Autres objets** : Table PRVOIR(valeur integer PK), Index NUMDEP_IDX sur COMMUNE(NUMDEP).

---

## I. INDEXES

#### Création et Gestion des Index

- **Créer un index** :
    
    ```sql
    CREATE INDEX GANDALF.NUMDEP_IDX ON GANDALF.COMMUNE(NUMDEP);
    ```
    
- **Index unique** :
    
    ```sql
    CREATE UNIQUE INDEX GANDALF.COMMUNE_PK ON GANDALF.COMMUNE(CODEINSEE);
    ```
    
- **Supprimer un index** :
    
    ```sql
    DROP INDEX GANDALF.NUMDEP_IDX;
    ```
    

#### Informations sur les Index (user_indexes, index_stats)

- **Lister tous les index de l’utilisateur** :
    
    ```sql
    SELECT index_name, table_name, uniqueness, blevel, distinct_keys, leaf_blocks, clustering_factor
    FROM user_indexes;
    ```
    
    - **blevel** : profondeur de l’index - 1 (0 = pas de branches, juste racine=feuilles)
    - **distinct_keys** : nombre de valeurs distinctes d’index
    - **leaf_blocks** : blocs feuilles
    - **clustering_factor** : mesure du degré d’ordre des données par rapport à l’index
- **Filtrer par table** :
    
    ```sql
    SELECT index_name, blevel, distinct_keys FROM user_indexes WHERE table_name='COMMUNE';
    ```
    
- **Analyser un index (collecter stats internes)** :
    
    ```sql
    ANALYZE INDEX GANDALF.COMMUNE_PK VALIDATE STRUCTURE;
    SELECT name, btree_space, used_space, pct_used, lf_rows, br_rows, height, most_repeated_key
    FROM index_stats;
    ```
    
    - **height = blevel + 1**
    - **lf_rows** : nb de lignes dans les feuilles (clé, rowid)
    - **br_rows** : nb de lignes dans les branches
    - **most_repeated_key** : fréquence de la clé la plus fréquente
- **Comparez la sélectivité de l’index** :
    
    - Plus distinct_keys est grand, plus la sélectivité est bonne
    - Si most_repeated_key est élevé, une valeur se répète beaucoup, réduisant la sélectivité

#### Autres Opérations sur les Index

- **Reconstruire un index** :
    
    ```sql
    ALTER INDEX GANDALF.NUMDEP_IDX REBUILD;
    ```
    
- **Collecter les stats pour l’optimiseur** :
    
    ```sql
    ANALYZE INDEX GANDALF.NUMDEP_IDX COMPUTE STATISTICS;
    ```
    

---

## II. OPTIMISATION

#### Statistiques sur Tables, Blocs, Distributions

- **Collecter stats sur une table** :
    
    ```sql
    ANALYZE TABLE GANDALF.COMMUNE COMPUTE STATISTICS;
    ```
    
- **Afficher stats table (user_tables)** :
    
    ```sql
    SELECT table_name, blocks, num_rows, avg_row_len, pct_free, last_analyzed
    FROM user_tables
    WHERE table_name='COMMUNE';
    ```
    
    - **blocks** : nb de blocs alloués
    - **num_rows** : nb de tuples estimés
    - **avg_row_len** : taille moyenne d’un tuple (en octets)
    - **pct_free** : pourcentage d’espace libre dans chaque bloc
- **Détailler les colonnes (user_tab_columns)** :
    
    ```sql
    SELECT column_name, num_distinct, density, num_nulls, avg_col_len
    FROM user_tab_columns
    WHERE table_name='COMMUNE';
    ```
    
    - **num_distinct** : nb distinct de valeurs par colonne
    - **avg_col_len** : taille moyenne de la colonne
    - Ces infos aident à comprendre la distribution des données.

#### Informations sur la Structure Physique (dba_objects, dba_segments, v$bh)

- **Identifier object_id d’une table** :
    
    ```sql
    SELECT owner, object_name, object_id, data_object_id
    FROM dba_objects
    WHERE owner='GANDALF' AND object_name='COMMUNE';
    ```
    
- **Lister les blocs de la table en mémoire (v$bh)** :
    
    ```sql
    SELECT o.owner, o.object_name, bh.file#, bh.block#, bh.class#, bh.status, bh.dirty
    FROM v$bh bh, dba_objects o
    WHERE bh.obj=o.data_object_id AND o.owner='GANDALF' AND o.object_name='COMMUNE';
    ```
    
    - **class#=1** : data block, **class#=4** : segment header
    - **dirty='Y'** : bloc modifié en mémoire, pas encore écrit sur disque
- **Trouver le segment header (dba_segments)** :
    
    ```sql
    SELECT segment_name, header_file, header_block
    FROM dba_segments
    WHERE owner='GANDALF' AND segment_name='COMMUNE';
    ```
    

#### Localisation Physique d’un Tuple (dbms_rowid)

- **Décoder un ROWID** :
    
    ```sql
    SELECT rowid,
           dbms_rowid.rowid_block_number(rowid) AS block_no,
           dbms_rowid.rowid_row_number(rowid) AS row_no
    FROM GANDALF.COMMUNE
    WHERE codeinsee='34172';
    ```
    
- **Reconstituer ROWID à partir (obj#, file#, block#, row#)** :
    
    ```sql
    SELECT dbms_rowid.rowid_create(1,<data_object_id>,<file#>,<block#>,<row#>) AS reconst_rowid
    FROM dual;
    
    SELECT * FROM GANDALF.COMMUNE WHERE rowid='<reconst_rowid>';
    ```
    

#### Facteur de Blocage, Cohérence avec les Stats

- **Calcul facteur de blocage** :
    
    - Taille bloc standard = 8K (vérifier la config)
    - Espace disponible = (8K * (1 - pct_free/100))
    - Facteur blocage ≈ Espace disponible / avg_row_len
- **Vérifier cohérence** :
    
    - Count du nombre de tuples dans un bloc donné via dbms_rowid.rowid_block_number(rowid) sur un échantillon.
    - Comparer le nombre réel au nombre théorique.

---

## III. TRANSACTIONS

#### Modes d’Isolation (READ ONLY, SERIALIZABLE)

- **Transaction READ ONLY** :
    
    ```sql
    SET TRANSACTION READ ONLY;
    SELECT * FROM COMMUNE; -- OK
    UPDATE COMMUNE SET fonction='X' WHERE fonction='Y'; -- interdit, car READ ONLY
    ROLLBACK;
    ```
    
- **Repasser en READ WRITE via DDL** :
    
    ```sql
    SET TRANSACTION READ ONLY;
    CREATE TABLE GANDALF.TestTab (id NUMBER PRIMARY KEY);
    -- Commit implicite, transaction repasse en lecture/écriture
    UPDATE COMMUNE SET fonction='commerciale' WHERE fonction='commercial';
    ROLLBACK;
    ```
    
- **Mode SERIALIZABLE** :
    
    ```sql
    SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
    SELECT * FROM GANDALF.COMMUNE;
    -- Pas de modifications concurrentes visibles tant que la transaction n’est pas finie.
    ```
    

#### Effet des DDL sur les Transactions (Commit implicite)

- **Toute DDL (CREATE, DROP, ALTER...) = Commit implicite** :
    
    ```sql
    INSERT INTO PRVOIR VALUES(1);
    CREATE TABLE GANDALF.EncorePrVoir (valeur INTEGER PRIMARY KEY);
    -- Commit implicite avant et après CREATE TABLE
    ROLLBACK; -- aucune annulation de l’INSERT, déjà validé.
    SELECT * FROM PRVOIR; -- voit la ligne 1
    ```
    
- **DDL avec erreur** :
    
    ```sql
    INSERT INTO PRVOIR VALUES(2);
    CREATE TABLE GANDALF.EncorePrVoir (valeur integre); -- erreur
    -- L’INSERT est validé avant l’erreur.
    ROLLBACK;
    SELECT * FROM PRVOIR; -- la ligne 2 est validée
    ```
    

#### Verrous et Blocages (v$lock, v$session, v$locked_object, dba_objects)

- **Lister les verrous** :
    
    ```sql
    SELECT sid, type, lmode, request, block FROM v$lock;
    ```
    
    - **block=1** : cette session bloque d’autres sessions
- **Associer verrous et objets verrouillés** :
    
    ```sql
    SELECT s.username, s.sid, o.object_name, o.object_type, l.locked_mode
    FROM v$locked_object l
    JOIN dba_objects o ON l.object_id=o.object_id
    JOIN v$session s ON l.session_id=s.sid;
    ```
    
- **Qui bloque qui ?** :
    
    ```sql
    SELECT a.sid AS blockingSession, b.sid AS blockedSession, b.request
    FROM v$lock a, v$lock b
    WHERE a.sid!=b.sid AND a.id1=b.id1 AND a.id2=b.id2 AND b.request>0 AND a.block=1;
    ```
    
- **Enrichir avec nom d’objet et type de verrou** :
    
    ```sql
    SELECT s1.username AS blocking_user, s2.username AS blocked_user,
           o.object_name, a.type AS blocking_lock_type, b.type AS blocked_lock_type, b.request
    FROM v$lock a, v$lock b, v$session s1, v$session s2,
         v$locked_object lo, dba_objects o
    WHERE a.sid!=b.sid AND a.id1=b.id1 AND a.id2=b.id2 
      AND b.request>0 AND a.block=1
      AND s1.sid=a.sid AND s2.sid=b.sid 
      AND lo.session_id=a.sid AND lo.object_id=o.object_id;
    ```
    
- **Transactions en cours (v$transaction)** :
    
    ```sql
    SELECT addr, xidusn, xidslot, xidsqn, start_time FROM v$transaction;
    ```
    

#### Cas Concurrents

- **Insertion non validée non visible par autre session** :
    
    - user2 insère une ligne, pas de commit
    - user1 ne voit rien.
    - Dès que user2 commit, user1 voit la nouvelle ligne.
- **Mises à jour concurrentes (blocage)** :
    
    ```sql
    -- user2
    UPDATE emp SET salaire=1000 WHERE num=101; -- verrou exclusif sur cette ligne
    -- user1
    UPDATE user2.emp SET salaire=2000 WHERE num=101; -- bloqué en attente du COMMIT user2
    -- user2
    COMMIT;
    -- user1 se débloque et effectue sa mise à jour.
    ```
    
- **Mode SERIALIZABLE et ORA-08177** :  
    Si user1 lit une ligne en début de transaction, user2 la modifie et commit ensuite, user1 tentera une mise à jour et recevra ORA-08177 (impossible de sérialiser l’accès) s’il y a conflit.
    
- **Interblocage (deadlock)** :  
    Deux sessions se bloquent mutuellement. Oracle détecte et émet ORA-00060, annulant une des transactions.
    

#### Droits et Privilèges

- **Lister privilèges sur les tables du schéma GANDALF** :
    
    ```sql
    SELECT grantee, privilege, owner, table_name FROM dba_tab_privs WHERE owner='GANDALF';
    ```
    
- **Donner droits** :
    
    ```sql
    GRANT ALL ON emp TO e20200010573;
    ```
    

---

### Conseils, Mots-Clés et Points Clés pour Répondre aux Questions

- **INDEXES** :  
    _CTRL+F_ : `user_indexes` pour hauteur, distinct_keys, blevel.  
    _CTRL+F_ : `ANALYZE INDEX` pour index_stats et détails internes.  
    _CTRL+F_ : `most_repeated_key` pour évaluer la sélectivité.
    
- **OPTIMISATION** :  
    _CTRL+F_ : `ANALYZE TABLE` pour calculer les stats.  
    _CTRL+F_ : `user_tables`, `user_tab_columns` pour nb de blocs, avg_row_len, distributions.  
    _CTRL+F_ : `v$bh` pour voir les blocs en cache.  
    _CTRL+F_ : `dbms_rowid` pour localiser physiquement un tuple.
    
- **TRANSACTIONS** :  
    _CTRL+F_ : `SET TRANSACTION READ ONLY` ou `SERIALIZABLE` pour modes d’isolation.  
    _CTRL+F_ : `v$lock`, `v$locked_object`, `v$session` pour identifier les verrous, blocages, sessions bloquées.  
    _CTRL+F_ : `ORA-08177` pour serializable conflicts.  
    _CTRL+F_ : `DDL` pour comprendre le commit implicite.  
    _CTRL+F_ : `deadlock`, `ORA-00060` pour interblocages.
    
- **Méthodologie** :
    
    - Identifier la question (index, table stats, verrous, isolation)
    - Sélectionner la vue ou la commande adéquate (ANALYZE, SELECT sur v$lock, etc.)
    - Exécuter la requête et interpréter les résultats : block=1 signifie session bloquante, blevel=0 index plat, etc.
- **Exemples de Questions et Réponses Express** :
    
    - _Combien de blocs la table occupe ?_
        
        ```sql
        SELECT blocks FROM user_tables WHERE table_name='COMMUNE';
        ```
        
    - _Qui bloque qui ?_
        
        ```sql
        SELECT ... FROM v$lock ... WHERE block=1;
        ```
        
    - _Quelle est la hauteur de l’index COMMUNE_PK ?_
        
        ```sql
        SELECT blevel FROM user_indexes WHERE index_name='COMMUNE_PK';
        -- hauteur = blevel+1
        ```
        
    - _Comment voir les transactions actives ?_
        
        ```sql
        SELECT * FROM v$transaction;
        ```
        
