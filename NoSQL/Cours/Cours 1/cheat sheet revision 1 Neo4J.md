---
tags:
  - nosql
  - cours
---
Fiche du cours [[Cours 1 Neo4J]]
#### 1. **Introduction à Neo4J**
Neo4J est un **SGBD orienté graphes** qui permet de modéliser et interroger des données fortement interconnectées. Contrairement aux bases de données relationnelles, Neo4J utilise un modèle **Labeled Property Graph (LPG)** composé de :
- **Nœuds** : Entités principales du graphe (ex. : `Person`, `City`, `Commune`).
- **Relations** : Arcs orientés reliant les nœuds (ex. : `KNOWS`, `NEARBY`).
- **Propriétés** : Attributs associés aux nœuds et aux relations (ex. : `nom`, `latitude`, `date_debut`).

#### 2. **Cypher Query Language (CQL)**
**Cypher** est le langage de requêtes utilisé pour manipuler des graphes dans Neo4J. Il permet de **créer**, **mettre à jour**, **supprimer** et **interroger** des données.

##### 2.1. **Commandes CRUD de base**
- **Création de nœuds et relations** :
  ```cypher
  CREATE (a:Person {nom: "Alice"}), (b:Person {nom: "Bob"})
  CREATE (a)-[:KNOWS {since: 2020}]->(b);
  ```
- **Requête simple** (récupérer les voisins de Montpellier) :
  ```cypher
  MATCH (m:Commune {nom: 'Montpellier'})-[:NEARBY]-(n:Commune)
  RETURN n;
  ```
- **Mise à jour de propriétés** :
  ```cypher
  MATCH (p:Person {nom: 'Alice'})
  SET p.age = 25;
  ```
- **Suppression de nœuds** (et relations associées) :
  ```cypher
  MATCH (n:Commune {nom: 'Montpellier'})
  DETACH DELETE n;
  ```

##### 2.2. **MATCH et WHERE**
- **MATCH** permet de rechercher des nœuds et des relations selon des critères précis.
- **WHERE** ajoute des conditions supplémentaires aux requêtes :
  ```cypher
  MATCH (p:Person)
  WHERE p.age > 30
  RETURN p;
  ```

##### 2.3. **Utilisation des chemins**
- Trouver le **plus court chemin** :
  ```cypher
  MATCH p = shortestpath((a:Person {nom: 'Alice'})-[:KNOWS*]-(b:Person {nom: 'Bob'}))
  RETURN p;
  ```
- Rechercher tous les chemins entre deux nœuds :
  ```cypher
  MATCH p = (m:Commune {nom: 'Montpellier'})-[:NEARBY*]- (n:Commune {nom: 'Beaulieu'})
  RETURN p;
  ```

#### 3. **Relations Complexes et Modèle Évolutif**
Neo4J offre la possibilité de gérer des graphes sans schéma rigide, ce qui permet d’ajouter des nœuds et relations de manière flexible. Cependant, il est utile de **conceptualiser le modèle de données** avant de manipuler les graphes.

Exemple de relations entre **communes** et **départements** avec des maires :
```cypher
MATCH (c:Commune {nom: 'MONTPELLIER'})
CREATE 
  (gf:Person {nom: "FRECHE", prenom: "GEORGES"}) <-[ap1:ADMINISTREE_PAR {date_debut: 1997, date_fin: 2004}]-(c);
```

#### 4. **Extensions APOC et Neosemantics**
**APOC** (Awesome Procedures On Cypher) et **Neosemantics** ajoutent des fonctionnalités avancées, telles que l'import/export de données RDF ou la gestion des inférences sémantiques.

##### 4.1. **Utilisation d'APOC pour l'export JSON**
APOC permet d'exporter des données dans divers formats comme JSON et CSV :
```cypher
CALL apoc.export.json.query(
  "MATCH (a:Commune)-[:WITHIN]->(d:Departement) RETURN id(a), a.name, d.name", 
  "output.json", {}
);
```

##### 4.2. **Neosemantics pour les données RDF**
**Neosemantics** permet d'importer et d'exporter des données au format RDF.  
Requête pour décrire un nœud au format RDF :
```bash
:GET /rdf/describe/id/123
```
Importer des données RDF depuis un endpoint SPARQL :
```cypher
CALL semantics.importRDF("https://query.wikidata.org/sparql?query=...", "JSON-LD", {})
```

#### 5. **Travail avec des Données Géographiques**
Neo4J est souvent utilisé pour modéliser des données géographiques, par exemple, les relations entre des **villes** ou **communes**.

##### 5.1. **Relations `NEARBY` et distance**
Pour modéliser des relations géographiques, on peut ajouter une propriété `distance` aux relations :
```cypher
MATCH (m:Commune {nom: 'Montpellier'})-[:NEARBY {distance: 15.5}]->(n:Commune {nom: 'Lattes'})
RETURN m, n;
```

##### 5.2. **Calculer des chemins en fonction de la distance**
Limiter la distance totale parcourue dans un chemin :
```cypher
MATCH p = (m:Commune {nom: 'Montpellier'})-[:NEARBY*]-(n:Commune {nom: 'Beaulieu'})
WHERE reduce(total = 0, rel in relationships(p) | total + rel.distance) <= 20
RETURN p;
```

#### 6. **Optimisation des Requêtes et Indexation**
Pour optimiser les performances des requêtes sur de grands graphes, il est crucial de créer des **index** sur les propriétés fréquemment recherchées.

Exemple d'indexation sur un attribut `nom` :
```cypher
CREATE INDEX ON :Commune(nom);
```

#### 7. **Étude de Cas : Chemins Complexes**
##### 7.1. **Renvoyer tous les chemins entre deux communes**
Limitation du nombre de chemins retournés :
```cypher
MATCH p = (m:Commune {nom: 'Montpellier'})-[:NEARBY*]- (g:Commune {nom: 'Beaulieu'})
RETURN p LIMIT 5;
```

##### 7.2. **Exclure certains nœuds d'un chemin**
Trouver un chemin qui ne passe pas par une commune donnée :
```cypher
MATCH p = shortestpath((m:Commune {nom: 'Montpellier'})-[:NEARBY*]- (g:Commune {nom: 'Beaulieu'}))
WHERE NOT ('Clapiers' IN [n IN nodes(p) | n.nom])
RETURN p;
```
