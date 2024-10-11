---
tags:
  - AdminBDD
  - cours
---
# 1.Shemat exploité
## Contexte :
```
Vous créerez une table COMMUNE à partir de la table COMMUNE du schéma utilisateur P00000009432.
Vous définirez une contrainte de clé primaire nommée COMMUNE_PK s’appliquant à l’attribut CODEINSEE de votre table COMMUNE.
Vous construirez également la table DEPARTEMENT à partir du même schéma utilisateur et y poserez une contrainte de clé primaire.
Vous définirez alors une contrainte de clé étrangère sur l’attribut NUMDEP de COMMUNE qui référence l’attribut NUMDEP de DEPARTEMENT.


```
## 1.1 QUESTION 1: les ordres associés aux créations des tables et contraintes ainsi définies.

```sql

CREATE TABLE DEPARTEMENT (
    NUMDEP VARCHAR2(3) PRIMARY KEY,
    NOMDEP VARCHAR2(100)
);


```

cette table est créée avec NUMDEP comme clé primaire 

### Etape 2 : la table avec clé étrangère faisant reference à DEPARTEMENT : COMMUNE

```sql


CREATE TABLE COMMUNE (
    CODEINSEE VARCHAR2(5),
    NOMCOMMUNE VARCHAR2(100),
    NUMDEP VARCHAR2(3),
    CONSTRAINT COMMUNE_PK PRIMARY KEY (CODEINSEE),
    CONSTRAINT COMMUNE_FK FOREIGN KEY (NUMDEP) REFERENCES DEPARTEMENT(NUMDEP)
);



```
cette table est créée avec CODEINSEE comme clé primaire
on dfinit la contrainte de clé etrangère

verdict (on crée les tables par ordre hierarchique de dependances de contraintes)

## 1.2 QUESTION 2: creation index non unique sur NUMDEP


```sql

CREATE INDEX numdep_idx
ON COMMUNE (NUMDEP);


```
create index permet de créer un Index qui accélère les recherches sur la colonne NUMDEP de COMMUNE

# 2.Rappels consultation vues de méta-schéma relatives aux index
## Contexte :
### Rappels
```md
Voici le texte réécrit sans changement :

### 2. Rappel sur la consultation des vues du méta-schéma relatives aux index
Les vues `index_stats` et `user_indexes` aident à la compréhension des structures d’index manipulées par un serveur de base de données. L’ordre SQL donné ci-dessous exploite `user_indexes` et permet par exemple de consulter l’ensemble des index définis sur le schéma utilisateur, le nom de l’index, le nom de la table impactée par l’index ainsi que la hauteur de l’arbre (sans le niveau des feuilles)¹.

`
SELECT index_name, blevel, table_name FROM USER_INDEXES;
`

*Listing 1 – Vue index*

La vue `index_stats` donne des informations complémentaires (parfois chevauchantes) à la vue `user_indexes`. Il est ainsi possible de disposer d’informations sur la place mémoire occupée par l’index, le nombre de blocs occupés par les nœuds branches (BR BLKS) et les nœuds feuilles de l’arbre (LF BLKS).

Il est cependant nécessaire de collecter les statistiques sur les index avant de consulter cette vue. La consultation ci-dessous retourne respectivement le nom de l’index, l’espace occupé en octets, le nombre de répétitions pour la valeur de clé la plus répétée, le nombre de tuples (clé, rowid, pointeur tuple gauche, pointeur tuple droit) au niveau feuille, le nombre de tuples (clé, pointeur) au niveau nœud des branches, et la hauteur de l’arbre (avec le niveau feuille, donc égal à `blevel+1`).

`
-- mettre à jour les statistiques pour un index cible
ANALYZE INDEX <index_name> VALIDATE STRUCTURE;
SELECT name, btree_space, most_repeated_key, lf_rows, br_rows, height FROM INDEX_STATS;
`

*Listing 2 – Vue statistiques sur index*

¹ La valeur 0 indique que l’index n’est constitué que d’un niveau racine.
```

## 2.1 QUESTION 3: 
```md
Vous exploiterez les vues du méta-schéma, notamment les vues `index_stats` et `user_indexes`, pour répondre aux questions suivantes. Pensez également à mettre à jour au préalable les statistiques de la table de l’une des deux manières suivantes :

`
-- 
ANALYZE TABLE COMMUNE COMPUTE STATISTICS;
-- possible aussi pour le schéma entier
EXEC DBMS_UTILITY.ANALYZE_SCHEMA(user, 'COMPUTE');
`

*Listing 3 – Collecter les statistiques*

Vous donnerez les requêtes SQL suivantes ainsi que leurs résultats :

1. Quelle est la hauteur de l'index `COMMUNE_PK` de la table COMMUNE ?
2. Quels sont les nombres de blocs de branches et de feuilles qui ont été réservés pour l'index `COMMUNE_PK` de la table COMMUNE ?
3. Pour cet index, quelle est la taille de chaque tuple (clé, pointeurs, rowid) présent au niveau des blocs des feuilles ?
4. Par comparaison, quelle est la taille moyenne de chaque tuple de la table COMMUNE et combien de tuples peuvent être stockés dans un bloc (calcul du facteur de blocage de l’espace qui tient compte de l’espace toujours laissé libre, et donc de la valeur de `PCT_FREE` de la vue `USER_TABLES`) ?
5. Quelle est la hauteur de l'index `NUMDEP_IDX` de la table COMMUNE ?
6. Expliquez ce que renvoient les valeurs des attributs `DISTINCT_KEYS` et `MOST_REPEATED_KEY` de la vue `INDEX_STATS` pour l'index `NUMDEP_IDX`.
```

### 2.1.1 Quelle est la hauteur de l'index COMMUNE_PK de la table COMMUNE ?

il faut extraire l'information de blevel (hauteur de l'arbre d'index sans les feuilles) c'est pour ça qu'on ajoute 1
```sql

SELECT blevel + 1 AS height
FROM USER_INDEXES
WHERE index_name = 'COMMUNE_PK';

```
### 2.1.2 Quels sont les nombres de blocs de branches et de feuilles réservés pour l'index COMMUNE_PK

il va falloir utiliser INDEX_STATS (qui contient des infos detaillées sur la structure )