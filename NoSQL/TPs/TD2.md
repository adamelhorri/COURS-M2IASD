---
tags:
  - nosql
  - tps
---
### Analyse et Correction détaillée du TD2 sur Neo4J

#### 1. Préambule Neo4J

Dans ce TD, nous utilisons Neo4J en version 3.5.21 pour intégrer des plugins spécifiques, comme **APOC** et **Neosemantics**, qui étendent les capacités de la base de données, notamment pour l’import/export de données RDF.

##### 1.1. Configuration de Neo4J
Pour permettre l'import et l'export RDF, deux configurations doivent être ajoutées dans le fichier `neo4j.conf` :
- **dbms.unmanaged_extension_classes=semantics.extension=/rdf**
- **apoc.export.file.enabled=true**

Cela permet d'activer les extensions nécessaires pour exploiter pleinement les fonctionnalités de Neo4J avec RDF et APOC. Une fois les configurations faites, le serveur est démarré via la commande :
```bash
./bin/neo4j start
```

---

#### 2. Enrichissement du Modèle

Cet exercice demande d’ajouter les maires successifs de Montpellier comme entités du graphe.

##### 2.1. Création des objets `Personne` et leurs relations

Nous allons créer des nœuds `Personne` représentant les maires de Montpellier et établir des relations entre ces personnes et la commune de Montpellier avec la relation `ADMINISTREE_PAR`. Voici l’exemple de commande Cypher à exécuter :

```cypher
MATCH (c:Commune {name: 'MONTPELLIER'})
CREATE 
  (gf:Personne {nom: "FRECHE", prenom: "GEORGES"}) <-[ap1:ADMINISTREE_PAR {date_debut: 1997, date_fin: 2004}]-(c),
  (hm:Personne {nom: "MANDROUX", prenom: "HELENE"}) <-[ap2:ADMINISTREE_PAR {date_debut: 2004, date_fin: 2014}]-(c),
  (ps:Personne {nom: "SAUREL", prenom: "PHILIPPE"}) <-[ap3:ADMINISTREE_PAR {date_debut: 2014, date_fin: 2020}]-(c),
  (md:Personne {nom: "DELAFOSSE", prenom: "MICKAEL"}) <-[ap4:ADMINISTREE_PAR {date_debut: 2020, date_fin: 2026}]-(c)
RETURN *;
```

**Explication :**
- La commande `CREATE` permet de créer quatre nœuds représentant les maires, chacun relié à la commune de Montpellier avec des informations sur les dates de début et fin de leur mandat via la relation `ADMINISTREE_PAR`.

---

#### 2.2 Exercices après enrichissement

##### 2.2.1. Ajouter un label `Maire` aux objets `Personne`

L’objectif ici est d’ajouter un label spécifique `Maire` à tous les objets de type `Personne` qui sont reliés à une commune par la relation `ADMINISTREE_PAR`.

```cypher
MATCH (p:Personne) <-[:ADMINISTREE_PAR]- (c:Commune)
SET p:Maire;
```

**Explication :**
- La requête utilise `MATCH` pour identifier toutes les personnes qui sont reliées à une commune via `ADMINISTREE_PAR` et leur assigne le label `Maire` avec `SET`.

##### 2.2.2. Retourner le nom du maire dont le mandat a débuté en 2020

Il s'agit de trouver le maire actuel de Montpellier, en filtrant par l’année de début de mandat.

```cypher
MATCH (md:Maire) <-[ap:ADMINISTREE_PAR]- (c:Commune)
WHERE ap.date_debut = 2020
RETURN md.nom;
```

**Explication :**
- `MATCH` identifie le maire (avec le label `Maire`) dont la relation `ADMINISTREE_PAR` avec Montpellier a débuté en 2020.

##### 2.2.3. Lister les procédures et fonctions disponibles dans Neo4J

Pour lister toutes les procédures et fonctions disponibles après l’installation des plugins **APOC** et **Neosemantics**, utilisez les commandes suivantes :

```cypher
CALL dbms.procedures();
CALL dbms.functions();
CALL db.schema();
```

**Explication :**
- `CALL dbms.procedures()` renvoie la liste de toutes les procédures installées, tandis que `CALL dbms.functions()` montre toutes les fonctions disponibles, et `CALL db.schema()` permet de visualiser le schéma du graphe.

##### 2.2.4. Exporter des données au format JSON

Nous allons exporter une partie des objets du graphe (communes, départements, régions) au format JSON. Cette exportation est sauvegardée dans un fichier dans le répertoire `import`.

```cypher
CALL apoc.export.json.query(
  "MATCH (a:Commune) -[w1:WITHIN]-> (d:Departement) -[w2:WITHIN]-> (r:Region)
   RETURN id(a), labels(a), a.name, d.name, r.name", 
  "test.json", {}
);
```

**Explication :**
- Cette commande exporte les informations des communes, des départements et des régions au format JSON. Des alternatives comme CSV ou GraphML sont aussi possibles avec les commandes `apoc.export.csv.query` et `apoc.export.graphml.all`.

##### 2.2.5. Comparaison de deux requêtes similaires

Les deux requêtes suivantes semblent proches, mais elles donnent des résultats différents :

**Première requête :**
```cypher
MATCH (c1:Commune) -[:NEARBY]- (inter:Commune) -[:NEARBY]- (mpl:Commune {name: 'MONTPELLIER'})
MATCH (c2:Commune) -[:NEARBY]- (inter:Commune) -[:NEARBY]- (mpl:Commune {name: 'MONTPELLIER'})
WHERE c1.name <> c2.name
RETURN c1, c2;
```

**Deuxième requête :**
```cypher
MATCH (c1:Commune) -[:NEARBY]- (inter:Commune) -[:NEARBY]- (mpl:Commune {name: 'MONTPELLIER'}),
      (c2:Commune) -[:NEARBY]- (inter:Commune) -[:NEARBY]- (mpl:Commune {name: 'MONTPELLIER'})
WHERE c1.name <> c2.name
RETURN c1, c2;
```

**Explication :**
- La première requête exécute deux correspondances distinctes pour `c1` et `c2`, tandis que la seconde impose que `c1` et `c2` ne réutilisent pas les mêmes relations plusieurs fois, conformément à la règle d’isomorphisme dans Neo4J. Par conséquent, la seconde requête ne retourne aucun résultat car elle ne permet pas l’utilisation répétée des mêmes relations dans un seul `MATCH`.

---

#### 3. Utilisation du plugin Neosemantics

Neosemantics permet de manipuler des graphes Neo4J en utilisant des données RDF. Ces exercices vous permettent de convertir des données du modèle en RDF et d’interagir avec celles-ci.

##### 3.1. Renvoi du modèle de connaissances en RDF

Pour obtenir une représentation RDF de tout le modèle de connaissances dans Neo4J, utilisez la commande suivante :

```bash
:GET /rdf/onto
```

**Explication :**
- La requête renvoie uniquement les propriétés `rdfs:label` des nœuds et relations. Toutes les propriétés concrètes (comme les attributs) sont absentes dans cette exportation RDF.

##### 3.2. Description d'un nœud au format RDF

Vous pouvez aussi obtenir une description détaillée d’un nœud spécifique, comme la commune de Montpellier, en utilisant son identifiant interne :

1. **Récupérer l'ID :**
   ```cypher
   MATCH (m:Commune {name: 'MONTPELLIER'}) 
   RETURN ID(m);
   ```

2. **Obtenir la description RDF :**
   ```bash
   :GET /rdf/describe/id/37
   ```

**Explication :**
- Contrairement à la requête précédente, cette requête retourne toutes les propriétés littérales du nœud, mais sans les types de données associés.

##### 3.3. Export des relations de Montpellier et ses maires au format RDF

Pour exporter les relations entre Montpellier et ses maires, voici la commande :

```bash
:POST /rdf/cypher { 
  "cypher": "MATCH path = (c:Commune {name:'MONTPELLIER'})-[ap1:ADMINISTREE_PAR]-> (p:Personne) RETURN path",
  "format": "N3"
}
```

**Explication :**
- Cette requête retourne les informations sur la commune de Montpellier et ses maires sous forme de chemin RDF, incluant les relations `ADMINISTREE_PAR`.

---
### 4. Import de données RDF

Il est possible d'importer des données RDF dans Neo4J à partir de sources externes via des points d'accès **SPARQL**. Ces données peuvent provenir de bases de connaissances publiques comme **Wikidata**. Nous allons explorer le processus d’importation et de manipulation des données RDF en Neo4J à l’aide du plugin **Neosemantics**.

#### 4.1. Préparation pour l’import de données RDF

Avant d’importer des données RDF, il est nécessaire de configurer la base de données en créant un index pour les objets `Resource` (au sens RDF) afin de garantir des performances optimales lors des requêtes. Voici comment créer cet index :

```cypher
CREATE INDEX ON :Resource(uri);
```

**Explication :**  
L’indexation sur l’attribut `uri` de l’objet `Resource` accélère la recherche et la manipulation des nœuds RDF dans le graphe Neo4J.

#### 4.2. Import de données RDF depuis Wikidata

Nous allons importer des informations sur des communes à partir du point d'accès SPARQL de **Wikidata**. La requête suivante, écrite en **SPARQL**, extrait des informations sur les communes de l’Hérault dont le code INSEE commence par "34". Ces informations incluent les noms des communes et les relations géographiques entre elles.

**Requête SPARQL et import des données RDF** :
```cypher
WITH '
PREFIX sch: <http://schema.org/>
CONSTRUCT {
  ?item a sch:City;
  sch:name ?itemLabel;
  sch:address ?inseeCode;
  sch:geoTouches ?otherItem.
  ?otherItem a sch:City;
  sch:name ?otheritemLabel;
  sch:address ?otherinseeCode.
}
WHERE {
  ?item wdt:P374 ?inseeCode.
  ?item wdt:P47 ?otherItem.
  ?item rdfs:label ?itemLabel.
  FILTER(lang(?itemLabel) = "fr").
  ?otherItem rdfs:label ?otheritemLabel.
  FILTER(lang(?otheritemLabel) = "fr").
  FILTER regex(?inseeCode, "^34").
}
LIMIT 400' AS sparql
CALL semantics.importRDF(
  "https://query.wikidata.org/sparql?query=" + apoc.text.urlencode(sparql), 
  "JSON-LD", 
  {headerParams: {Accept: "application/ld+json"}}
)
YIELD terminationStatus, triplesLoaded, namespaces, extraInfo
RETURN terminationStatus, triplesLoaded, namespaces, extraInfo;
```

**Explication :**
- **Requête SPARQL :** La requête extrait des informations RDF sur des entités de type `City`, notamment leur nom (`sch:name`), leur code INSEE (`sch:address`), et leurs relations géographiques avec d'autres communes via `sch:geoTouches`.
- **Import dans Neo4J :** La commande `semantics.importRDF` utilise le résultat de la requête SPARQL pour importer les données RDF dans Neo4J. Les données sont formatées en **JSON-LD**, un format couramment utilisé pour les graphes RDF.
- **Résultats :** Les informations sur l'importation, telles que le nombre de triplets chargés et les namespaces utilisés, sont retournées à la fin de l'import.

#### 4.3. Questions d’appropriation sur les données RDF importées

Après l'importation des données, voici une série d'exercices pour mieux comprendre et manipuler les données RDF importées dans le graphe Neo4J.

##### 4.3.1. Expliquer la requête SPARQL et dessiner un graphe simplifié

La requête SPARQL construite récupère des informations sur des communes de l’Hérault. Elle extrait les noms (`sch:name`) et les relations de voisinage (`sch:geoTouches`) entre elles. Un graphe simple illustrant deux communes et leur relation peut ressembler à ceci :

```plaintext
[Montpellier] --geoTouches--> [Lattes]
```

- **Montpellier** et **Lattes** sont deux communes dont les noms sont extraits par la requête.
- La relation `geoTouches` signifie que ces communes sont adjacentes géographiquement.

##### 4.3.2. Lier les communes importées au département de l’Hérault

Après l’import des communes, nous devons les relier au département correspondant. Voici comment ajouter une relation `WITHIN` entre les communes et le département de l’Hérault :

```cypher
MATCH (ci:sch__City)
MATCH (n:Departement {id: '34'})
CREATE (ci) -[:WITHIN]-> (n);
```

**Explication :**  
- `MATCH` permet de sélectionner toutes les communes (`sch__City`) et de trouver le nœud représentant le département de l'Hérault (ID : 34).
- Ensuite, `CREATE` ajoute la relation `WITHIN` pour relier chaque commune au département.

##### 4.3.3. Suppression des nœuds de type `Commune`

Pour supprimer tous les nœuds `Commune` du graphe, ainsi que leurs relations, utilisez la commande suivante :

```cypher
MATCH (co:Commune) DETACH DELETE co;
```

**Explication :**  
- `DETACH DELETE` supprime les nœuds `Commune` ainsi que toutes les relations qui leur sont associées, garantissant que le graphe reste cohérent après la suppression.

##### 4.3.4. Compter les communes limitrophes de Montpellier

Pour compter le nombre de communes qui sont adjacentes à Montpellier (voisines via la relation `geoTouches`), utilisez cette commande :

```cypher
MATCH (m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches]- (x)
RETURN count(x) as limitrophes;
```

**Explication :**  
- La relation `sch__geoTouches` est utilisée pour identifier les communes limitrophes de Montpellier. Le `count(x)` retourne le nombre total de ces communes.

---

### 5. Questions sur les chemins

Les exercices suivants explorent les chemins dans le graphe entre différentes communes.

##### 5.1. Trouver le plus court chemin entre Montpellier et Beaulieu

```cypher
MATCH p = shortestpath((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
RETURN p;
```

**Explication :**
- La fonction `shortestpath` permet de trouver le chemin le plus court entre deux communes (ici, Montpellier et Beaulieu) en traversant les relations `sch__geoTouches`.

##### 5.2. Retourner tous les plus courts chemins entre Montpellier et Beaulieu

Pour retourner tous les plus courts chemins possibles entre les deux communes, on utilise `allshortestpaths` :

```cypher
MATCH p = allshortestpaths((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
RETURN p;
```

**Explication :**
- Contrairement à `shortestpath`, qui retourne un seul chemin, `allshortestpaths` retourne tous les chemins les plus courts entre deux nœuds.

##### 5.3. Extraire les noms des cités traversées par un des plus courts chemins

```cypher
MATCH p = shortestpath((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
RETURN extract(n in nodes(p) | n.sch__name) as noeudsDuChemin;
```

**Explication :**  
- La fonction `extract` est utilisée pour extraire les noms des nœuds traversés le long du chemin.

##### 5.4. Renvoyer le nom et l’adresse des cités traversées

Pour obtenir non seulement les noms des cités mais aussi leurs adresses (codes INSEE), utilisez cette commande :

```cypher
MATCH p = shortestpath((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
RETURN extract(n IN nodes(p) | {name: n.sch__name, codeInsee: n.sch__address});
```

**Explication :**  
- En plus des noms des communes traversées, cette requête retourne également leurs codes INSEE (représentés par l'attribut `sch__address`).

##### 5.5. Compter le nombre de plus courts chemins entre Montpellier et Beaulieu

```cypher
MATCH p = allshortestpaths((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
RETURN count(p);
```

**Explication :**  
- Cette requête compte le nombre total de chemins les plus courts entre Montpellier et Beaulieu.

##### 5.6. Renvoyer tous les chemins entre Montpellier et Beaulieu (et optimisation)

La recherche de **tous les chemins** entre deux nœuds dans un graphe peut devenir une opération coûteuse, surtout si le graphe est de grande taille ou contient de nombreuses relations. Neo4J permet cependant de trouver tous les chemins possibles entre deux nœuds via une requête Cypher.

Voici la commande de base pour renvoyer **tous les chemins** entre Montpellier et Beaulieu en traversant les relations `sch__geoTouches` :

```cypher
MATCH p = ((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
RETURN p;
```

**Explication :**
- La requête parcourt toutes les relations de voisinage `sch__geoTouches` entre **Montpellier** et **Beaulieu**, renvoyant ainsi tous les chemins possibles entre ces deux villes.

**Optimisation** :  
Renvoyer tous les chemins dans un graphe très connecté peut rapidement devenir trop coûteux en termes de ressources (temps de calcul, mémoire, etc.). Pour éviter cela, il est possible de limiter le nombre de chemins retournés en utilisant la clause `LIMIT` :

```cypher
MATCH p = ((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
RETURN p LIMIT 5;
```

**Explication :**  
- La clause `LIMIT 5` permet de renvoyer uniquement les cinq premiers chemins trouvés. Cela réduit considérablement la complexité de la requête et l'utilisation des ressources.

**Autres Optimisations** :
1. **Indexation** :  
   Créer des index sur les propriétés souvent utilisées dans les requêtes, comme `sch__name`, peut améliorer les performances. Par exemple :
   ```cypher
   CREATE INDEX ON :sch__City(sch__name);
   ```

2. **Utilisation de la profondeur** :  
   Limiter la profondeur des relations à explorer peut aussi aider à contrôler la complexité de la recherche. Par exemple :
   ```cypher
   MATCH p = ((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*1..3]- (g:sch__City {sch__name: 'Beaulieu'}))
   RETURN p;
   ```
   Cette requête limite la recherche à des chemins ayant une profondeur de 1 à 3 relations `sch__geoTouches`.

---

##### 5.7. Renvoyer un des plus courts chemins qui ne passe pas par Clapiers

Une des questions courantes dans l’analyse des chemins est de trouver un chemin qui évite certains nœuds spécifiques. Ici, nous souhaitons trouver un des plus courts chemins entre **Montpellier** et **Beaulieu**, mais qui ne passe pas par **Clapiers**.

Voici la requête pour effectuer cette opération :

```cypher
MATCH p = shortestpath((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
WHERE NOT ('Clapiers' IN [n IN nodes(p) | n.sch__name])
RETURN p;
```

**Explication :**
- **shortestpath()** : Cette fonction trouve le chemin le plus court entre Montpellier et Beaulieu.
- **WHERE NOT** : La condition `WHERE NOT` vérifie que le chemin trouvé ne contient aucun nœud ayant pour nom "Clapiers".
- **[n IN nodes(p) | n.sch__name]** : Cette partie de la requête extrait tous les noms des nœuds le long du chemin et vérifie que **Clapiers** n’en fait pas partie.

---

##### 5.8. Renvoyer tous les plus courts chemins qui passent par Lattes

Dans cet exercice, nous cherchons tous les plus courts chemins entre Montpellier et Beaulieu qui passent obligatoirement par la commune de **Lattes**. Pour cela, nous utilisons la fonction `allshortestpaths` et une condition `WHERE` pour s'assurer que **Lattes** est présent dans le chemin.

Voici la requête :

```cypher
MATCH p = allshortestpaths((m:sch__City {sch__name: 'Montpellier'}) -[:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
WHERE 'Lattes' IN [n IN nodes(p) | n.sch__name]
RETURN p;
```

**Explication :**
- **allshortestpaths()** : Cette fonction retourne tous les chemins les plus courts entre **Montpellier** et **Beaulieu**.
- **WHERE 'Lattes' IN [...]** : Cette condition filtre les chemins en ne conservant que ceux qui passent par **Lattes**.
- **[n IN nodes(p) | n.sch__name]** : Cela permet d'extraire les noms de toutes les communes présentes dans le chemin et de vérifier la présence de **Lattes**.

---

##### 5.9. Trouver des chemins en limitant la distance maximale parcourue

Dans certains cas, il est utile de limiter la distance maximale parcourue dans les chemins. Si les relations `sch__geoTouches` incluent une propriété `distance`, il est possible de filtrer les chemins en fonction de la distance totale.

Voici une requête qui retourne les chemins entre Montpellier et Beaulieu dont la distance totale ne dépasse pas une certaine valeur (par exemple, 20 km) :

```cypher
MATCH p = allshortestpaths((m:sch__City {sch__name: 'Montpellier'}) -[r:sch__geoTouches*]- (g:sch__City {sch__name: 'Beaulieu'}))
WHERE reduce(s = 0, rel IN relationships(p) | s + rel.distance) <= 20
RETURN p;
```

**Explication :**
- **reduce()** : Cette fonction accumule les distances de chaque relation dans le chemin et s'assure que la somme totale ne dépasse pas 20 km.
- **relationships(p)** : Cela extrait toutes les relations du chemin, permettant d'accéder à leur propriété `distance`.

---

### Conclusion

Ce TD a permis d'explorer plusieurs aspects avancés de la gestion des chemins dans un graphe Neo4J, notamment l'optimisation des requêtes de chemin, l'exclusion de nœuds spécifiques, l'inclusion de nœuds obligatoires, et la gestion de la distance dans les chemins. Grâce à des fonctionnalités comme `shortestpath`, `allshortestpaths`, et `reduce`, Neo4J offre une grande flexibilité pour analyser et manipuler les chemins dans un graphe complexe, tout en permettant d'améliorer les performances via des optimisations comme `LIMIT` et l'indexation des propriétés importantes.

Ces techniques sont particulièrement utiles pour des applications pratiques telles que les systèmes d'information géographique, les réseaux sociaux, ou encore les systèmes de recommandation, où les relations et les distances entre entités jouent un rôle clé dans l'analyse des données.