---
tags:
  - nosql
  - cours
---
### 1. Introduction au NoSQL

**NoSQL** est un terme utilisé pour désigner une variété de systèmes de gestion de bases de données qui s'écartent des systèmes relationnels traditionnels. Il regroupe diverses initiatives qui visent à compléter ou remplacer les modèles relationnels ou orientés objets. Les motivations derrière NoSQL proviennent de la nécessité d’adresser de nouvelles problématiques engendrées par l'évolution du web et des données.

#### 1.1. Pourquoi NoSQL ?
L'émergence de NoSQL a été motivée par plusieurs facteurs :
- **L'évolution du Web** : L'apparition de nouvelles plateformes (Google, Facebook, Amazon, Twitter) a entraîné un volume massif de données et une interconnexion accrue entre ces données.
- **Les limites des bases relationnelles** : Ces bases sont souvent inadaptées aux nouveaux besoins en raison de leur rigidité (schémas stricts), leur manque de flexibilité et leur difficulté à évoluer pour répondre à des modifications fréquentes.
- **Les besoins d’évolutivité** : Les bases relationnelles deviennent inefficaces lorsqu'il s'agit de servir des milliers, voire des millions d'utilisateurs simultanément avec des données distribuées.

#### 1.2. Quand opter pour un système NoSQL ?
Les systèmes NoSQL sont particulièrement adaptés lorsque :
- Les **schémas évoluent fréquemment** et doivent gérer des entités avec des attributs variés et des associations multiples.
- Les **flux transactionnels** sont élevés (lecture et écriture) et les données sont **distribuées mondialement** dès leur création.

### 2. Principes généraux des systèmes orientés graphes

Les systèmes orientés graphes représentent un sous-ensemble spécifique de NoSQL qui est particulièrement adapté pour gérer des données interconnectées. Le graphe devient un modèle de choix lorsqu'il s'agit de représenter des relations complexes entre entités.

#### 2.1. Positionnement contextuel
Les systèmes orientés graphes se distinguent par leur capacité à gérer à la fois des **volumes élevés de données** et une **grande complexité** en termes de relations et d'expressivité des données. Ce sont des systèmes qui se rapprochent des systèmes à objets ou navigationnels, tout en reposant sur les concepts de la **théorie des graphes**.

Le modèle de données utilisé est celui du **graphe attribué et orienté**. Ce type de graphe permet de représenter des entités (nœuds) et des relations (arcs) avec des attributs associés, facilitant ainsi la modélisation de données complexes.

### 3. Exemple de système : Neo4J

**Neo4J** est l'un des systèmes de gestion de bases de données orientés graphes les plus populaires. Il permet de stocker des données sous forme de graphes tout en offrant des mécanismes efficaces de requêtage et de manipulation des données.

#### 3.1. Extensions au système Neo4J
Neo4J propose des extensions permettant d'améliorer les capacités natives du système, notamment dans le cadre de l’évolution des besoins des utilisateurs. Ces extensions permettent d’intégrer des fonctionnalités supplémentaires pour répondre à des problématiques spécifiques, comme le traitement de graphes de grande taille ou la gestion de schémas complexes.

#### 3.2. Comparaison Neo4J vs Triplestore
Neo4J se distingue des systèmes **Triplestore** (qui stockent des triplets sujet-prédicat-objet) par sa flexibilité et sa capacité à gérer des graphes plus complexes. Les triplestores sont généralement utilisés pour des systèmes basés sur des logiques formelles, comme le Web sémantique, alors que Neo4J se prête à des applications plus généralistes et plus larges.

### 4. NoSQL, graphes et Neo4J : Panorama général

#### 4.1. NoSQL : caractéristiques et avantages
NoSQL se distingue des **SGBD relationnels** par une série de caractéristiques techniques et architecturales qui visent à améliorer la performance et l'évolutivité dans des contextes de très grandes bases de données :
- **Répartition et réplication** : Ces techniques permettent d’assurer une tolérance aux pannes et une distribution efficace des données sur plusieurs serveurs.
- **Simplification des schémas** : Contrairement aux schémas relationnels rigides et normalisés, NoSQL prône des modèles plus flexibles pour éviter la complexité inutile.
- **Alternative aux propriétés ACID** : Les systèmes NoSQL offrent des alternatives moins exigeantes en matière de transactions, en particulier pour des applications où la consistance forte n’est pas critique.

#### 4.2. Passage à l’échelle et Scalabilité
L’un des atouts majeurs des systèmes NoSQL est leur capacité à **passer à l’échelle** pour répondre à la montée en charge (nombre croissant d’utilisateurs, augmentation des transactions). Cette scalabilité se décline sous deux formes :
- **Scalabilité horizontale (scaling out)** : Ajout de serveurs supplémentaires pour répartir la charge.
- **Scalabilité verticale (scaling up)** : Amélioration des performances d’un serveur en ajoutant des ressources matérielles.

Dans un environnement NoSQL, la scalabilité horizontale est préférée, car elle permet de gérer plus efficacement un grand nombre de transactions en parallèle, en ajoutant simplement des serveurs pour augmenter la capacité.

### 5. Principes CAP et NoSQL

Le **théorème CAP** énonce qu'il est impossible pour un système distribué de satisfaire simultanément les trois propriétés suivantes :
1. **Consistance (C)** : Toute mise à jour de données doit être visible instantanément par tous les nœuds du système.
2. **Disponibilité (A)** : Chaque requête doit recevoir une réponse, même en cas de panne partielle.
3. **Tolérance aux partitions (P)** : Le système doit continuer à fonctionner correctement malgré la perte ou la déconnexion de certains nœuds.

Les **SGBD relationnels** privilégient la consistance et la disponibilité (C et A), mais ne sont pas conçus pour tolérer de grandes partitions (nombre limité de nœuds). En revanche, les systèmes NoSQL se concentrent principalement sur la tolérance aux partitions (P), en acceptant soit de réduire la consistance (C), soit de sacrifier la disponibilité (A) selon les besoins de l’application.

### 6. Typologie des systèmes NoSQL

Les systèmes NoSQL se classent en plusieurs catégories selon leur modèle de représentation des données :
- **Systèmes clé-valeur** : Stockent les données sous forme de paires clé/valeur.
- **Systèmes orientés colonne** : Organisent les données en colonnes plutôt qu’en lignes.
- **Systèmes orientés document** : Gèrent les données sous forme de documents (ex. JSON).
- **Systèmes orientés graphes** : Représentent les données sous forme de graphes.

Les **triplestores** et les systèmes **SGBDOO** (orientés objets) partagent des similarités avec les systèmes orientés graphes dans leur gestion des relations complexes entre entités.

### 7. Pourquoi les graphes ?

Les systèmes de gestion de graphes sont particulièrement adaptés pour représenter et manipuler des données fortement interconnectées. Les graphes offrent un modèle simple mais puissant pour représenter des réseaux complexes, qu'il s'agisse de réseaux sociaux, de systèmes biologiques ou de la structure des connaissances. Neo4J, avec sa capacité à naviguer efficacement dans des graphes de grande taille, se positionne comme un outil incontournable dans l’univers NoSQL.

### 8. Les graphes dans NoSQL

Les graphes jouent un rôle central dans les bases de données NoSQL, en particulier lorsqu'il s'agit de gérer des relations complexes entre les entités. Contrairement aux systèmes relationnels, qui structurent les données en tables et en colonnes, les systèmes orientés graphes permettent de modéliser des relations sous forme de nœuds (entités) et d'arcs (relations) de manière beaucoup plus flexible.

#### 8.1. Exemples concrets avec PlantUML

PlantUML est un excellent outil pour modéliser visuellement des graphes et des diagrammes, ce qui permet d’illustrer la structure d’une base de données orientée graphes comme Neo4J. Voici quelques exemples pour mieux comprendre la manière dont les données peuvent être représentées dans ces systèmes.

##### Exemple 1 : Modélisation d'une base de données relationnelle en graphe

```plantuml
@startuml
entity "Person" {
  *id : Integer
  *name : String
}

entity "Movie" {
  *id : Integer
  *title : String
}

entity "ActedIn" {
  *person_id : Integer
  *movie_id : Integer
  *role : String
}

Person -- ActedIn : "has acted"
ActedIn -- Movie : "in"
@enduml
```

Dans cet exemple, la relation entre une personne et un film est représentée par une entité intermédiaire `ActedIn`, qui stocke des informations supplémentaires comme le rôle joué par la personne dans le film. Le diagramme montre clairement comment les nœuds (entités) et les arcs (relations) interagissent.

##### Exemple 2 : Modélisation des relations sociales

```plantuml
@startuml
entity "User" as U {
  *id : Integer
  *name : String
}

entity "Friendship" {
  *since : Date
}

U -- U : "is friend with"
@enduml
```

Ici, chaque utilisateur est modélisé comme un nœud, et la relation d’amitié entre deux utilisateurs est représentée par un arc `Friendship`, avec des informations supplémentaires telles que la date à laquelle l'amitié a commencé. Ce type de modèle est particulièrement adapté pour les réseaux sociaux.

#### 8.2. Typologie des graphes dans Neo4J

Dans Neo4J, les graphes peuvent être enrichis avec des propriétés et des types pour les nœuds et les arcs. Ce type de modèle est souvent appelé **Property Graph** (graphe de propriétés). Voici quelques éléments essentiels :

1. **Nœuds** : Chaque nœud représente une entité, comme une personne ou un lieu, et possède des propriétés.
2. **Arcs** : Les arcs (ou relations) relient les nœuds et peuvent eux aussi avoir des propriétés.
3. **Propriétés** : Les propriétés sont des paires clé/valeur associées aux nœuds et aux arcs, permettant d'ajouter des détails.

Voici un exemple de graphe enrichi avec des propriétés utilisant PlantUML :

```plantuml
@startuml
entity "Person" {
  *name : String
  *age : Integer
}

entity "Movie" {
  *title : String
  *release_year : Integer
}

entity "Directed" {
  *since : Date
}

Person -- Directed : "directed"
Directed -- Movie : "a"
@enduml
```

Dans cet exemple, la relation `Directed` entre une personne et un film contient une propriété `since` qui indique la date à laquelle la personne a commencé à diriger le film.

#### 8.3. Cas d’utilisation des systèmes graphes

Les systèmes orientés graphes sont utilisés dans de nombreux domaines où les données sont interconnectées et nécessitent une analyse relationnelle. Voici quelques exemples de cas d'utilisation :
- **Réseaux sociaux** : Modéliser les relations d’amitié, les likes, les partages, etc.
- **Biologie** : Représenter des réseaux d’interactions biologiques, comme des cascades de signalisation ou des réseaux métaboliques.
- **Géomatique** : Cartographier des territoires et des infrastructures.
- **Systèmes de recommandation** : Identifier des connexions complexes entre utilisateurs, produits, ou services pour offrir des recommandations personnalisées.

#### 8.4. Requêtage des graphes avec Cypher

Cypher est le langage de requête utilisé dans Neo4J pour interagir avec les graphes. Il permet de créer, modifier et interroger les nœuds et les relations. Voici un exemple de requête Cypher pour créer une relation entre deux entités :

```cypher
CREATE (p:Person {name: 'Alice'})-[:KNOWS {since: 2020}]->(p2:Person {name: 'Bob'})
```

Cette requête crée deux nœuds `Person` représentant Alice et Bob, et établit une relation `KNOWS` entre eux avec une propriété `since` indiquant depuis quand ils se connaissent.

#### 8.5. Visualisation des graphes avec PlantUML

PlantUML peut également être utilisé pour visualiser le résultat d'une requête Cypher, en modélisant le graphe obtenu. Par exemple, après avoir exécuté une requête pour récupérer toutes les relations d'amitié, vous pouvez générer un schéma comme celui-ci :

```plantuml
@startuml
entity "Alice" as A
entity "Bob" as B
entity "Charlie" as C

A -- B : "KNOWS"
B -- C : "KNOWS"
@enduml
```

Cela montre les relations entre trois utilisateurs dans un réseau social, chacun étant connecté par une relation `KNOWS`.

### 9. La puissance des graphes dans les systèmes NoSQL

Les graphes dans les systèmes NoSQL, comme Neo4J, permettent de représenter et de manipuler des ensembles de données complexes d'une manière qui est difficilement réalisable avec des bases de données relationnelles. Grâce à des outils comme PlantUML, il est possible de visualiser facilement ces graphes et de mieux comprendre leurs structures. Le langage Cypher offre également une grande flexibilité pour interagir avec les graphes, en permettant de créer des requêtes efficaces pour naviguer dans ces structures de données complexes.

### 9. Requêtage dans Neo4J avec Cypher

Le langage **Cypher** est essentiel pour interagir avec les bases de données Neo4J. Il permet de naviguer dans les graphes, de rechercher des entités spécifiques, de créer des relations entre elles et de manipuler les données de manière performante. Voyons quelques exemples concrets des capacités de Cypher et des opérations qu’il permet d’effectuer.

#### 9.1. Recherche de chemins dans les graphes

L'un des grands avantages des bases de données orientées graphes est la possibilité de **naviguer à travers les relations**. Par exemple, en utilisant Cypher, vous pouvez rechercher les chemins entre deux nœuds. Voici quelques exemples de requêtes permettant de trouver des chemins :

- **Cheminer le long des arcs :**
  
  ```cypher
  MATCH (c1:Commune)-[:NEARBY]->()<-[:NEARBY]-(c2:Commune)
  RETURN c1, c2
  ```

  Cette requête cherche des communes reliées par une relation de proximité (`NEARBY`).

- **Chemins multiples avec longueur variable :**

  ```cypher
  MATCH (m:Commune {nom: 'MONTPELLIER'}) -[:NEARBY*1..2]- (n:Commune)
  RETURN m, n
  ```

  Cette requête trouve les communes à une ou deux relations de distance de Montpellier.

#### 9.2. Utilisation des fonctions de chemins

Les graphes permettent de calculer des **chemins optimaux** entre des nœuds. Cypher offre des fonctions comme `shortestPath` pour trouver le plus court chemin entre deux nœuds.

- **Trouver un chemin entre deux communes :**

  ```cypher
  MATCH path=(m:Commune {nom: 'MONTPELLIER'})-[:NEARBY*]-(n:Commune {nom: 'NIMES'})
  RETURN path
  ```

  Cette requête retourne tous les chemins reliant Montpellier à Nîmes via des relations `NEARBY`.

- **Trouver le plus court chemin :**

  ```cypher
  MATCH p=shortestPath((m:Commune)-[:NEARBY*]-(n:Commune))
  WHERE m.nom = 'MONTPELLIER' AND n.nom = 'NIMES'
  RETURN length(p) AS taillePlusCourtChemin
  ```

  Cette requête calcule la longueur du plus court chemin entre Montpellier et Nîmes.

#### 9.3. Extraction des nœuds sur un chemin

Une fois le chemin trouvé, il est possible d'extraire des informations sur les nœuds qui le composent. Voici un exemple de requête permettant de lister les communes situées sur le plus court chemin entre Montpellier et Nîmes :

- **Lister les nœuds le long d'un chemin :**

  ```cypher
  MATCH p=shortestPath((m:Commune)-[:NEARBY*]-(n:Commune))
  WHERE m.nom = 'MONTPELLIER' AND n.nom = 'NIMES'
  RETURN extract(n IN nodes(p) | n.nom) AS communesSurLeChemin
  ```

  Cette requête utilise la fonction `extract` pour lister les noms des communes sur le chemin.

#### 9.4. Filtrage des chemins

Il est également possible de filtrer les résultats pour exclure certains nœuds ou relations. Par exemple, si l'on souhaite trouver le plus court chemin entre Montpellier et Nîmes sans passer par Lunel, voici la requête correspondante :

- **Exclure un nœud spécifique :**

  ```cypher
  MATCH p=shortestPath((m:Commune {nom: 'MONTPELLIER'})-[:NEARBY*]-(n:Commune {nom: 'NIMES'}))
  WHERE NOT ('LUNEL' IN [n IN nodes(p) | n.nom])
  RETURN p
  ```

  Ici, le chemin exclut explicitement le nœud représentant Lunel.

#### 9.5. Manipulation des données

Cypher permet également d'effectuer des opérations **CRUD** (Créer, Lire, Mettre à jour, Supprimer) sur les nœuds et les relations des graphes. Voici quelques exemples d'opérations CRUD en Cypher :

- **Création d’un nœud et d'une relation :**

  ```cypher
  CREATE (m:Commune {nom: 'MONTPELLIER', latitude: 43.610769, longitude: 3.876716, codeinsee: '34172'})
  RETURN m
  ```

  Cette commande crée un nœud `Commune` pour Montpellier avec plusieurs propriétés.

- **Suppression de nœuds et relations :**

  ```cypher
  MATCH (n)
  OPTIONAL MATCH (n)-[r]-()
  DELETE n, r
  ```

  Cette requête supprime un nœud et toutes les relations associées à ce nœud.

#### 9.6. Indexation et contraintes

Pour améliorer les performances, Neo4J permet de **créer des index** et des **contraintes** sur certains attributs des nœuds :

- **Créer un index sur un attribut :**

  ```cypher
  CREATE INDEX ON :City(name)
  ```

  Cet index accélérera les recherches basées sur le nom des villes.

- **Créer une contrainte d’unicité :**

  ```cypher
  CREATE CONSTRAINT ON (c:City) ASSERT c.name IS UNIQUE
  ```

  Cela garantit que chaque ville aura un nom unique dans la base de données.

### 10. Utilisation avancée de Neo4J

#### 10.1. Services Web REST

Neo4J offre une API REST qui permet d’interagir avec la base de données via des appels HTTP. Cela permet d'intégrer Neo4J dans des architectures web plus larges. Voici un exemple d'utilisation de l'API REST pour créer une ville dans la base de données :

```bash
curl -H "Accept: application/json; charset=UTF-8" -H "Content-Type: application/json" \
     -X POST http://neo4j:neo4j17@localhost:7474/db/data/cypher \
     -d '{"query" : "CREATE (n:City {code:34, name : {name} }) RETURN n", "params" : { "name" : "Montpellier" }}'
```

L'API REST permet de **créer, modifier et supprimer** des nœuds et des relations dans la base de données depuis un client web.

#### 10.2. Neo4J embarqué

Neo4J peut être utilisé en mode embarqué dans une application Java, ce qui permet d'intégrer directement la base de données dans des applications autonomes. Voici un exemple d'utilisation de l'API Java pour créer un nœud :

```java
Node car = graphDb.createNode(Labels.CAR);
car.setProperty("brand", "citroen");
car.setProperty("model", "2cv");

Node owner = graphDb.createNode(Labels.EMPLOYEE);
owner.setProperty("lastName", "M");
owner.setProperty("job", "teacher");

owner.createRelationshipTo(car, Relations.OWNS);
```

Dans cet exemple, un nœud représentant une voiture et un autre représentant un propriétaire sont créés, puis reliés par une relation `OWNS`.

#### La flexibilité et la puissance de Neo4J

Neo4J offre une grande souplesse pour gérer des bases de données graphes, que ce soit via Cypher pour la manipulation des graphes ou l'utilisation de l'API REST et Java pour des intégrations plus complexes. Sa capacité à gérer des données interconnectées et à répondre à des requêtes complexes en fait un outil puissant pour de nombreux cas d’utilisation.

### 11. Extension de Neo4J : APOC et Neosemantics

**APOC** (Awesome Procedures On Cypher) et **Neosemantics** sont des extensions puissantes pour Neo4J qui enrichissent ses fonctionnalités de manière significative. APOC apporte un ensemble de procédures utiles pour manipuler des graphes, tandis que Neosemantics permet de travailler avec des données RDF et d’intégrer des fonctionnalités de Web sémantique.

#### 11.1. APOC : Une bibliothèque de procédures

APOC est une collection de procédures préconstruites qui peuvent être utilisées avec Cypher pour accomplir des tâches avancées comme la manipulation des graphes, l’exportation de données, ou la conversion de formats. Ces procédures sont particulièrement utiles pour étendre les fonctionnalités de Neo4J dans des cas spécifiques.

##### 11.1.1. Installation et appel des procédures APOC

Les procédures APOC sont déployées sous forme de fichiers `.jar` à placer dans le répertoire des **plugins** de Neo4J. Une fois en place, ces procédures peuvent être invoquées directement depuis Cypher.

- **Appel de procédures APOC :**
  
  ```cypher
  CALL dbms.procedures() YIELD name
  RETURN head(split(name,".")) AS package, count(*), collect(name) AS procedures;
  ```

  Cette requête retourne une liste des procédures APOC disponibles.

##### 11.1.2. Exemples d’utilisation

Les procédures APOC couvrent un large éventail de fonctionnalités, notamment la **traversée de graphes**, la **recherche plein texte**, les **fonctions spatiales**, et bien plus encore. Voici quelques exemples spécifiques :

- **Requêtes sur le schéma** :

  ```cypher
  CALL db.schema()
  ```

  Cette commande affiche le schéma du graphe.

- **Export des données au format JSON** :

  ```cypher
  CALL apoc.export.json.query(
      "MATCH (a) RETURN id(a), labels(a), a.name", 
      "output.json", {}
  )
  ```

  Cette procédure exporte les données correspondant à la requête Cypher au format JSON dans un fichier texte.

- **Exemple d’exportation avec sortie** :

  ```json
  {"id(a)":1,"labels(a)":["Personne"],"a.name":"bob"}
  {"id(a)":20,"labels(a)":["Institution"],"a.name":"UM"}
  {"id(a)":82,"labels(a)":["City"],"a.name":"Montpellier"}
  ```

  Ce résultat montre un exemple d’export des nœuds avec leurs identifiants, étiquettes et propriétés au format JSON.

##### 11.1.3. Autres fonctionnalités avancées

APOC propose également des fonctionnalités spécifiques telles que la **conversion de formats**, la **migration entre SGBD**, et des opérations complexes comme le **partionnement dynamique des graphes**. Ces outils sont essentiels pour des scénarios nécessitant une manipulation fine des graphes ou une intégration avec d'autres systèmes.

#### 11.2. Neosemantics : Intégration des données RDF

**Neosemantics** est une extension de Neo4J qui permet de travailler avec des données **RDF** (Resource Description Framework), un format de représentation des données souvent utilisé dans le Web sémantique et le Linked Open Data (LOD). Elle permet notamment :
- L'importation et l'exportation de données RDF.
- La manipulation de graphes dans des formats RDF.
- La gestion des ontologies et des inférences sur les données RDF.

##### 11.2.1. LPG vs RDF : Comparaison des modèles

Neo4J repose sur le modèle **Labeled Property Graph (LPG)**, tandis que RDF utilise une approche basée sur les triplets (sujet-prédicat-objet). Bien que les deux modèles aient des similarités, il existe des différences importantes :

- **Nœud dans LPG** : Un nœud dans LPG a un identifiant interne, un ou plusieurs labels, et un ensemble de propriétés définies sous forme de paires clé-valeur.
- **Relation dans LPG** : Une relation dans LPG possède également un identifiant interne, un seul type, et des propriétés sous forme de paires clé-valeur. Les relations peuvent transporter des informations spécifiques, comme la distance entre deux nœuds.
  
En revanche, dans le modèle RDF :
- **Nœud RDF** : Représenté par une URI, associé à une ou plusieurs classes, et possède des propriétés littérales pointées de manière externe.
- **Relation RDF** : Les relations sont également des ressources à part entière et peuvent jouer le rôle de nœuds dans certains triplets. Cependant, une relation ne peut avoir que des propriétés définies de manière générique.

L’une des forces du modèle LPG est sa concision et sa capacité à **stocker des informations spécifiques aux relations**, contrairement à RDF, qui nécessite souvent la réification (création de nouveaux nœuds pour représenter des relations).

##### 11.2.2. Utilisation de Neosemantics avec RDF

Neosemantics permet à Neo4J de **stocker des données RDF** et d’**exporter des graphes au format RDF**. Voici quelques exemples de commandes pour manipuler des données RDF dans Neo4J :

- **Requête Cypher avec export RDF** :

  ```cypher
  :POST /rdf/cypher { 
    "cypher": "MATCH (v:City) RETURN v", 
    "format" : "N3"
  }
  ```

  Cette commande exécute une requête Cypher et renvoie les résultats au format **N3** (un format de sérialisation RDF).

- **Importation de données RDF** :

  ```cypher
  CREATE INDEX ON :Resource(uri)
  CALL semantics.importRDF("http://example.com/data.ttl","Turtle")
  ```

  Ici, les données RDF situées à l'URL spécifiée sont importées au format **Turtle** dans Neo4J.

##### 11.2.3. Gestion des relations et des propriétés dans RDF

Un point important à considérer lors de l'utilisation de Neosemantics est la gestion des propriétés associées aux relations. En RDF, les relations sont des entités distinctes, mais dans LPG, elles peuvent avoir des propriétés internes.

Exemple de relations avec propriétés dans LPG :
  
```cypher
CREATE (gf:Personne {nom: "Freche", prenom: "Georges"})
  <-[ap1:administree_par {date_debut: 1997, date_fin: 2004}]-
  (c:Ville {nom: "Montpellier"})
```

Cet exemple montre comment des dates peuvent être directement associées à la relation `administree_par` entre un maire et une ville.

##### 11.2.4. Problèmes de mapping et limitations

Le mapping des données entre LPG et RDF peut poser des difficultés, en particulier lorsque les propriétés des relations sont perdues lors de la conversion en RDF. Cela nécessite souvent un **remaniement manuel du mapping** pour s’assurer que toutes les informations sont correctement transférées.

### 12. Conclusion : Puissance et flexibilité des extensions de Neo4J

Les extensions **APOC** et **Neosemantics** élargissent considérablement les capacités de Neo4J. APOC, avec son ensemble de procédures avancées, permet de traiter des cas d'utilisation complexes, tandis que Neosemantics offre une intégration fluide avec les données RDF et le Web sémantique. Ces outils renforcent Neo4J en tant que système de gestion de graphes polyvalent, adapté à une variété de contextes, du traitement de données massives aux applications spécifiques nécessitant des fonctionnalités avancées comme l’inférence et le traitement de données hétérogènes.