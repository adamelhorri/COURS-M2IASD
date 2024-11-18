---
tags:
  - nosql
---
## **CouchDB : Concepts avancés et Guide détaillé**

---

### **1. Introduction à CouchDB et JSON**

#### **JSON**
- **Format semi-structuré** : 
  - Données flexibles pouvant inclure des attributs manquants ou supplémentaires.
  - **Auto-descriptivité** : Les valeurs et le schéma coexistent dans un même fichier.
  - Structure hiérarchique adaptée à des relations complexes.

#### **Langages semi-structurés**
- **JSON** : Orienté données, léger, facile à lire par les machines et les humains.
- **XML** : Plus verbeux, riche en métadonnées, souvent utilisé pour les documents complexes.
- **YAML** : Format lisible pour les configurations et plus concis que JSON ou XML.

---

### **2. Fonctionnalités principales de CouchDB**

- **Documents comme unités fondamentales** : 
  - Représentation JSON des données.
  - Chaque document a un identifiant unique (`_id`) et un numéro de version (`_rev`).

- **Accès RESTful via HTTP** : 
  - CouchDB expose ses ressources via une interface REST.
  - Méthodes disponibles :
    - `GET` : Récupérer des données.
    - `POST` : Ajouter des documents ou exécuter des vues.
    - `PUT` : Mettre à jour ou créer des documents.
    - `DELETE` : Supprimer des documents.

- **MVCC (Multi-Version Concurrency Control)** : 
  - Gestion des conflits entre versions.
  - Préserve l’intégrité des données dans un environnement distribué.

---

### **3. Requêtage et vues dans CouchDB**

#### **Définition des vues**
- **Vue** : 
  - Document JSON définissant une tâche Map/Reduce.
  - Utilise JavaScript pour mapper (Map) et réduire (Reduce) les données.

- **Types de vues** : 
  - **Permanentes** :
    - Stockées dans un design document.
    - Performantes grâce à l’indexation via B+Trees.
  - **Temporaires** :
    - Calculées à la volée.
    - Utilisées pour des tests ponctuels (moins efficaces).

#### **Exemples pratiques**
- **Lister tous les noms des documents** :
  ```json
  {
    "views": {
      "all": {
        "map": "function(doc) { emit(null, doc.name); }"
      }
    }
  }
  ```
- **Indexation sur clé (prénoms → nationalités)** :
  ```json
  {
    "views": {
      "players": {
        "map": "function(doc) { if (doc.type === 'player') emit(doc.firstname, doc.nationality); }"
      }
    }
  }
  ```

---

### **4. Map/Reduce dans CouchDB**

#### **Cheat Sheet : Principes de Map/Reduce**

| **Étape**     | **Fonction**                                      | **Objectif**                                                   |
|---------------|---------------------------------------------------|---------------------------------------------------------------|
| **Map**       | `function(doc) { emit(key, value); }`             | Génére une liste clé/valeur pour chaque document.             |
| **Reduce**    | `function(keys, values) { return aggregatedValue; }` | Regroupe et réduit les résultats en une seule valeur.         |
| **Finalisation** | Agrégation des résultats | CouchDB finalise la vue pour une utilisation optimale. |

#### **Qu'est-ce que Map/Reduce ?**

Map/Reduce est une méthode utilisée dans CouchDB pour organiser, filtrer, et agréger (regrouper) des données. Cela vous permet de rechercher, classer, ou compter les informations de manière plus efficace.

- **Map (Cartographier)** : Cette étape parcourt tous vos documents et "émet" des paires clé/valeur.
- **Reduce (Réduire)** : Cette étape prend toutes les paires clé/valeur émises par la fonction Map et les "réduit" pour donner un résultat plus simple, comme un total ou une liste.

#### **Pourquoi utiliser Map/Reduce ?**

Supposons que vous avez une grande quantité de données (comme des informations sur des joueurs de tennis dans différentes villes), et vous souhaitez obtenir un **compte du nombre de joueurs dans chaque ville**. C'est ici que Map/Reduce entre en jeu !

---

#### **1. Création d'une Vue Map/Reduce**

##### **Étape 1 : Définir une vue Map**

Une vue Map vous permet de "lire" vos documents et d'extraire les informations pertinentes.

Voici comment créer une vue simple pour lister les **prénoms des joueurs** à partir de vos documents CouchDB.

**Vue Map (extraction des prénoms) :**

1. Allez dans votre base de données CouchDB.
2. Créez un **design document** où vous allez définir votre vue. Ce document contient toutes les vues pour une certaine collection de données.

Dans votre design document, ajoutez la vue **Map** suivante :

```json
{
  "_id": "_design/players",
  "views": {
    "listNames": {
      "map": "function(doc) { emit(doc.firstName, null); }"
    }
  }
}
```

- **map** : Cette fonction `emit` prend le prénom du joueur (`doc.firstName`) et l'affiche.
- `null` : Cela signifie que nous ne retournons aucune autre donnée pour l'instant, juste les prénoms.

##### **Étape 2 : Appeler la Vue avec CURL**

Une fois la vue définie, vous pouvez l'interroger (lancer la vue) pour obtenir des résultats.

Voici comment appeler la vue `listNames` via une commande **CURL** (ce qui est comme un "clic" pour récupérer les résultats dans la base de données).

```bash
curl -X GET localhost:5984/votre_base_de_donnees/_design/players/_view/listNames
```

Cela vous retournera une liste de tous les prénoms des joueurs dans la base de données.

---

#### **2. Ajout de l'étape Reduce**

Maintenant, imaginons que vous souhaitiez compter combien de joueurs il y a dans chaque ville, plutôt que simplement lister les prénoms. Vous pouvez utiliser la fonction **Reduce** pour agrégater les résultats.

##### **Vue Map avec Reduce**

Voici un exemple où on ajoute un **compteur** pour chaque ville. Nous allons émettre la ville (clé) et chaque joueur dans cette ville (valeur).

```json
{
  "_id": "_design/tournaments",
  "views": {
    "playersInCity": {
      "map": "function(doc) { emit(doc.city, 1); }",
      "reduce": "_sum"
    }
  }
}
```

- **map** : Pour chaque joueur, on `émets` la ville (`doc.city`) et on associe un "1" (signifiant un joueur dans cette ville).
- **reduce** : La fonction spéciale `_sum` permet de **additionner** tous les "1" pour chaque ville. Cela vous donne le **nombre total de joueurs dans chaque ville**.

##### **Étape 3 : Appeler la Vue avec Reduce**

Maintenant que nous avons ajouté la fonction **Reduce**, appelons la vue pour obtenir le total des joueurs dans chaque ville.

```bash
curl -X GET localhost:5984/votre_base_de_donnees/_design/tournaments/_view/playersInCity
```

Cela retournera un résultat comme celui-ci :

```json
{
  "rows": [
    {"key": "Paris", "value": 5},
    {"key": "Lyon", "value": 3},
    {"key": "Marseille", "value": 4}
  ]
}
```

Cela vous donne le nombre total de joueurs dans chaque ville :  
- **Paris** : 5 joueurs  
- **Lyon** : 3 joueurs  
- **Marseille** : 4 joueurs

---

#### **3. Personnalisation de vos vues Map/Reduce**

Maintenant que vous avez vu comment créer une vue Map et l'utiliser avec Reduce, voici quelques idées de personnalisation :

##### **A. Regrouper les joueurs par genre et par ville**

```json
{
  "_id": "_design/players",
  "views": {
    "groupByCityAndGender": {
      "map": "function(doc) { emit([doc.city, doc.gender], 1); }",
      "reduce": "_sum"
    }
  }
}
```

Ici, nous utilisons une **clé composite** (ville + genre). Chaque joueur est compté par ville et genre.

##### **B. Regrouper par nationalité et genre**

```json
{
  "_id": "_design/players",
  "views": {
    "groupByNationalityAndGender": {
      "map": "function(doc) { emit([doc.nationality, doc.gender], 1); }",
      "reduce": "_sum"
    }
  }
}
```

Cela permet de compter combien de joueurs existent pour chaque combinaison de nationalité et de genre.

---

#### **4. Résumé et bonnes pratiques**

- **Map** : Créez des paires clé/valeur à partir de vos documents pour extraire ou transformer les données.
- **Reduce** : Utilisez des fonctions d'agrégation comme `_sum`, `_count`, ou écrivez votre propre fonction pour regrouper les résultats.
- **Vue permanente** : Créez des vues qui peuvent être appelées plusieurs fois pour des requêtes répétées.

Les vues Map/Reduce vous permettent de structurer vos données dans CouchDB d'une manière efficace et flexible. Grâce à cette méthode, vous pouvez filtrer, trier, et agréger vos données de manière plus performante.

#### **Astuces supplémentaires** :
- Utilisez **CouchDB Fauxton**, l'interface graphique, pour créer et tester vos vues de manière plus simple si vous ne voulez pas utiliser CURL.
- N'oubliez pas que **Map** est pour l'extraction des données, et **Reduce** est pour leur agrégation.

Avec ces bases, vous êtes maintenant prêt à commencer à exploiter pleinement le potentiel de CouchDB pour travailler avec vos données de manière dynamique et performante !
#### **7. Résumé des bonnes pratiques**

1. **Modélisation des données** :
   - Utiliser `_id` comme identifiant unique.
   - Ajouter des champs de type (`type`) pour différencier les entités.
   - Inclure des timestamps pour suivre les modifications.

2. **Optimisation des vues** :
   - Préférer les vues permanentes pour des requêtes fréquentes.
   - Grouper les vues similaires dans un design document.

3. **Scalabilité horizontale** :
   - Tirer parti du partitionnement et de la réplication.
   - Ajouter des nœuds pour augmenter la capacité.

---

---

### **5. Architecture distribuée**

#### **Partitionnement**
- Les données sont divisées en **shards** (partitions horizontales).
- **Méthodes** :
  - **Hachage cohérent** : Répartition équilibrée sur plusieurs nœuds.
  - **Partitionnement par plages** : Organisation des données par intervalle.

#### **Réplication**
- Réplication pour assurer la disponibilité et éviter les points de défaillance uniques.
- **Paramètres** :
  - `N` : Nombre total de copies.
  - `R` : Quorum de lecture.
  - `W` : Quorum d’écriture.

#### **CAP Theorem**
- **C** : Cohérence.
- **A** : Disponibilité.
- **P** : Tolérance aux partitions.
- **CouchDB** : Se concentre sur **A** et **P** avec cohérence éventuelle.

---

### **Tuto : Comprendre et utiliser les vues Map/Reduce dans CouchDB pour les débutants**

Si vous n'avez jamais utilisé JavaScript ou travaillé avec des vues Map/Reduce, ne vous inquiétez pas ! Ce tutoriel est conçu pour vous guider à travers le processus de manière simple et intuitive, sans avoir besoin de connaissances techniques approfondies.

---


### **Cheat Sheet Résumé**

| **Action**                     | **Commande**                                                                 |
|--------------------------------|-------------------------------------------------------------------------------|
| **Créer une base**             | `curl -X PUT admin:pwd@localhost:5984/db_test`                               |
| **Ajouter un document**        | `curl -X PUT admin:pwd@localhost:5984/db_test/doc_id -d '{"name":"Nadal"}'`  |
| **Lister toutes les bases**    | `curl -X GET admin:pwd@localhost:5984/_all_dbs`                              |
| **Appeler une vue**            | `curl -X GET admin:pwd@localhost:5984/tennis/_design/tournois/_view/city`    |
| **Lister les nœuds du cluster**| `curl -s $COUCH3/_membership | jq`                                           |
| **Lister les partitions**      | `curl -s $COUCH3/tennis/_shards | jq`                                        |

CouchDB offre une flexibilité inégalée pour gérer des données semi-structurées dans des environnements distribués. Ce guide et cheat sheet facilitent une prise en main rapide et efficace.