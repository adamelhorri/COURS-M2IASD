---
tags:
  - nosql
---

#### 1. **Architecture multi-maîtres**
- **Définition claire** : Modèle où chaque nœud d'un système distribué peut lire et écrire, évitant les points uniques de défaillance. La gestion des conflits repose sur le contrôle de version (MVCC), avec un quorum défini pour les lectures (**R**) et écritures (**W**) pour maintenir la cohérence.
- **Définition simplifiée** : Tous les nœuds peuvent lire et écrire, garantissant disponibilité et résilience via des règles de quorum.

#### 2. **Hachage cohérent**
- **Définition claire** : Technique pour répartir les données sur plusieurs nœuds à l’aide d’une fonction de hachage. Lorsqu’un nœud est ajouté ou retiré, seules les données voisines (au sein de la plage définie par **Q**) sont redistribuées, minimisant les perturbations et optimisant la scalabilité.
- **Définition simplifiée** : Répartir les données efficacement entre serveurs, en limitant les impacts des modifications au voisinage direct.

#### 3. **Scalabilité (verticale et horizontale)**
- **Définition claire** : La scalabilité verticale consiste à renforcer un nœud existant (CPU, RAM), tandis que la scalabilité horizontale ajoute de nouveaux nœuds. Dans un système distribué, des quorums de lecture (**R**) et d'écriture (**W**) gèrent la charge et assurent la cohérence.
- **Définition simplifiée** : On améliore un serveur ou on en ajoute d’autres, tout en répartissant intelligemment les requêtes via les quorums.

#### 4. **Réplication**
- **Définition claire** : Duplication des données sur plusieurs nœuds pour garantir disponibilité et tolérance aux pannes. Les quorums de lecture (**R**) et d'écriture (**W**) contrôlent le niveau de cohérence souhaité dans un environnement distribué.
- **Définition simplifiée** : Copier les données sur plusieurs serveurs et utiliser des quorums pour maintenir cohérence et disponibilité.
	- Types:
	- **CouchDB**
	- **Multi-Master** : Tous les nœuds acceptent des écritures et se synchronisent, idéal pour la décentralisation et le hors-ligne.
	
	- **Neo4j**
	- **Master-Slave** : Le master gère les écritures, les slaves optimisent les lectures, adapté pour cohérence et performance.
#### 5. **Partitionnement**
- **Définition claire** : Technique de découpe des données en partitions logiques, chaque nœud gérant une partie des données. La répartition des partitions utilise une fonction de hachage cohérent avec des règles de quorum pour garantir disponibilité et performance.
- **Définition simplifiée** : Découper les données pour les répartir sur plusieurs serveurs en optimisant leur accès via un quorum.

#### 6. **Postulat CAP et système distribué**
- **Définition claire** : Théorème qui impose de choisir entre cohérence, disponibilité ou tolérance aux partitions dans un système distribué. Les systèmes NoSQL ajustent les paramètres **Q**, **N**, **R**, et **W** pour trouver un équilibre entre ces contraintes.
- **Définition simplifiée** : En distribué, il faut choisir entre cohérence stricte, disponibilité continue ou tolérance aux pannes.
- ![[Pasted image 20241120091401.png]]

#### 7. **Persistance polyglotte**
- **Définition claire** : Approche consistant à utiliser plusieurs types de bases de données dans une application pour répondre à des besoins spécifiques. Les systèmes reposant sur des quorums adaptent leurs interactions entre ces bases selon les besoins en cohérence et disponibilité.
- **Définition simplifiée** : Choisir le bon type de base pour chaque cas d’usage, avec une gestion ajustée via des quorums.

#### 8. **Modèle semi-structuré**
- **Définition claire** : Modèle flexible permettant de stocker des données variées sans structure fixe. Les bases exploitant ce modèle, comme les bases orientées documents, bénéficient des mécanismes de quorum pour gérer les interactions distribuées.
- **Définition simplifiée** : Une structure de données adaptable qui facilite les changements, optimisée pour le distribué via des quorums.

#### 9.Système **QNRW** – Définition et explication

Le système **QNRW** est une méthode de gestion des systèmes distribués basée sur quatre paramètres fondamentaux :

- **Q** : Quorum total requis pour valider une opération.
- **N** : Nombre total de réplicas des données sur les nœuds.
- **R** : Nombre minimum de réplicas requis pour valider une lecture.
- **W** : Nombre minimum de réplicas requis pour valider une écriture.

Ce modèle garantit un équilibre entre la cohérence, la disponibilité et la tolérance aux partitions en ajustant dynamiquement ces paramètres en fonction des besoins du système.

- **Définition simplifiée** :C’est un système qui définit comment les données sont lues et écrites dans un environnement distribué, en imposant des règles de quorum pour assurer un équilibre entre cohérence et disponibilité.


### Petit cocktail

### Réponses détaillées :

#### 1. **Importance de la scalabilité dans les bases NoSQL avec illustration du système QNRW :**

La scalabilité est essentielle pour garantir qu’un système peut gérer une augmentation du volume de données, du trafic ou du nombre d’utilisateurs sans perdre en performance. Dans le monde actuel, où les applications génèrent des données massives en continu, les bases de données doivent pouvoir s’adapter rapidement et efficacement. C’est là que les bases NoSQL montrent leur avantage, notamment grâce à leur capacité à évoluer facilement.

Deux types de scalabilité existent :
- **La scalabilité verticale** : on améliore les performances d’un serveur existant en ajoutant des ressources comme plus de RAM, un processeur plus puissant ou un disque dur plus rapide. Cependant, cette solution a des limites physiques et devient coûteuse au-delà d’un certain point.
- **La scalabilité horizontale** : on ajoute de nouveaux serveurs au système pour répartir la charge. Ce modèle est plus flexible et moins coûteux à long terme. Les bases NoSQL, comme CouchDB, sont conçues pour favoriser ce type de scalabilité.

Pour rendre cette scalabilité horizontale efficace, des mécanismes comme le partitionnement et la réplication sont utilisés :
- **Le partitionnement** divise les données en blocs (ou partitions), qui sont répartis entre les serveurs. Cela permet de traiter plus de requêtes en parallèle.
- **La réplication** copie les données sur plusieurs nœuds pour éviter les pertes en cas de panne et améliorer la disponibilité.

Le système **QNRW**, utilisé dans CouchDB, met en pratique ces principes tout en gérant les compromis entre cohérence, disponibilité et tolérance aux pannes. Voici comment il fonctionne :
- **N (nombre de réplicas)** : Définit combien de copies des données sont stockées sur le système. Par exemple, si \( N = 3 \), chaque donnée est copiée sur trois nœuds.
- **Q (quorum)** : Indique combien de nœuds doivent valider une opération (lecture ou écriture). La règle est que \( Q = R + W > N \), ce qui garantit que les lectures et écritures se recoupent pour éviter les incohérences.
- **R (quorum de lecture)** : Nombre minimum de réplicas qui doivent être consultés pour confirmer une lecture.
- **W (quorum d’écriture)** : Nombre minimum de réplicas devant valider une écriture.

Ces mécanismes garantissent une disponibilité élevée et une tolérance aux pannes, car même si un nœud devient inaccessible, les deux autres suffisent pour continuer à répondre aux requêtes. En parallèle, ils limitent les risques d’incohérence, tout en permettant au système de rester rapide et flexible.

La scalabilité, associée au système QNRW, est donc un pilier fondamental des bases NoSQL comme CouchDB, car elle permet de répondre aux exigences croissantes des applications modernes tout en maintenant la stabilité du système.

---

#### 2. **En quoi consiste le postulat CAP ?**

Le postulat CAP, ou théorème de Brewer, décrit les limites des systèmes distribués. Il affirme qu’un système distribué ne peut garantir simultanément :
1. **Cohérence (Consistency)** : Toutes les copies des données sont parfaitement synchronisées. Cela signifie que si vous effectuez une lecture après une écriture, vous récupérez toujours la dernière version des données.
2. **Disponibilité (Availability)** : Chaque requête reçoit une réponse, même si une partie du système est en panne.
3. **Tolérance aux partitions (Partition Tolerance)** : Le système reste fonctionnel même si certains nœuds ne peuvent pas communiquer entre eux, à cause d’une panne réseau, par exemple.

Dans la pratique, un système doit choisir deux de ces trois propriétés :
- Les bases relationnelles privilégient souvent **Cohérence** et **Disponibilité**, ce qui limite leur utilisation dans des environnements distribués où des pannes réseau peuvent survenir.
- Les bases NoSQL, comme CouchDB, privilégient **Disponibilité** et **Tolérance aux partitions**, en acceptant une cohérence éventuelle, c’est-à-dire que les données peuvent être temporairement désynchronisées.

Le postulat CAP montre donc qu’il n’existe pas de solution parfaite dans les systèmes distribués. Il s’agit de choisir les priorités en fonction des besoins de l’application. Par exemple, pour un système de messagerie, la disponibilité est cruciale, tandis que pour un système bancaire, la cohérence est prioritaire.


