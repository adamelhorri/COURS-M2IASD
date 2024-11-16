---
tags:
  - nosql
---
## Introduction à Neo4J
### Pourquoi NoSQL?

- **NoSQL** : Adapté aux données flexibles, variées et incomplètes, aux relations multiples et hiérarchiques, et aux transactions à haut volume en environnement distribué.
### Scalabilité
- **Scalabilité horizontale** (Scaling Out) :
    
    - Ajout de serveurs pour répartir la charge.
    - Capacité d’augmentation linéaire, chaque nœud gérant une part de charge.
    - Adapté aux bases NoSQL décentralisées pour les grandes applications Web.
- **Scalabilité verticale** (Scaling Up) :
    
    - Amélioration d’un serveur par ajout de ressources matérielles (CPU, RAM, stockage).
    - Limité par la capacité physique, coûteux à un certain point.
- **Limites des SGBDR pour la scalabilité** :
    - **Fragmentation des schémas** : Distribution des tables par lignes (fragmentation horizontale), par colonnes (fragmentation verticale) ou hybride.
    - **Réplication et performance** : Garantir l’intégrité des transactions et la cohérence est complexe, affectant disponibilité et rapidité des traitements dans un environnement distribué.
### Theorème CAP
- **Théorème CAP** : Un système distribué ne peut simultanément garantir **Cohérence** (synchronisation immédiate des données), **Disponibilité** (réponse garantie aux requêtes), et **Tolérance aux partitions** (fonctionnalité malgré des nœuds inaccessibles).
- **Compromis CAP** :
    
    - **SGBDR** : Privilégient **Cohérence** et **Disponibilité**, avec une faible **Tolérance aux partitions** ; adaptés aux environnements centralisés.
    - **NoSQL** : Privilégient la **Tolérance aux partitions** pour les systèmes distribués :
        - **Cohérence** (CP) : Synchronisation assurée avec délai.
        - **Disponibilité** (AP) : Réponse constante, même si les données ne sont pas à jour.
- **Conclusion CAP** : Les SGBDR favorisent cohérence et disponibilité, tandis que NoSQL, orienté distribué, favorise tolérance aux partitions avec un choix entre cohérence et disponibilité selon les besoins.
![[Pasted image 20241114145158.png]]
### Caracteristiques NoSQL
- **Caractéristiques des Systèmes NoSQL** :
    
    - **APIs spécifiques** adaptées aux fonctionnalités propres à chaque système.
    - **Terminologies propriétaires** variables selon les systèmes.
    - **Requêtage à géométrie variable** selon la structure (documents, graphes, colonnes, clés-valeurs).
- **Exemples de Systèmes NoSQL** :
    
    - **BigTable** (Google) : Stockage par colonnes.
    - **Memcached** : Stockage clé-valeur pour la mise en cache.
    - **Dynamo** (Amazon) : Stockage clé-valeur distribué, tolérant aux pannes.
- **Modes de Représentation des Données** :
    - **Document** : CouchDB.
    - **Graph** : Neo4j.
    - **Column** : Hbase, Cassandra, Hypertable.
    - **Key-Value** : Riak, Project Voldemort.
- **Neo4j et les Graphes NoSQL** : Conçu pour les relations complexes, Neo4j excelle dans la représentation intuitive des associations et hiérarchies, idéal pour les réseaux sociaux et la biologie, correspondant au mode d’organisation mental humain.
## Graphes
- **Graphe** G=⟨V,E⟩G = \langle V, E \rangleG=⟨V,E⟩ : Ensemble des nœuds (V) et des liens (E) entre eux.
    
    - **Graphe orienté** : Les liens sont des arcs dirigés d'un nœud vers un autre.
    - **Sous-graphe** G′=⟨V′,E′⟩G' = \langle V', E' \rangleG′=⟨V′,E′⟩ : Sous-ensemble de nœuds et de liens de GGG.
    - **Chemin** CCC entre deux nœuds v1v_1v1​ et v2v_2v2​ : Suite de nœuds et de liens reliant v1v_1v1​ à v2v_2v2​.
        - **Cycle** : Chemin fermé revenant au point de départ C(vi,vi)C(v_i, v_i)C(vi​,vi​).
    - **Composantes connexes** : Sous-ensembles de nœuds connectés entre eux, représentant des parties isolées dans le graphe.
    - **Graphe connecté** : Tout nœud est accessible depuis n'importe quel autre nœud.
    - **Arbre** : Graphe connecté et acyclique.
- **Algorithmes de parcours** :
    
    - **Parcours en largeur (BFS)** : Exploration niveau par niveau, utile pour trouver des chemins courts dans des graphes non pondérés.
    - **Parcours en profondeur (DFS)** : Exploration en profondeur maximale avant retour, pratique pour la détection de cycles.
- **Algorithmes de chemins** :
    
    - **Recherche du plus court chemin (ex. : algorithme de Dijkstra)** : Trouve le chemin minimal entre deux nœuds dans un graphe pondéré, optimisé pour les poids positifs.
- **Analyse de structure** :
    
    - **Mesures de centralité (ex. : centralité d'Eigenvector)** : Identifie les nœuds influents basés sur leurs connexions à d'autres nœuds influents.
    - **Partitionnement** : Division en clusters maximisant les connexions internes et minimisant les connexions externes, utile pour l’analyse communautaire.
    - **Coloration** : Attribution de couleurs aux nœuds pour éviter des conflits entre nœuds adjacents, utile en planification et ordonnancement.
## Neo4J
### Specs
- **Accès et manipulation des données** :
  - **Traversal Java API** : API Java pour parcourir et manipuler les graphes dans Neo4j, optimisant la navigation des données.

- **Langages de requête** :
  - **Cypher (et OpenCypher)** : Langage de requête principal de Neo4j, intuitif et expressif.
  - **Gremlin** : Langage de requête pour des systèmes multi-graphes.
  - **GQL (Graph Query Language)** : Standard en développement pour les requêtes de graphes.

- **Indexation** :
  - Utilisation d’index pour améliorer la rapidité d'accès aux nœuds et relations, optimisant les performances de recherche.

- **Mécanismes transactionnels (ACID)** :
  - Garantie des propriétés ACID pour assurer l'intégrité et la fiabilité des données lors des transactions.

- **Architecture en cluster** :
  - La version payante offre une architecture de clustering pour améliorer la distribution et la disponibilité, bien que cela reste complexe dans les bases de graphes.

- **Conçu pour le Web** :
  - **Compatibilité Java EE** : Intégration avec Spring et Spring Data.
  - **Web de données** : Support de SAIL et SPARQL.
  - **API REST** : Interface REST pour des interactions faciles avec les applications web.
### Cypher
#### Clauses
- **CREATE** : Crée des nœuds et relations.
  - Ex : `CREATE (n:Person {name: "Alice"})` crée un nœud `Person` avec `name` = "Alice".

- **DELETE / REMOVE** :
  - **DELETE** : Supprime des nœuds ou relations.
    - Ex : `DELETE n` pour supprimer le nœud `n`.
  - **REMOVE** : Supprime des étiquettes ou propriétés.
    - Ex : `REMOVE n:Label` pour retirer l’étiquette `Label` du nœud `n`.

- **SET** : Met à jour les propriétés d’un nœud ou d’une relation.
  - Ex : `SET n.age = 30` pour définir `age` = 30 pour le nœud `n`.

- **MATCH** : Recherche des motifs dans le graphe, souvent pour débuter une requête.
  - Ex : `MATCH (n:Person {name: "Alice"})` pour trouver un nœud `Person` nommé "Alice".

- **MERGE** : Combine `MATCH` et `CREATE` pour trouver ou créer des motifs.
  - Ex : `MERGE (n:Person {name: "Bob"})` crée un nœud `Person` nommé "Bob" s’il n'existe pas.

- **WHERE** : Ajoute des conditions aux requêtes.
  - Ex : `MATCH (n:Person) WHERE n.age > 25 RETURN n` pour les `Person` ayant `age` > 25.

- **RETURN** : Spécifie les éléments à renvoyer dans les résultats.
  - Ex : `MATCH (n:Person) RETURN n.name` pour retourner le `name` de chaque `Person` trouvé.

- **UNION** : Combine les résultats de plusieurs requêtes.
  - Ex : `MATCH (n:Person) RETURN n.name UNION MATCH (m:Place) RETURN m.name` pour les noms de `Person` et `Place`.

- **WITH** : Enchaîne les opérations sur des résultats intermédiaires.
  - Ex : `MATCH (n:Person) WITH n.name AS name WHERE name STARTS WITH 'A' RETURN name` pour les noms de `Person` commençant par "A".

