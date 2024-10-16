---
tags:
  - AdminBDD
  - tps
  - annales
---
Exercice 1 (13 points) :
Expliquer tous vos résultats (vues du dictionnaire et requêtes SQL exploitées)

-- Pour la table (1 point par question)

1. donner la taille d'un tuple de table et donner la cardinalité de la table (nombre de tuples)
   1.1 . Nombre de tuples : 1000000

   ```sql
   select count(*) from ABC;
   ```

   resultat:

   ```
   COUNT(*)
   ----------
   1000000
   ```

   1.2.Taille d'un tuple : 50 octets

   ```sql
   SELECT avg_row_len 
   FROM USER_TABLES WHERE table_name = 'ABC';
   ```

   resultat:

   ```
   AVG_ROW_LEN
   -----------
           50
   ```
2. donner le nombre total de blocs alloués à la table ABC en indiquant le nombre de blocs ayant fait l'objet d'écriture et le nombre de blocs vides
   2.1. Nombre de blocs utilisés: 7300
   requête :

   ```sql
   SELECT blocks AS total_blocks FROM user_tables WHERE table_name = 'ABC';
   ```

   resultat:

   ```
   TOTAL_BLOCKS
   ------------
           7300
   ```

   2.2. Nombre de blocs vides : 124
   requete

   ```sql
   SELECT empty_blocks 
   FROM USER_TABLES WHERE table_name = 'ABC';
   ```

   resultat:

   ```
   EMPTY_BLOCKS
   ------------
           124
   ```
3. donner le nombre d'extents (et leur taille en blocs) compris dans le segment de table associé à la table ABC
   3.1.Nombre d'extents : 73
   requète:

   ```sql
   select count(EXTENT_ID) from dba_extents where segment_name = 'ABC' and OWNER= 'E20230003536';
   ```

   resultat:

   ```
   COUNT(EXTENT_ID)
   ----------------
               73
   ```

   3.2. Taille des extents :16 extents de 65536 octets et 57 extents de 1048576 octets
   requète:

   ```sql
   select count(EXTENT_ID),BYTES from dba_extents where segment_name = 'ABC' and OWNER= 'E20230003536' group by BYTES;
   ```

   resultat:

   ```
   COUNT(EXTENT_ID)      BYTES
   ---------------- ----------
               16      65536
               57    1048576
   ```
4. donner la taille en octets de l'espace de stockage qui a été réservé pour la table ABC
   4.1. Taille allouée à la table : 60,817408 mb (60817408 octets)
   requète:

   ```sql
   select sum(bytes) from dba_extents where segment_name = 'ABC' and OWNER= 'E20230003536';
   ```
5. donnez le nom d'une vue consultable pour connaître le nombre de blocs parcourus lors de l'exécution d'une requête (utilisant ou non l'index), ainsi qu'un exemple de requête associée à cette vue ?
   la vue v$bh
   exemple :

   ```sql
        select count(*) from ((select block_id from dba_extents  where segment_name = 'ABC') minus (select block# from v$bh));
   ```

--Pour l'index (1 point par question)

1. comment savoir si l'index ABC_PK est unique et dense ?
   l'index est unique car basé sur la clé primaire A , il est également dense car il pointe directement sur les enreistrement individuels
   requète:

   ```sql
   select lf_rows from index_stats where name='ABC_PK';
   ```

   resultat

   ```
   LF_ROWS
   ----------
   1000000
   ```
2. donner la taille en octets d'un tuple de branche d'index et la taille d'un tuple de feuille d'index (en expliquant la différence de taille)
   taille tuple feuille : 7996 octets
   taille tuple branche : 8028 octets

   ```sql
   select lf_blk_len from index_stats where name='ABC_PK';
   select br_blk_len from index_stats where name='ABC_PK';
   ```

   resultat :

   ```
   LF_BLK_LEN
   ----------
       7996

   BR_BLK_LEN
   ----------
       8028
   ```
3. donner le nombre de blocs branche d'index et le nombre de blocs feuille d'index
   nombre de blocs branches d'index: 4
   nombre de blocs feuilles d'index: 2088
   requètes:

   ```sql
   select br_blks from index_stats where name='ABC_PK';
   select lf_blks from index_stats where name='ABC_PK';
   ```

   resultat:

   ```
   BR_BLKS
   ----------
           4
   LF_BLKS
   ----------
       2088
   ```
4. donner le nombre total de blocs alloués à l'index ABC_PK, ainsi que la hauteur de l'index
   4.1.nombre total de blocks alloués à l'index :2176
   requête:

   ```sql
   select blocks from index_stats where name='ABC_PK';
   ```

   resultat:

   ```
       BLOCKS
   ----------
       2176
   ```

   4.2.hauteur de l'index:3
   requête:

   ```sql
   select height from index_stats where name='ABC_PK';
   ```

   resultat:

   ```
       HEIGHT
   ----------
           3
   ```
5. donner la taille en octets de l'espace de stockage qui a été réservé à l'index ABC_PK
   taille de l'espace de stockage reservé à l'index : 16727760 octets
   requête:

   ```sql
   select btree_space from index_stats where name='ABC_PK';
   ```

   resultat:

   ```
   BTREE_SPACE
   -----------
   16727760
   ```
6. donnez la vue et un exemple d'utilisation de la vue qui pourrait être consultée pour savoir si tous les blocs de l'index ABC_PK sont présents dans le cache de données
   la vue necessaire est v$cache
   requête pour voir si tous les blocs de ABC_PK sont presents dans le cache

   ```sql
   select count(*) from ((select block_id from dba_extents  where segment_name = 'ABC') minus (select block# from v$cache));
   ```

Commenter les résultats obtenus par rapport à ce que vous en saviez après calculs (exercice TD). Est ce cohérent (2 points)  ?
Commentaires des resultats obtenus pendant le tp :
Nous remarquons une similarité entre les valeurs retrouvées pendant le tp et pendant le td , les valeurs se rapprochent beaucoup

Exercice 2 (3 points) :
Pensez vous que l'index est utilisé pour les deux ordres de consultation suivants ? Justifiez votre réponse et donnez le nombre de blocs à parcourir (de données et possiblement d'index dans les deux cas) pour satisfaire la requête.

1. select A from ABC where A = 10001 ;
   **reponse**:*l'index est utilisé en effet , les index sont automatiquement utilisés par Oracle si la clé d'index est mentionnée dans la clause where ce qui est le cas ici*
2. select A, B from ABC where C like '%ABC%';
   **reponse**:*l'index ici n'est pas utilisé car clé A non mentionnée dans la clause where*

Exercice 3  (4 points):
Construisez une procédure PL/SQL qui vous semble à même de renvoyer les informations les plus importantes concernant l'organisation logique et physique d'une table et de ses index. Un plus sera de traiter les exceptions possibles et de construire un paquetage. Un exemple d'exécution et d'affichage des résultats est également attendu.
    NON effectué