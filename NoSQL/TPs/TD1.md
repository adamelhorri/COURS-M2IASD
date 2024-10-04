---
tags:
  - nosql
  - tps
---

### Analyse et Correction du TD1 sur Neo4J

#### 1. Préalable Neo4J

**Neo4J** est un système de gestion de base de données orienté graphe, idéal pour représenter des réseaux d'entités fortement interconnectées. Dans cet exercice, nous utilisons la version **community 3.5.21** de Neo4J pour assurer la compatibilité avec certains plugins, notamment **neosemantics**. L'exercice nécessite l'environnement d'exécution **Java 11**.

#### 2. CQL (Cypher Query Language) - Exercices d’appropriation

Les exercices portent sur les opérations principales **CRUD** (Create, Read, Update, Delete) dans un graphe Neo4J.

### 2.1. Exercice 1 - Manipuler les commandes `CREATE` et `MATCH`

#### 2.1.1. Création de nœuds et de relations

L'objectif de cet exercice est de créer des communes et des départements, ainsi que les relations entre eux. Voici un exemple de création de relations `WITHIN` (commune dans un département) et `NEARBY` (communes proches).

```cypher
CREATE (m:Commune {nom: 'MONTPELLIER', latitude: 43.610769, longitude: 3.876716, codeinsee: '34172'})
-[:WITHIN]-> (h:Departement {nom: 'HERAULT', numero: '34'}),
(h) <-[:WITHIN]- (l:Commune {nom: 'LUNEL', latitude: 43.67445, longitude: 4.135366, codeinsee: '34145'}),
(m) -[:NEARBY]-> (l),
(c:Commune {nom: 'CLAPIERS', latitude: 43.6575, longitude: 3.88833, codeinsee: '34830', pop_17: 5135}) -[:WITHIN]-> (h),
(m) -[:NEARBY {type: 'border'}]-> (c)
RETURN l, m, h, c;
```

**Explication** : 
- `CREATE` permet de créer des nœuds (communes et départements) et de définir des relations `WITHIN` (appartenance à un département) et `NEARBY` (communes proches).
- Le retour de la commande (`RETURN`) affiche les nœuds créés pour vérifier leur existence.

#### 2.1.2. Mise à jour d'objets avec `SET`

Cet exercice montre comment utiliser la commande `SET` pour mettre à jour des propriétés sur des nœuds et des relations. Exemple :

```cypher
MATCH (s:Commune {nom: 'SOMMIERES'})
SET s.pop_1975 = 3072, s.latitude = 43.783450, s.longitude = 4.089738
RETURN s;
```

Ici, nous mettons à jour les attributs de la commune de Sommières.

**Explication** :
- `MATCH` est utilisé pour trouver un nœud correspondant à une commune.
- `SET` permet de modifier ou d’ajouter des propriétés à un nœud. Ici, nous ajoutons des informations de population et de coordonnées géographiques.

#### 2.1.3. Utilisation de `MERGE`

La commande **MERGE** combine les fonctionnalités de `CREATE` et `MATCH`. Si le nœud ou la relation existe déjà, il n'est pas recréé, sinon il est créé.

Exemple :
```cypher
MATCH (c1:Commune)-[:NEARBY]->()<-[:NEARBY]-(c2:Commune)
MERGE (c1)-[:NEARBY]-(c2);
```

**Explication** :
- `MERGE` permet de créer la relation entre deux nœuds si elle n’existe pas encore, sans créer de doublon.

### 2.2. Exercice de consultation avec `MATCH` et `RETURN`

#### 2.2.1. Rechercher les communes de l'Hérault

```cypher
MATCH (d:Departement {nom: 'HERAULT'}) <-[p:WITHIN]- (n:Commune)
RETURN d, n, p;
```

**Explication** : 
- Cette requête retourne toutes les communes du département de l'Hérault. `WITHIN` montre la relation d'appartenance entre une commune et son département.

#### 2.2.2. Communes proches de Montpellier

```cypher
MATCH (m:Commune {nom: 'MONTPELLIER'}) -[:NEARBY]- (n:Commune)
RETURN m, n;
```

**Explication** : 
- Cette requête retourne les communes qui ont une relation `NEARBY` avec Montpellier, permettant d'obtenir les communes proches de Montpellier.

#### 2.2.3. Rechercher les chemins entre Montpellier et Nîmes

```cypher
MATCH p=((m:Commune {nom: 'MONTPELLIER'})-[:NEARBY*]-(n:Commune {nom: 'NIMES'}))
RETURN p;
```

**Explication** :
- Ici, nous récupérons tous les chemins entre Montpellier et Nîmes via des relations `NEARBY`. L'étoile (`*`) permet de parcourir un nombre quelconque de relations entre les deux communes.

### 2.3. Exercice de suppression avec `DELETE` et `DETACH DELETE`

Cet exercice se concentre sur la suppression de nœuds, de relations ou d'attributs spécifiques.

#### 2.3.1. Suppression de tous les objets du graphe

```cypher
MATCH (n) OPTIONAL MATCH (n)-[r]-() DELETE n, r;
```

**Explication** :
- Cette commande supprime tous les nœuds et relations du graphe. La clause `OPTIONAL MATCH` assure que toutes les relations associées à un nœud sont également supprimées.

#### 2.3.2. Suppression d'un attribut de nœud

```cypher
MATCH (n:Commune {nom: 'NIMES'})
REMOVE n.codeinsee
RETURN n;
```

**Explication** :
- La commande `REMOVE` permet de supprimer une propriété d'un nœud sans le supprimer lui-même. Ici, nous retirons la propriété `codeinsee` de la commune de Nîmes.

### 3. Import de données avec Cypher

Cet exercice porte sur l'importation de données en format CSV dans Neo4J.

Exemple d'importation :
```cypher
LOAD CSV WITH HEADERS FROM 'file:///Region.csv' AS regions
CREATE (r:Region {id: toInteger(regions.id), name: regions.name});
```

**Explication** :
- La commande `LOAD CSV` permet de charger un fichier CSV contenant des données de régions. `WITH HEADERS` indique que la première ligne du fichier contient les noms des colonnes.

### 4. Exercices de consultation avancée

Voici quelques exemples de requêtes Cypher utilisées pour consulter des informations dans le graphe.

#### 4.1. Compter le nombre de départements dans la région Occitanie

```cypher
MATCH (d:Departement) -[:WITHIN]-> (r:Region {name: 'OCCITANIE'})
RETURN count(d) as nombreDep;
```

**Explication** :
- Cette requête compte le nombre de départements associés à la région Occitanie en utilisant la relation `WITHIN`.

#### 4.2. Retourner les communes voisines de Montpellier et leurs départements

```cypher
MATCH (m:Commune {nom: 'MONTPELLIER'}) -[:NEARBY]- (v:Commune) -[:WITHIN]-> (d:Departement)
RETURN v.nom, d.nom;
```

**Explication** :
- Cette requête retourne le nom des communes voisines de Montpellier ainsi que le nom du département auquel elles appartiennent.

