---
tags:
  - nosql
---
### Quand Utiliser NOSQL

**Cas spécifiques** : Alternatives aux bases relationnelles lorsque :

- Les schémas évoluent fréquemment.
- Les entités ont des caractéristiques variées et souvent incomplètes.
- Les associations multiples (1..*) et les attributs composites sont fréquents.
- Les flux transactionnels (lecture/écriture) sont très élevés.
- Les données sont distribuées dès l'origine (ex. globalisation).

### Comparaison Notions SQL/NOSQL
- **NoSQL** : Idéal pour les environnements distribués, massivement scalables et nécessitant une gestion flexible des données. S'adapte à des cas d’usage variés grâce à ses modèles semi-structurés et à sa persistance polyglotte.  
- **Relationnel** : Plus adapté aux applications nécessitant une cohérence stricte, des transactions complexes et une structure rigide. Idéal pour les cas où les données ne nécessitent pas de flexibilité ou de distribution massive.

| **Aspect**                 | **NoSQL**                                                                                                                                                                                                                                           | **Relationnel**                                                                                                                                                                       |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Problématique initiale** | Répond aux limites des SGBD relationnels grâce à sa flexibilité et son adaptabilité, particulièrement face à l’augmentation du volume des données et leur interconnexion.                                                                             | Limité pour des environnements massivement distribués ou des données hétérogènes.                                                                                                     |
| **Scalabilité**            | - **Horizontale** : Ajout de nouveaux nœuds pour répartir la charge. <br> - **Verticale** : Amélioration matérielle (CPU, RAM).                                                                                                                    | - Principalement verticale, avec des contraintes matérielles. <br> - Scalabilité horizontale complexe et coûteuse à mettre en œuvre (partitionnement manuel).                         |
| **Réplication**            | Optimisée pour assurer une disponibilité constante et une tolérance aux pannes. En cas de panne d’un nœud, un autre prend automatiquement le relais.                                                                                                | Garantit une cohérence stricte, mais configuration complexe et lente dans des environnements distribués.                                                                              |
| **Partitionnement**        | Automatique et performant (ex. sharding intégré dans MongoDB, Cassandra). Permet une répartition des données sans altérer le schéma initial, assurant des performances élevées à grande échelle.                                                     | Souvent manuel et moins efficace pour de grandes échelles. Nécessite des ajustements spécifiques pour s’adapter à des volumes importants de données.                                  |
| **Flexibilité**            | Modèles semi-structurés (ex. JSON), adaptés aux données hétérogènes et évolutives. Permet une évolution facile des schémas sans nécessiter de migrations complexes.                                                                                  | Structure fixe nécessitant une normalisation stricte. Peu adapté pour les données non standardisées, et toute modification de schéma nécessite des migrations potentiellement coûteuses. |
| **Persistance polyglotte** | Conçu pour des environnements nécessitant plusieurs types de bases (documents, colonnes, graphes). Permet de choisir le bon système selon les cas d’usage (ex. Neo4J pour les graphes, MongoDB pour les documents).                                    | Modèle unique (tables relationnelles), rigide pour des applications variées. Peu d’intérêt à coupler plusieurs bases relationnelles entre elles dans un même projet.                   |
| **Postulat CAP**           | Offre des choix entre : <br> - **AP** (Disponibilité et Tolérance aux partitions, ex. CouchDB). <br> - **CA** (Cohérence et Disponibilité, ex. Neo4J).                                                                                               | Typiquement **CA** (Cohérence et Disponibilité) avec une faible tolérance aux partitions (P), ce qui le rend moins adapté aux environnements massivement distribués.                   |
| **Système distribué**      | Optimisé pour des environnements distribués. Permet une gestion efficace des données entre plusieurs nœuds, avec une gestion automatique des échecs et de la latence.                                                                               | Moins performant dans des environnements distribués. Les transactions complexes et la cohérence stricte augmentent la latence et nécessitent des ressources importantes.              |
### Systèmes existants

| **Nom**               | **Mode de représentation** | **CAP**   |
|------------------------|----------------------------|-----------|
| CouchDB               | Document                  | AP        |
| Neo4j                 | Graph                     | CA        |
| HBase                 | Column                    | CP        |
| Riak                  | Key-Value                 | CP        |
| Project Voldemort     | Key-Value                 | AP        |
| Cassandra             | Column                    | AP        |
| Hypertable            | Column                    | Unknown   |
## Definitions
### Scalabilité
- **Scalabilité horizontale (scaling out)** : Ajout de serveurs (nœuds) avec répartition de charge (→ NoSQL).
- **Scalabilité verticale (scaling up)** : Amélioration matérielle d’un serveur (CPU, RAM, stockage, réseau).


