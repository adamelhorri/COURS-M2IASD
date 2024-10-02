---
tags:
  - AdminBDD
  - annales
---

### Structure de l'examen

L'examen se concentrera sur l'ensemble des compétences couvertes dans les cours et TP, incluant la modélisation, l'optimisation des requêtes, la gestion des transactions et l'architecture Oracle. Voici la structure détaillée avec des **tâches à accomplir (todos)** pour chaque section :

---

#### 1. **Modélisation et Schéma Relationnel (15-20%)**

**Type de question** : Vous devrez **créer un schéma relationnel** en fonction d’un domaine donné (par exemple : gestion des employés, gestion des musées, etc.). Cela inclut la définition des entités, des relations, des contraintes (clés primaires, clés étrangères), et l'optimisation de la structure.

- **Ce que vous devez maîtriser** :
  - Identifier les **entités** et leurs attributs.
  - Définir les **relations** et **contraintes** entre ces entités.
  - Choisir les **clés primaires** et **étrangères** appropriées.
  - Utiliser correctement les commandes SQL pour créer des tables avec des **contraintes intégrées**.

**TODOs** :
1. **Révisez la modélisation UML ou ERD** : Faites des exercices pour transformer des descriptions textuelles en schémas relationnels.
2. **Pratiquez la création de tables** en SQL avec les bonnes contraintes.
3. **Utilisez les TP sur les contraintes (clés étrangères, cascade)** pour renforcer vos compétences.

---

#### 2. **Optimisation des Requêtes et Utilisation des Index (40-45%)**

**Type de question** : Vous serez amené à analyser des **plans d'exécution** produits par Oracle pour des requêtes SQL et à proposer des améliorations. Vous devrez aussi créer des **index** pour optimiser les performances.

- **Ce que vous devez maîtriser** :
  - **Lire et interpréter** les plans d'exécution SQL (par exemple, `EXPLAIN PLAN`).
  - Analyser les opérateurs physiques utilisés, comme les **jointures**, les **tri-fusion** ou les **boucles imbriquées**.
  - Identifier les **bottlenecks** (goulots d'étranglement) dans les requêtes et proposer des **solutions d'optimisation**.
  - Savoir **créer et gérer des index** pour améliorer la performance des requêtes.

**TODOs** :
1. **Lisez attentivement les plans d'exécution** : Utilisez des requêtes complexes dans vos TP et vérifiez les plans d'exécution avec `EXPLAIN PLAN`.
2. **Identifiez les points faibles des requêtes** : Apprenez à repérer les jointures mal optimisées ou les scans de table complets.
3. **Pratiquez la création d’index** et vérifiez leur effet sur les performances.
4. **Révisez les TP sur l'optimisation des requêtes** en utilisant des tables avec de gros volumes de données pour observer les différences avec et sans index.

---

#### 3. **Gestion des Transactions et Consistance (10-15%)**

**Type de question** : Vous devrez comprendre et analyser les **transactions**, les **niveaux d’isolation** et les mécanismes de **commit/rollback** dans des environnements multi-utilisateurs.

- **Ce que vous devez maîtriser** :
  - **Gérer les transactions** en SQL, notamment les **commit** et **rollback**.
  - Comprendre les **niveaux d’isolation** (comme `READ COMMITTED`, `SERIALIZABLE`) et leur impact sur la **concurrence** et la **cohérence des données**.
  - Résoudre des scénarios où des conflits de transaction surviennent (par exemple, un `dirty read` ou un `phantom read`).

**TODOs** :
1. **Pratiquez la gestion des transactions** en créant des scénarios de commit et de rollback dans vos TP.
2. **Étudiez les différents niveaux d’isolation** et leur application pratique dans des situations de concurrence.
3. **Testez des cas de conflit** : Simulez des transactions concurrentes sur les mêmes données et observez le comportement du système selon les niveaux d'isolation.

---

#### 4. **Architecture Oracle (10-15%)**

**Type de question** : Vous serez amené à expliquer et compléter des **schémas d'architecture Oracle**, à décrire les **structures de mémoire**, et à analyser l'impact des **structures de stockage**.

- **Ce que vous devez maîtriser** :
  - Connaître les principales **structures de mémoire** dans Oracle, comme la **System Global Area (SGA)** et la **Program Global Area (PGA)**.
  - Expliquer le rôle des différentes composantes de la SGA (comme le **Database Buffer Cache**, le **Library Cache**, et le **Redo Log Buffer**).
  - Savoir comment Oracle utilise ces structures pour **gérer l'accès aux données** et **optimiser les performances**.

**TODOs** :
1. **Révisez la théorie sur l'architecture Oracle** : Concentrez-vous sur les structures mémoires et le rôle des différents processus (`SMON`, `PMON`, etc.).
2. **Créez un schéma d’architecture** et assurez-vous de pouvoir expliquer comment chaque composant interagit avec les autres.
3. **Révisez les TP sur la gestion des blocs, des segments, et des tablespaces** pour comprendre l'organisation physique des données.

---

#### 5. **Requêtes SQL Complexes et Algébriques (10-15%)**

**Type de question** : Vous devrez **écrire des requêtes SQL complexes** ou les **réécrire** pour améliorer les performances et faciliter l'interprétation. Cela inclut des requêtes avec jointures, sous-requêtes, agrégations, et filtres.

- **Ce que vous devez maîtriser** :
  - **Écrire des requêtes SQL complexes** incluant des **jointures multiples**, des **sous-requêtes**, et des **fonctions d’agrégation**.
  - Réécrire des requêtes pour **les optimiser** ou les rendre plus lisibles, en utilisant des jointures internes ou externes, des semi-jointures, etc.
  - Comprendre l’impact de la réécriture sur la **performance** et le **plan d'exécution**.

**TODOs** :
1. **Pratiquez la réécriture de requêtes** SQL dans différents formats (avec ou sans sous-requêtes, jointures multiples, etc.).
2. **Testez des scénarios avec de grandes tables** et observez l'effet de vos optimisations sur le plan d'exécution.
3. **Révisez les TP de requêtes SQL complexes** et pratiquez des exercices supplémentaires pour être à l'aise avec la réécriture de requêtes.

---

### Conclusion

L’examen couvrira toutes les compétences essentielles acquises au cours du module, en particulier la **modélisation**, l'**optimisation des requêtes**, la **gestion des transactions**, et la **structure de l'architecture Oracle**. Pour vous préparer efficacement :

1. **Révisez les TP et cours** sur chaque sujet et **testez-vous régulièrement**.
2. **Maîtrisez les outils Oracle** comme `EXPLAIN PLAN`, `DBMS_STATS`, et les vues système.
3. **Appliquez les concepts de transaction et d’optimisation** dans des environnements réalistes, avec des données de grande taille.

En vous concentrant sur ces **tâches à accomplir** et en adoptant une progression méthodique, vous serez bien préparé pour réussir l’examen.