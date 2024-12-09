
### Base de Données de Référence

**Schéma** : GANDALF

**Tables** :

- **DEPARTEMENT**(NUMDEP NUMBER PRIMARY KEY, NOMDEP VARCHAR2(100))
- **COMMUNE**(CODEINSEE VARCHAR2(10) PRIMARY KEY, NOMCOMMAJ VARCHAR2(100), NUMDEP NUMBER, FONCTION VARCHAR2(50),  
    CONSTRAINT COMMUNE_FK FOREIGN KEY(NUMDEP) REFERENCES DEPARTEMENT(NUMDEP))

**Exemple de données** :

- **DEPARTEMENT** :
    - (34, 'Hérault')
    - (30, 'Gard')
- **COMMUNE** :
    - ('34172', 'MONTPELLIER', 34, 'ouvrier')
    - ('34129', 'LATTES', 34, 'ouvrier')
    - ('30123', 'NIMES', 30, 'technicien')
    - ('34100', 'SETE', 34, 'ouvrier')

**Autres objets potentiels** :

- **PRVOIR**(valeur INTEGER PRIMARY KEY)
- **Index non unique** sur COMMUNE(NUMDEP) nommé NUMDEP_IDX.

Ces tables et objets servent d’exemple pour toutes les requêtes suivantes. Adaptez les noms et schémas en fonction de votre contexte.

---

### PARTIE 1 : INDEX

**Création et Informations sur les Index**

```sql
-- Créer un index non unique sur NUMDEP
CREATE INDEX GANDALF.NUMDEP_IDX ON GANDALF.COMMUNE(NUMDEP);
```

_Commentaires :_  
Cette requête crée un **index non unique** nommé `NUMDEP_IDX` sur la colonne `NUMDEP` de la table `COMMUNE` dans le schéma `GANDALF`.  
_Explication Théorique :_  
Un **index** améliore les performances des requêtes en permettant un accès rapide aux données sans avoir à scanner toute la table.

```sql
-- Lister tous les index du schéma
SELECT index_name, table_name, blevel, distinct_keys, leaf_blocks 
FROM user_indexes;
```

_Commentaires :_  
Cette requête récupère les noms des index, les tables associées, le niveau de l'arbre B (`blevel`), le nombre de clés distinctes (`distinct_keys`), et le nombre de blocs de feuilles (`leaf_blocks`) pour tous les index dans le schéma courant.  
_Explication Théorique :_  
La **vue `user_indexes`** contient des informations sur tous les index appartenant à l'utilisateur courant.

```sql
-- Filtrer sur la table COMMUNE
SELECT index_name, blevel, table_name, distinct_keys, leaf_blocks
FROM user_indexes
WHERE table_name='COMMUNE';
```

_Commentaires :_  
Cette requête affiche les détails des index spécifiquement associés à la table `COMMUNE`.  
_Explication Théorique :_  
Filtrer par `table_name` permet de se concentrer sur les index d'une table particulière.

```sql
-- Analyser un index pour obtenir des statistiques détaillées
ANALYZE INDEX GANDALF.COMMUNE_PK VALIDATE STRUCTURE;
```

_Commentaires :_  
Cette commande analyse l'index `COMMUNE_PK` dans le schéma `GANDALF` pour valider sa structure et collecter des statistiques détaillées.  
_Explication Théorique :_  
L'**analyse** permet d'obtenir des informations sur la structure de l'index, ce qui aide le planificateur de requêtes à optimiser les performances.

```sql
-- Après l’analyse, consulter la vue INDEX_STATS
SELECT name, btree_space, used_space, pct_used,
       most_repeated_key, lf_rows, br_rows, height
FROM index_stats;
```

_Commentaires :_  
Cette requête affiche des statistiques détaillées sur les index, telles que l'espace utilisé par l'arbre B (`btree_space`), l'espace effectivement utilisé (`used_space`), le pourcentage d'utilisation (`pct_used`), la clé la plus répétée (`most_repeated_key`), le nombre de lignes dans les feuilles (`lf_rows`), le nombre de lignes dans les branches (`br_rows`), et la hauteur de l'index (`height`).  
_Explication Théorique :_  
La **vue `INDEX_STATS`** fournit des statistiques essentielles pour évaluer la performance et l'efficacité des index.

```sql
-- Mettre à jour les stats sur la table pour cohérence
ANALYZE TABLE GANDALF.COMMUNE COMPUTE STATISTICS;
```

_Commentaires :_  
Cette commande calcule et met à jour les statistiques pour la table `COMMUNE` dans le schéma `GANDALF`, assurant ainsi la cohérence des informations utilisées par le planificateur de requêtes.  
_Explication Théorique :_  
Les **statistiques de table** aident l'optimiseur de requêtes à choisir le meilleur plan d'exécution.

```sql
-- Vérifier la sélectivité (distinct_keys), la hauteur de l’index
SELECT index_name, distinct_keys, blevel FROM user_indexes WHERE table_name='COMMUNE';
```

_Commentaires :_  
Cette requête extrait le nom de l'index, le nombre de clés distinctes (`distinct_keys`), et le niveau de l'arbre B (`blevel`) pour les index de la table `COMMUNE`.  
_Explication Théorique :_  
La **sélectivité** d'un index, mesurée par `distinct_keys`, indique la proportion de valeurs uniques, influençant ainsi l'efficacité de l'index.

```sql
-- Comparer taille moyenne des tuples dans la table avec la taille 
-- des tuples stockés dans l’index
SELECT table_name, blocks, num_rows, avg_row_len, pct_free
FROM user_tables
WHERE table_name='COMMUNE';
```

_Commentaires :_  
Cette requête compare la taille moyenne des enregistrements (`avg_row_len`) dans la table `COMMUNE` avec celle des tuples dans l'index, en affichant également le nombre de blocs (`blocks`), le nombre de lignes (`num_rows`), et le pourcentage d'espace libre (`pct_free`).  
_Explication Théorique :_  
Comparer ces tailles aide à évaluer l'efficacité de l'index par rapport à la table principale.

**Ambiguïtés Potentielles et Explications** :

- **blevel** : Représente la hauteur-1 de l’index. Si `blevel=0`, l’index n’a qu’un niveau (racine = feuilles).
- **most_repeated_key** : Indique le nombre maximal de répétitions d’une clé dans l’index, utile pour juger de la sélectivité.
- **distinct_keys** : Nombre de valeurs distinctes dans l’index. Plus il y a de `distinct_keys`, meilleure est la sélectivité généralement.
- **pct_used** : Pourcentage d’utilisation des blocs de l’index après analyse.

**Répondre aux questions sur les Index** :

- **Hauteur de l’index** : `blevel + 1`  
    _Exemple :_ Si `blevel=2`, alors la hauteur de l'index est `3`.
- **Nombre de blocs de feuilles / branches** : `lf_rows` (feuilles), `br_rows` (branches) dans `INDEX_STATS`.  
    _Exemple :_ Une valeur élevée de `lf_rows` indique plus de blocs de feuilles.
- **Taille moyenne d’un tuple d’index** : `btree_space / lf_rows` (approximatif).  
    _Exemple :_ Si `btree_space=1000` et `lf_rows=200`, la taille moyenne est `5`.
- **Comparaison facteur de blocage table vs index** : Utiliser `avg_row_len` (table) vs espace dans les feuilles (index).  
    _Exemple :_ Si la taille moyenne des tuples dans la table est inférieure à celle dans l'index, l'index est plus efficace pour les recherches.

---

### PARTIE 2 : OPTIMISATION

**Statistiques et Informations Physiques sur les Tables et Blocs**

```sql
-- Collecter des stats sur la table
ANALYZE TABLE GANDALF.COMMUNE COMPUTE STATISTICS;
```

_Commentaires :_  
Cette commande calcule et met à jour les statistiques pour la table `COMMUNE` dans le schéma `GANDALF`.  
_Explication Théorique :_  
Les **statistiques de table** incluent des informations sur le nombre de lignes, la distribution des données, et l'espace utilisé, essentielles pour l'optimisation des requêtes.

```sql
-- Infos sur la table (nombre de blocs, nb de tuples, taille moyenne, pct_free)
SELECT table_name, blocks, num_rows, avg_row_len, pct_free
FROM user_tables
WHERE table_name='COMMUNE';
```

_Commentaires :_  
Cette requête affiche des informations détaillées sur la table `COMMUNE`, incluant le nombre de blocs alloués (`blocks`), le nombre de tuples (`num_rows`), la longueur moyenne des lignes (`avg_row_len`), et le pourcentage d'espace libre (`pct_free`).  
_Explication Théorique :_  
Ces métriques aident à comprendre la structure physique de la table et à identifier des opportunités d'optimisation, comme ajuster `pct_free` pour mieux gérer les mises à jour.

```sql
-- Voir l’owner, object_name, object_id pour jointure avec v$bh
SELECT owner, object_name, object_id, data_object_id
FROM dba_objects
WHERE owner='GANDALF' AND object_name='COMMUNE';
```

_Commentaires :_  
Cette requête récupère le propriétaire (`owner`), le nom de l'objet (`object_name`), l'identifiant de l'objet (`object_id`), et l'identifiant de l'objet de données (`data_object_id`) pour la table `COMMUNE` dans le schéma `GANDALF`.  
_Explication Théorique :_  
Ces informations sont nécessaires pour effectuer des jointures avec d'autres vues système comme `v$bh` afin d'obtenir des détails supplémentaires sur les blocs de données.

```sql
-- Lister les blocs en cache (v$bh) pour la table COMMUNE
SELECT o.owner, o.object_name, bh.file#, bh.block#, bh.class#, bh.status, bh.dirty
FROM v$bh bh, dba_objects o
WHERE bh.obj = o.data_object_id
  AND o.owner = 'GANDALF'
  AND o.object_name = 'COMMUNE';
```

_Commentaires :_  
Cette requête liste les blocs en cache pour la table `COMMUNE`, en joignant la vue `v$bh` (buffer headers) avec `dba_objects` pour obtenir des informations sur les fichiers, les numéros de blocs, les classes de blocs (`class#`), le statut (`status`), et si le bloc est modifié en mémoire (`dirty`).  
_Explication Théorique :_  
La **vue `v$bh`** fournit des informations sur les blocs actuellement en mémoire, ce qui est crucial pour analyser les performances et l'utilisation du cache.

```sql
-- Vérifier le bloc d’en-tête du segment (segment header)
SELECT segment_name, header_file, header_block
FROM dba_segments
WHERE owner='GANDALF' AND segment_name='COMMUNE';
```

_Commentaires :_  
Cette requête affiche le nom du segment (`segment_name`), le fichier d'en-tête (`header_file`), et le bloc d'en-tête (`header_block`) pour la table `COMMUNE` dans le schéma `GANDALF`.  
_Explication Théorique :_  
Le **bloc d'en-tête** contient des informations de gestion sur le segment, telles que la localisation des blocs de données.

```sql
-- Localiser physiquement un tuple avec dbms_rowid
SELECT rowid, 
       dbms_rowid.rowid_block_number(rowid) AS block_no,
       dbms_rowid.rowid_row_number(rowid) AS row_no
FROM GANDALF.COMMUNE
WHERE codeinsee='34172';
```

_Commentaires :_  
Cette requête récupère le `ROWID` d'un tuple spécifique dans la table `COMMUNE` où `codeinsee` est '34172', ainsi que le numéro de bloc (`block_no`) et le numéro de ligne (`row_no`) correspondant.  
_Explication Théorique :_  
Le **`ROWID`** est une adresse unique pour chaque ligne dans une table, permettant un accès direct et rapide.

```sql
-- Reconstitution d’un ROWID à partir des infos row_wait_obj#, file#, block#, row#
-- (Cas où on a row_wait_obj# depuis v$session)
SELECT data_object_id 
FROM dba_objects
WHERE object_id=<row_wait_obj#>;
```

_Commentaires :_  
Cette requête récupère `data_object_id` à partir de `object_id` fourni (`row_wait_obj#`), nécessaire pour reconstituer un `ROWID`.  
_Explication Théorique :_  
Pour reconstituer un `ROWID`, il faut connaître l'identifiant de l'objet de données, le fichier, le bloc et la ligne.

```sql
SELECT dbms_rowid.rowid_create(1, <data_object_id>, <file#>, <block#>, <row#>) AS reconst_rowid
FROM dual;
```

_Commentaires :_  
Cette requête utilise la fonction `dbms_rowid.rowid_create` pour créer un `ROWID` à partir des composants fournis : le type de données (1 pour les tables), `data_object_id`, numéro de fichier (`file#`), numéro de bloc (`block#`), et numéro de ligne (`row#`).  
_Explication Théorique :_  
La fonction `ROWID_CREATE` permet de reconstituer un `ROWID` valide à partir de ses composants individuels.

```sql
SELECT * FROM GANDALF.COMMUNE WHERE rowid='reconst_rowid';
```

_Commentaires :_  
Cette requête sélectionne le tuple de la table `COMMUNE` dont le `ROWID` correspond à `reconst_rowid`.  
_Explication Théorique :_  
Utiliser un `ROWID` permet d'accéder directement à un enregistrement spécifique sans scanner la table.

**Ambiguïtés et Explications** :

- **class# dans v$bh** :
    - `1` = data block
    - `4` = segment header
    - _Autres valeurs possibles_ représentent différents types de blocs.
- **dirty='Y'** :  
    Indique que le bloc a été modifié en mémoire et n'a pas encore été écrit sur disque.
- **pct_free** :  
    Pourcentage d’espace libre réservé dans chaque bloc pour les mises à jour futures.
- **Facteur de blocage** :  
    Calculé comme suit : \text{Facteur de blocage} = \frac{\text{Taille du bloc} \times (1 - \frac{\text{pct_free}}{100})}{\text{avg_row_len}} Cela donne une estimation du nombre de tuples pouvant être stockés par bloc.

**Répondre aux questions sur l’Optimisation** :

- **Combien de blocs ?**  
    Utiliser `user_tables.blocks`.  
    _Exemple :_
    
    ```sql
    SELECT blocks FROM user_tables WHERE table_name='COMMUNE';
    ```
    
- **Taille moyenne d’un tuple ?**  
    Utiliser `user_tables.avg_row_len`.  
    _Exemple :_
    
    ```sql
    SELECT avg_row_len FROM user_tables WHERE table_name='COMMUNE';
    ```
    
- **Facteur de blocage théorique vs réel ?**  
    Comparer `avg_row_len`, taille des blocs, et le nombre effectif de tuples par bloc (en itérant sur leurs `rowid_block_number`).  
    _Explication :_  
    Cela permet de vérifier si la distribution des données respecte les attentes théoriques basées sur les statistiques.
- **Bloc d’en-tête correct ?**  
    Comparer `header_block` de `dba_segments` avec le bloc trouvé en `class#=4` dans `v$bh`.  
    _Exemple :_
    
    ```sql
    SELECT header_block FROM dba_segments WHERE segment_name='COMMUNE';
    ```
    
    ```sql
    SELECT block# FROM v$bh WHERE class#=4 AND object_id=(SELECT object_id FROM dba_objects WHERE object_name='COMMUNE');
    ```
    

---

### PARTIE 3 : TRANSACTIONS

**Modes d’Isolation, Transactions READ ONLY, SERIALIZABLE**

```sql
-- Définir une transaction READ ONLY
SET TRANSACTION READ ONLY;
SELECT * FROM COMMUNE; -- lecture OK
UPDATE COMMUNE SET fonction='X' WHERE fonction='Y'; -- interdit
ROLLBACK;
```

_Commentaires :_  
Cette séquence de commandes définit une transaction en mode **READ ONLY**, effectue une sélection qui est autorisée, tente une mise à jour qui est **interdite** et finalement annule la transaction avec `ROLLBACK`.  
_Explication Théorique :_  
Le mode **READ ONLY** empêche toute modification des données durant la transaction, assurant ainsi l'intégrité des lectures.

```sql
-- Revenir en READ WRITE via un DDL
SET TRANSACTION READ ONLY;
CREATE TABLE GANDALF.PrVoir (valeur INTEGER PRIMARY KEY);
-- Le DDL provoque commit implicite, nouvelle transaction en READ WRITE
UPDATE COMMUNE SET fonction='commerciale' WHERE fonction='commercial';
ROLLBACK;
```

_Commentaires :_  
Après avoir défini la transaction en **READ ONLY**, l'exécution d'un DDL (`CREATE TABLE`) provoque un **commit implicite**, ce qui réinitialise la transaction en mode **READ WRITE**. La mise à jour suivante est alors autorisée jusqu'à l'exécution de `ROLLBACK`, qui annule cette dernière.  
_Explication Théorique :_  
Les commandes **DDL** (Data Definition Language) comme `CREATE`, `DROP`, ou `ALTER` provoquent un **commit implicite**, ce qui termine la transaction en cours et commence une nouvelle transaction en mode **READ WRITE**.

```sql
-- Mode SERIALIZABLE
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
SELECT * FROM GANDALF.COMMUNE;
-- Insertions concurrentes après le début de la transaction pas visibles jusqu’au commit/rollback
```

_Commentaires :_  
Cette séquence définit la transaction avec le niveau d'isolation **SERIALIZABLE**, effectue une sélection, et indique que les insertions concurrentes ne seront pas visibles jusqu'à la fin de la transaction.  
_Explication Théorique :_  
Le mode **SERIALIZABLE** garantit que les transactions sont exécutées de manière totalement isolée, comme si elles étaient séquentielles, évitant ainsi les effets de concurrence.

**Effets des DDL sur les Transactions (COMMIT implicite)**

```sql
INSERT INTO PrVoir VALUES(1);
CREATE TABLE GANDALF.EncorePrVoir (valeur integer primary key);
-- Commit implicite avant et après CREATE TABLE
-- L’INSERT est validé, ROLLBACK ensuite n’a aucun effet
SELECT * FROM PrVoir; -- voit la ligne 1
```

_Commentaires :_  
L'insertion dans `PrVoir` est effectuée, suivie d'un DDL (`CREATE TABLE`). Le DDL provoque un **commit implicite** avant et après son exécution, ce qui valide l'insertion. Par conséquent, un `ROLLBACK` ultérieur n'aura aucun effet sur l'insertion déjà validée.  
_Explication Théorique :_  
Les **DDL** interrompent la transaction en cours en exécutant un **commit**, ce qui fixe les modifications précédentes.

```sql
-- En cas d’erreur DDL
INSERT INTO PrVoir VALUES(2);
CREATE TABLE GANDALF.EncorePrVoir (valeur integre); -- erreur
-- L’INSERT est validé avant l’erreur
ROLLBACK; -- sans effet sur l’INSERT
SELECT * FROM PrVoir; -- voit la ligne 2 validée
```

_Commentaires :_  
L'insertion est réalisée, suivie d'une tentative de création de table avec une erreur (`valeur integre` au lieu de `integer`). L'**insertion est validée** avant l'erreur DDL, et le `ROLLBACK` n'affecte pas l'insertion déjà commise.  
_Explication Théorique :_  
Même si un DDL échoue, le **commit implicite** avant l'exécution du DDL valide les modifications précédentes.

**Verrous, Blocages et Transactions Concurrentes**

```sql
-- Voir les verrous actifs
SELECT sid, type, lmode, request, block FROM v$lock;
```

_Commentaires :_  
Cette requête affiche les verrous actifs, incluant l'identifiant de session (`sid`), le type de verrou (`type`), le mode de verrouillage actuel (`lmode`), la demande de verrou (`request`), et si le verrou bloque d'autres sessions (`block`).  
_Explication Théorique :_  
La **vue `v$lock`** fournit des informations sur les verrous détenus et demandés par les sessions, essentielle pour diagnostiquer les problèmes de concurrence.

```sql
-- Associer verrous et objets
SELECT s.username, s.sid, o.object_name, o.object_type, l.locked_mode
FROM v$locked_object l
JOIN dba_objects o ON l.object_id = o.object_id
JOIN v$session s ON l.session_id = s.sid;
```

_Commentaires :_  
Cette requête associe les verrous aux objets en joignant `v$locked_object` avec `dba_objects` et `v$session`, affichant le nom de l'utilisateur (`username`), l'identifiant de session (`sid`), le nom de l'objet (`object_name`), le type d'objet (`object_type`), et le mode de verrouillage (`locked_mode`).  
_Explication Théorique :_  
Cela permet d'identifier quels objets sont verrouillés et par quelles sessions, facilitant ainsi la résolution des blocages.

```sql
-- Identifier sessions bloquantes/bloquées
SELECT a.sid AS blockingSession, b.sid AS blockedSession, b.request
FROM v$lock a, v$lock b
WHERE a.sid != b.sid 
  AND a.id1 = b.id1 
  AND a.id2 = b.id2 
  AND b.request > 0 
  AND a.block = 1;
```

_Commentaires :_  
Cette requête identifie les sessions bloquantes et bloquées en comparant les verrous détenus et demandés.

- `blockingSession` : Session qui détient le verrou.
- `blockedSession` : Session qui demande le verrou.
- `request` : Type de demande de verrou.  
    _Explication Théorique :_  
    Les sessions avec `a.block = 1` sont celles qui bloquent d'autres sessions (`b`), identifiées par des verrouillages similaires (`id1`, `id2`).

```sql
-- Ajouter le nom de l’objet, types de verrous
SELECT s1.username AS blocking_user, s2.username AS blocked_user,
       o.object_name, a.type AS blocking_lock_type, b.type AS blocked_lock_type, b.request
FROM v$lock a, v$lock b, v$session s1, v$session s2,
     v$locked_object lo, dba_objects o
WHERE a.sid != b.sid
  AND a.id1 = b.id1 
  AND a.id2 = b.id2
  AND b.request > 0 
  AND a.block = 1
  AND s1.sid = a.sid 
  AND s2.sid = b.sid
  AND lo.session_id = a.sid
  AND lo.object_id = o.object_id;
```

_Commentaires :_  
Cette requête enrichit l’identification des sessions bloquantes et bloquées en incluant :

- `blocking_user` : Nom de l'utilisateur bloquant.
- `blocked_user` : Nom de l'utilisateur bloqué.
- `object_name` : Nom de l'objet verrouillé.
- `blocking_lock_type` : Type de verrou détenu par le bloquant.
- `blocked_lock_type` : Type de verrou demandé par le bloqué.
- `request` : Demande spécifique de verrou.

_Explication Théorique :_  
Cette requête fournit une vue détaillée des interactions de verrouillage entre utilisateurs, aidant à diagnostiquer et résoudre les conflits de verrous.

```sql
-- Transactions en cours
SELECT addr, xidusn, xidslot, xidsqn, start_time FROM v$transaction;
```

_Commentaires :_  
Cette requête affiche les transactions en cours, incluant l'adresse (`addr`), l'ID de transaction (`xidusn`), le slot de transaction (`xidslot`), la séquence de transaction (`xidsqn`), et l'heure de début (`start_time`).  
_Explication Théorique :_  
La **vue `v$transaction`** fournit des détails sur les transactions actives, permettant de surveiller leur état et leur durée.

**Scénarios Concurrents**

```sql
-- Insert non commitée par user2 non visible par user1
INSERT INTO emp VALUES(101,'Dupont','Jean','Comptable',3500,NULL,SYSDATE,NULL,1);
-- pas de commit
-- user1: SELECT * FROM user2.emp; ne voit pas la nouvelle ligne
-- user2: COMMIT;
-- user1: SELECT * FROM user2.emp; voit la nouvelle ligne
```

_Commentaires :_  
Ce scénario illustre qu'une insertion effectuée par `user2` n'est pas visible par `user1` tant que `user2` n'a pas effectué de `COMMIT`.  
_Explication Théorique :_  
Les **transactions non committées** sont isolées et ne sont pas visibles par d'autres sessions, assurant ainsi l'intégrité des données.

```sql
-- Mise à jour concurrente
-- user2
UPDATE emp SET salaire=1000 WHERE num=101; -- verrou
-- user1
UPDATE user2.emp SET salaire=2000 WHERE num=101; -- bloqué jusqu’au COMMIT de user2
-- user2: COMMIT;
-- user1 se débloque et applique son UPDATE.
```

_Commentaires :_  
Dans ce scénario, `user2` met à jour le salaire de l'employé numéro 101, acquérant un verrou.  
Ensuite, `user1` tente de mettre à jour le même enregistrement et est **bloqué** jusqu'à ce que `user2` effectue un `COMMIT`.  
_Explication Théorique :_  
Les **verrous exclusifs** empêchent les mises à jour concurrentes sur le même enregistrement jusqu'à ce que la première transaction soit terminée.

```sql
-- Mode SERIALIZABLE et ORA-08177
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
-- user1 lit une ligne
SELECT * FROM user2.emp WHERE num=12;
-- user2 modifie cette ligne et commit
-- user1 essaie plus tard de la modifier, reçoit ORA-08177 car la donnée a changé depuis la lecture initiale.
```

_Commentaires :_  
Dans ce scénario, `user1` est en mode **SERIALIZABLE** et lit une ligne.  
`user2` modifie cette même ligne et effectue un `COMMIT`.  
Lorsque `user1` tente de modifier la ligne, Oracle lève une erreur **ORA-08177** car les données ont été modifiées depuis la lecture initiale.  
_Explication Théorique :_  
En mode **SERIALIZABLE**, toute modification concurrente après une lecture initiale entraîne une **erreur de validation** pour maintenir la cohérence.

```sql
-- Interblocage (deadlock)
-- user1 bloque A, user2 bloque B, user1 veut B, user2 veut A
-- ORA-00060 détecté par Oracle, une transaction rollbackée d’office
```

_Commentaires :_  
Ce scénario décrit un **interblocage** où `user1` et `user2` se bloquent mutuellement en tentant d'accéder à des ressources verrouillées par l'autre.  
Oracle détecte cet interblocage et lève l'erreur **ORA-00060**, annulant automatiquement l'une des transactions pour résoudre le conflit.  
_Explication Théorique :_  
Les **interblocages** surviennent lorsque deux ou plusieurs transactions attendent indéfiniment des ressources verrouillées par les autres, nécessitant une intervention du système pour les résoudre.

**Droits et Privilèges**

```sql
-- Voir les privilèges accordés par GANDALF
SELECT grantee, privilege, owner, table_name
FROM dba_tab_privs
WHERE owner='GANDALF';
```

_Commentaires :_  
Cette requête liste les privilèges accordés par le propriétaire `GANDALF`, incluant le bénéficiaire (`grantee`), le type de privilège (`privilege`), le propriétaire de la table (`owner`), et le nom de la table (`table_name`).  
_Explication Théorique :_  
La **vue `dba_tab_privs`** affiche les privilèges d'accès aux tables, permettant de vérifier qui peut effectuer quelles opérations.

```sql
-- Donner tous les droits sur EMP à un utilisateur
GRANT ALL ON emp TO e20200010573;
```

_Commentaires :_  
Cette commande accorde **tous les privilèges** sur la table `EMP` à l'utilisateur `e20200010573`.  
_Explication Théorique :_  
Le **commande `GRANT`** permet d'attribuer des privilèges d'accès aux objets de la base de données à des utilisateurs ou rôles spécifiques.

**Ambiguïtés et Explications** :

- **Chaque DDL (CREATE, DROP, ALTER) provoque un commit implicite** :  
    Cela valide toutes les modifications en attente avant et après l'exécution du DDL, terminant ainsi la transaction actuelle.
- **Mode READ ONLY** :  
    Empêche les modifications des données, mais un DDL redémarre la transaction en mode **READ WRITE**.
- **Mode SERIALIZABLE** :  
    Assure une cohérence stricte en empêchant les lectures non répétables et les fantômes, mais peut échouer avec l'erreur **ORA-08177** si des modifications concurrentes surviennent.
- **Interblocage** :  
    Lorsque deux sessions s'attendent mutuellement sur des ressources verrouillées, Oracle résout le blocage en annulant une des transactions et en levant l'erreur **ORA-00060**.

**Répondre aux questions sur Transactions** :

- **Comment voir qui verrouille quoi ?**  
    Utiliser les vues `v$locked_object`, `dba_objects`, et `v$session`.  
    _Exemple :_
    
    ```sql
    SELECT s.username, o.object_name, l.locked_mode
    FROM v$locked_object l
    JOIN dba_objects o ON l.object_id = o.object_id
    JOIN v$session s ON l.session_id = s.sid;
    ```
    
- **Qui est bloquant, qui est bloqué ?**  
    Utiliser la vue `v$lock` avec `block=1` pour identifier les sessions bloquantes et bloquées.  
    _Exemple :_
    
    ```sql
    SELECT a.sid AS blockingSession, b.sid AS blockedSession
    FROM v$lock a, v$lock b
    WHERE a.sid != b.sid 
      AND a.id1 = b.id1 
      AND a.id2 = b.id2 
      AND b.request > 0 
      AND a.block = 1;
    ```
    
- **Qu’arrive-t-il en mode READ ONLY ?**  
    **Aucune modification** des données n'est possible. Toute tentative de modification est **interdite**.
- **Effet d’un DDL sur la transaction ?**  
    **Commit implicite** avant et après l'exécution du DDL, validant toutes les modifications en attente.
- **Comment résoudre un blocage ?**  
    Attendre le `COMMIT` ou `ROLLBACK` de la transaction bloquante, ou Oracle résout automatiquement les **interblocages** en annulant une transaction.
- **Comment expliquer ORA-08177 ?**  
    **Conflit en mode SERIALIZABLE** : Une donnée a été modifiée depuis la lecture initiale de la transaction, invalidant son instantané.
- **Quid des modifications non visibles avant COMMIT ?**  
    Les **modifications** restent **isolées** et **invisibles** pour les autres transactions jusqu'à ce qu'elles soient **commitées**.

---

### Méthodologie de Réponse aux Questions

- **Identifier la vue ou la commande appropriée** :
    
    - **Si question sur l’index** → `user_indexes`, `index_stats`, `ANALYZE INDEX`
    - **Si question sur la table (blocs, tuples)** → `user_tables`, `dba_segments`, `dbms_rowid`, `v$bh`
    - **Si question sur les verrous** → `v$lock`, `v$session`, `v$locked_object`, `dba_objects`
    - **Si question sur l’isolation, READ ONLY, SERIALIZABLE** → `SET TRANSACTION`
    - **Si question sur l’effet des DDL** → Comprendre le **commit implicite**
- **Justifier la réponse** :
    
    1. **Montrer la requête utilisée**.
    2. **Montrer le résultat** (ex : “Cette requête affiche `block=1`, donc la session X bloque la session Y”).
    3. **Interpréter** : Expliquer pourquoi `block=1` signifie un blocage, pourquoi `CREATE TABLE` provoque un commit, etc.
- **Questions fréquentes et réponses types** :
    
    - _Comment savoir combien de blocs a la table ?_
        
        ```sql
        SELECT blocks FROM user_tables WHERE table_name='COMMUNE';
        ```
        
        **Blocks** = nombre de blocs alloués.
    - _Comment savoir qui bloque qui ?_
        
        ```sql
        SELECT ... FROM v$lock ... WHERE block=1; 
        ```
        
        La session qui a `block=1` bloque l’autre.
    - _Comment comparer la sélectivité d’un index ?_  
        Consulter `distinct_keys` et `most_repeated_key`.
    - _Comment localiser un tuple précis ?_  
        Utiliser `dbms_rowid.rowid_block_number(rowid)` et `dbms_rowid.rowid_row_number(rowid)` pour trouver le bloc et la position.
    - _Que se passe-t-il avec une transaction READ ONLY ?_  
        On ne peut pas modifier les données. Un DDL met fin au mode READ ONLY.
    - _Qu’est-ce qu’un commit implicite ?_  
        Toute commande DDL force un **commit** avant et après son exécution, validant les modifications en cours.

---

**Joyeux Noël et bonne chance pour le TP.**