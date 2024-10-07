---
tags:
  - AdminBDD
  - cours
---
### **Partie 1 : Introduction aux Mécanismes d'Indexation et Efficacité des Accès aux Données**

#### **1.1 Retour sur les Blocs Oracle**

Un bloc Oracle est l'unité minimale de stockage de données utilisée par le SGBD Oracle. Ce bloc contient des enregistrements organisés sous forme de tuples. Lorsqu'une requête SQL est exécutée, Oracle accède aux blocs de données pour récupérer ou manipuler les enregistrements souhaités.

**Structure d'un enregistrement :**
- Chaque enregistrement est identifié par un *rowid* qui suit la structure suivante : `object_number.relative_file_number.block_number.row_number`. Cette information permet de localiser précisément un enregistrement dans un bloc spécifique. Le paquetage PL/SQL `dbms_rowid` permet d'exploiter ces informations.

#### **1.2 Organisations Séquentielles Indexées (ISAM)**

**ISAM (Indexed Sequential Access Method)** est une méthode de gestion de fichiers développée par IBM en 1966. Elle organise les données de manière séquentielle tout en utilisant des index pour accélérer l'accès à ces données.

- **Index dense** : Chaque valeur de clé est représentée dans l'index. Cet index est trié et contient un pointeur vers le bloc de données correspondant.
- **Index creux (sparse)** : Seules certaines valeurs de clé sont représentées dans l'index. Généralement, une seule clé par bloc de données est utilisée, ce qui réduit la taille de l'index, mais allonge le temps de recherche.

##### **Exemple d'une Organisation Séquentielle avec un Index Dense :**

Supposons une table contenant 1 000 000 de tuples, avec 10 tuples par bloc de 4 Ko (ce qui équivaut à 100 000 blocs). La taille totale de la table est de 400 Mo. Si nous utilisons un index dense, chaque clé fait 30 octets et chaque pointeur fait 10 octets, donc la taille totale de l'index sera de 40 Mo (1 000 000 clés/pointeurs). Cet index est divisé en environ 10 000 blocs.

Lorsqu'une recherche est effectuée sur une clé, la recherche dans l'index se fait en utilisant une méthode dichotomique, ce qui implique un parcours logarithmique des blocs de l'index. Le temps d'accès est donc de `log2(10 000) ≈ 14` blocs.

##### **Exemple d'une Organisation Séquentielle avec un Index Creux :**

L'index creux ne représente qu'une partie des valeurs de clé, ce qui diminue sa taille et le rend plus efficace en termes de mémoire. Toutefois, la recherche dans les blocs de données peut être plus lente puisqu'il faut parcourir les blocs non indexés.

#### **1.3 B-Trees et Variantes**

Les **B-Trees** sont une structure d'index très couramment utilisée pour maintenir les données triées et permettre des recherches rapides. Contrairement aux ISAM, où l'index est fixe et ne s'adapte pas aux nouvelles insertions, les B-Trees réorganisent les données dynamiquement pour garantir un accès rapide, même après des insertions ou suppressions.

- **Caractéristiques des B-Trees :**
  - Les nœuds internes du B-Tree contiennent des pointeurs vers les enfants et des clés de recherche.
  - Les feuilles du B-Tree contiennent les valeurs de clé et les pointeurs vers les enregistrements de la base de données.
  - L'arbre est équilibré, ce qui signifie que tous les chemins de la racine aux feuilles ont la même longueur, garantissant ainsi une complexité logarithmique pour les opérations de recherche, d'insertion et de suppression.

#### **1.4 Tables de Hachage**

La **table de hachage** est une autre méthode d'indexation utilisée pour accélérer les recherches. Contrairement aux index basés sur les arbres, la table de hachage repose sur une fonction de hachage qui transforme la clé de recherche en une adresse ou une position dans une table.

- **Avantages de la table de hachage :**
  - Accès en temps constant (O(1)) pour les opérations de recherche et d'insertion.
  - Utilisée principalement pour des recherches d'égalité, c'est-à-dire lorsque l'on cherche une correspondance exacte avec une clé donnée.

- **Inconvénients :**
  - Moins efficace pour les recherches de plage (par exemple, trouver toutes les valeurs entre 10 et 20).
  - La performance dépend de la qualité de la fonction de hachage et des collisions possibles.

#### **1.5 Index Bitmap**

Les **index bitmap** sont particulièrement adaptés aux bases de données décisionnelles où les requêtes impliquent souvent des opérations sur de larges ensembles de données. Un index bitmap associe chaque valeur distincte de la clé indexée à une séquence de bits (bitmap) où chaque bit correspond à un enregistrement de la table.

- **Avantages des index bitmap :**
  - Très efficace pour les colonnes ayant un faible nombre de valeurs distinctes (par exemple, une colonne de sexe avec seulement deux valeurs possibles : homme ou femme).
  - Permet des opérations logiques rapides (ET, OU, NON) sur plusieurs index bitmap.

- **Inconvénients :**
  - Les index bitmap sont moins adaptés aux environnements où il y a beaucoup d'insertions, suppressions ou mises à jour, car chaque modification nécessite une réorganisation du bitmap.
### **Partie 2 : Structures d’Indexation Avancées et Arbres B-Tree**

#### **2.1 Organisation Séquentielle Indexée avec Index Creux**

Dans une organisation séquentielle indexée avec un **index creux**, l’index ne contient qu’une seule entrée par bloc de données. Cela signifie que certaines valeurs de clé sont représentées, mais pas toutes. Cela permet de réduire la taille de l'index comparé à un index dense, au prix d'une recherche un peu moins directe dans certains cas.

##### **Exemple avec Index Creux :**
- Supposons une table de 1 000 000 de tuples répartis dans 100 000 blocs, chaque bloc contenant 10 tuples. La taille totale de la table est de **400 Mo**.
- L’index creux contient une seule entrée par bloc, soit 100 000 entrées. Chaque entrée a une taille de 40 octets (30 pour la clé et 10 pour le pointeur), ce qui donne une taille d’index de **4 Mo**.
- La recherche dans un index creux implique le parcours de **log₂(1 000) ≈ 10 blocs**, ce qui nécessite une opération d’entrée/sortie pour obtenir le bloc de données correspondant.

#### **2.2 Recherche Séquentielle dans un Index Creux**

Prenons l'exemple d'une recherche avec une clé de valeur **15** :
- On commence par chercher la plus grande valeur de clé dans l'index qui est inférieure ou égale à **15**. Ici, la plus proche est **10**.
- Ensuite, une opération d’entrée/sortie est nécessaire pour accéder au bloc de données associé à la clé **10**. Ce processus peut entraîner des lectures de blocs inutiles si la clé recherchée n’existe pas dans les blocs parcourus.

#### **2.3 Index Multi-niveaux**

Pour améliorer les performances de recherche dans un index creux, on peut utiliser un **index multi-niveaux**. Ce type d’index repose sur l'idée de construire un index supplémentaire sur le premier index. 
- **Niveau 1** : Peut être dense pour permettre une recherche rapide.
- **Niveau 2** : Doit être creux pour éviter un stockage inutile de données.

##### **Exemple avec un Index Multi-niveaux :**
- Imaginons une table de 1 000 000 de tuples répartis dans 100 000 blocs.
- Le premier niveau d’index (creux) occupe 1 000 blocs.
- Un deuxième niveau d’index (dense) est ajouté pour pointer sur les blocs du premier niveau, et occupe seulement 10 blocs.
- La recherche d'une clé implique de parcourir **log₂(10) ≈ 4 blocs** avec deux opérations d’entrée/sortie : une pour accéder au bloc du niveau 1, puis une pour accéder au bloc de données correspondant.

#### **2.4 Arbres B-Tree et Variantes**

Les **arbres B-Tree** sont des structures d'index équilibrées qui permettent de maintenir les données triées, facilitant ainsi les opérations de recherche, d'insertion et de suppression de manière efficace. Contrairement à ISAM, qui exige une réorganisation des blocs lors des insertions, les arbres B-Tree permettent des insertions plus souples sans nécessiter de tri constant des données.

- **Noeuds branches** : Contiennent les clés et les pointeurs vers les noeuds enfants.
- **Noeuds feuilles** : Contiennent les clés et les pointeurs vers les enregistrements de la base de données.
- L'arbre est toujours équilibré, c’est-à-dire que la longueur des chemins de la racine aux feuilles est toujours la même.

##### **Variantes des B-Trees :**
- **B+-Tree** : Les feuilles contiennent toutes les clés et sont doublement chaînées pour faciliter la recherche séquentielle. Les noeuds internes contiennent uniquement des pointeurs et des clés.
- **B*-Tree** : Les noeuds sont remplis à 2/3 minimum, augmentant ainsi l'utilisation de l'espace dans l'arbre.

##### **Exemple d’un B+-Tree :**
- Hauteur constante : Un arbre de hauteur 2 peut contenir des millions d'enregistrements grâce à sa structure équilibrée.
- Les noeuds sont organisés de manière à minimiser le nombre d’opérations d'entrée/sortie lors des parcours de l’arbre.

#### **2.5 Opérations sur les B-Trees**

Les opérations de recherche, d’insertion et de suppression dans un B-Tree se font toutes de manière efficace, avec une complexité logarithmique.
- **Recherche** : Le parcours est fait depuis la racine jusqu’aux feuilles. À chaque noeud, la clé est comparée aux clés présentes pour déterminer si l’on doit suivre le pointeur gauche (pour les valeurs inférieures) ou le pointeur droit (pour les valeurs égales ou supérieures).
- **Insertion** : Si le noeud où l’on souhaite insérer une clé est plein, un processus appelé **éclatement** se produit. L'éclatement consiste à diviser le noeud en deux et à remonter la clé médiane au noeud parent. Ce processus peut se propager jusqu’à la racine, si nécessaire.

##### **Exemple d'insertion dans un B-Tree :**
- Si l’on souhaite insérer une nouvelle valeur **35** dans un arbre où les valeurs dans un noeud sont déjà pleines, l’éclatement se produit, et le noeud est divisé en deux. La clé médiane (par exemple **35**) est promue au noeud parent, et l’arbre reste équilibré.

#### **2.6 Taille et Efficacité d’un B-Tree**

L'efficacité d'un arbre B-Tree dépend de la hauteur de l'arbre et de l'espace qu'il occupe en mémoire.
- **Taille de l'arbre** : Un bloc de 4 Ko peut contenir environ 340 clés (si chaque clé fait 4 octets et chaque pointeur 8 octets). Pour un arbre d'une hauteur de 3, cela signifie que plus de 16 millions d'enregistrements peuvent être indexés.
- **Parcours de l'arbre** : Avec une hauteur de 3, il suffit de 3 opérations d’entrée/sortie pour accéder à n'importe quel enregistrement.
### **Partie 3 : Index B-Tree, Bitmap et Hachage**

#### **3.1 B+-Tree Oracle**

Dans les bases de données Oracle, les **index B+-Tree** sont utilisés pour organiser les données en structures d'index équilibrées qui permettent des recherches efficaces.

##### **Caractéristiques d’un B+-Tree Oracle** :
- L'index est **dense**, c'est-à-dire que chaque clé est représentée dans les feuilles de l'arbre.
- Les enregistrements dans les feuilles sont constitués de la clé d'index et d’un `rowid`, qui permet de localiser l'enregistrement dans le fichier de données.
- Contrairement à d’autres systèmes, le fichier de données n’est pas nécessairement trié. Il peut être une simple structure en tas (*heap file*), où les enregistrements sont insérés de manière non ordonnée.

##### **Exemple d'index secondaire et non plaçant** :
Un **index secondaire** ne modifie pas la façon dont les blocs de données sont organisés. L'index pointe simplement vers les blocs contenant les enregistrements correspondant aux clés recherchées, mais les données ne sont pas ordonnées selon cet index.

#### **3.2 Création d’Index B-Tree**

Oracle permet la création explicite d’index B-Tree via des requêtes SQL. Ces index peuvent être **uniques** (c'est-à-dire qu'aucune valeur dupliquée n’est autorisée dans la colonne indexée) ou **non uniques**.

##### **Exemples de commandes SQL pour gérer les index B-Tree :**
- **Création d’un index unique** :
   ```sql
   CREATE UNIQUE INDEX com_idx ON commune (code_insee);
   ```
- **Création d’un index sur une fonction** :
   ```sql
   CREATE INDEX com_idx ON commune(lower(nom_com));
   ```
- **Désactivation d’un index** :
   ```sql
   ALTER INDEX com_idx DISABLE;
   ```
- **Suppression d’un index** :
   ```sql
   DROP INDEX com_idx;
   ```
- **Reconstruction d’un index** (par exemple, après l’avoir rendu inutilisable) :
   ```sql
   ALTER INDEX commune_pk REBUILD;
   ```

#### **3.3 Table Organisée en Index B-Tree**

Une **table organisée en index B-Tree** signifie que l’organisation physique des données est basée sur l’index B-Tree. Cela optimise les performances des opérations de recherche, car les enregistrements sont stockés de manière ordonnée en fonction de l’index.

##### **Exemple de création d’une table organisée en index :**
```sql
CREATE TABLE nomTable (
    attr1 datatype1, 
    attr2 datatype2, 
    ...
) ORGANIZATION INDEX;
```

#### **3.4 Index Bitmap**

Les **index bitmap** sont particulièrement efficaces lorsque le nombre de valeurs distinctes d’un attribut est faible, comme pour des colonnes contenant des données de type booléen ou des petites catégories (par exemple, le genre ou le statut d’un employé).

##### **Principe d’un index bitmap :**
- Chaque valeur de clé est représentée par un vecteur de bits.
- Chaque bit dans le vecteur représente un tuple dans la table : `1` signifie que le tuple possède la valeur de clé correspondante, `0` signifie qu’il ne l’a pas.
  
##### **Exemple avec l'attribut Genre dans une table EMP :**
- Si nous avons une table avec les colonnes **Num**, **Nom**, **Genre** (M ou F), et **Fonction**, l'index bitmap sur le genre se représenterait comme suit :
  | Num | Nom    | Genre | Fonction   |
  |-----|--------|-------|------------|
  | 1   | Martin | M     | Ingénieur  |
  | 2   | Dupond | F     | Président  |
  | 3   | Dupont | M     | Commercial |
  | 4   | Dubois | F     | Ingénieur  |

  **Vecteurs de bits** pour Genre :
  - F: `0 1 0 1`
  - M: `1 0 1 0`

##### **Calcul de la taille d’un index bitmap :**
La taille de l'index est déterminée par la cardinalité de l'attribut (le nombre de valeurs distinctes) et le nombre de tuples dans la table. Pour un bloc de 8 Ko, la taille d'un vecteur est donnée par le nombre de tuples divisé par 8, avec un codage de 8 bits par octet.

##### **Exemple d’index bitmap sur une fonction :**
Pour un index bitmap sur l’attribut **fonction** (exemple ci-dessus), chaque fonction aurait son propre vecteur de bits, facilitant les requêtes d'agrégats, comme :
```sql
SELECT COUNT(*) FROM EMP WHERE fonction = 'président';
```
Cette requête reviendrait simplement à compter les `1` dans le vecteur correspondant à la fonction 'président'.

#### **3.5 Index de Hachage**

Les **index de hachage** sont utilisés pour les recherches basées sur des correspondances exactes. Contrairement aux B-Trees et aux index bitmap, les index de hachage sont moins adaptés aux recherches par intervalles ou aux comparaisons d'inégalités (par exemple, >, <).

##### **Fonctionnement de l'index de hachage :**
- Une **fonction de hachage** est appliquée à la clé de recherche pour déterminer l'adresse ou la position du tuple dans une table.
- Cela permet une recherche très rapide lorsque la clé exacte est connue.

##### **Exemple d’utilisation d’un index de hachage :**
Supposons que nous ayons une table EMP avec un attribut **Num** (numéro d’employé) et que nous créons un index de hachage sur cet attribut :
```sql
CREATE TABLE EMP1 (
    Num INTEGER, 
    Nom VARCHAR(10), 
    Genre VARCHAR(1), 
    Fonction VARCHAR(15)
) PARTITION BY HASH (Num) PARTITIONS 20;
```
Ici, l'index de hachage permet de partitionner la table sur l’attribut **Num**, ce qui accélère la recherche d’un employé en particulier.

##### **Gestion des collisions** :
Les fonctions de hachage peuvent produire des **collisions** (plusieurs clés mappées sur la même adresse). Il existe plusieurs techniques pour résoudre ces collisions, comme l'utilisation de chaînes ou de zones de débordement.
