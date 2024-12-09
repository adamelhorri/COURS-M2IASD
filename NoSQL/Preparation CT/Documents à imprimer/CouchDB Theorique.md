---
tags:
  - nosql
---
# Sommaire

1. [CouchDB et la flexibilité des données](#couchdb-et-la-flexibilité-des-données)
   1. [Introduction à JSON](#introduction-à-json)
   2. [Langages pour données semi-structurées](#langages-pour-données-semi-structurées)
2. [Comparaison JSON vs XML](#comparaison-json-vs-xml)
   1. [JSON](#json)
   2. [XML](#xml)
   3. [Observation](#observation)
3. [JSON (JavaScript Object Notation)](#json-javascript-object-notation)
   1. [Introduction](#1-introduction)
   2. [Principes structuraux](#2-principes-structuraux)
4. [Principes Généraux de CouchDB](#principes-généraux-de-couchdb)
   1. [CouchDB : Un système orienté documents](#1-couchdb--un-système-orienté-documents)
   2. [Architecture REST](#2-architecture-rest)
   3. [Vision document](#3-vision-document)
5. [GeoJSON et son utilisation](#geojson-et-son-utilisation)
   1. [Description](#1-description)
   2. [Exemple d’objet GeoJSON](#2-exemple-dobjet-geojson)
   3. [Visualisation](#3-visualisation)
6. [Partitionnement et réplication](#partitionnement-et-réplication)
   1. [Partitionnement](#1-partitionnement)
   2. [Réplication](#2-réplication)
   3. [Paramètres clés](#paramètres-clés)
7. [Système distribué et principes CAP](#système-distribué-et-principes-cap)
   1. [CAP Theorem](#1-cap-theorem)
   2. [CouchDB](#2-couchdb)
      1. [Stratégie d’organisation](#stratégie-dorganisation)
8. [Instructions pour gestion distribuée](#instructions-pour-gestion-distribuée)
   1. [Lister les nœuds du cluster](#1-lister-les-nœuds-du-cluster)
   2. [Lister les partitions](#2-lister-les-partitions)
   3. [Informations sur un document spécifique](#3-informations-sur-un-document-spécifique)

---

*Note : Les liens d'ancrage (`#`) sont générés automatiquement en fonction des titres. Assurez-vous que les en-têtes de votre document correspondent exactement aux textes des liens pour un fonctionnement optimal.*
#### **CouchDB et la flexibilité des données**

1. **Introduction à JSON :**
   
   - **Structure semi-structurée :**
     - Flexible : gestion des attributs manquants ou supplémentaires.
     - Attributs pouvant contenir des valeurs atomiques ou des collections.
     - Données auto-descriptives (valeurs et schéma coexistent).

2. **Langages pour données semi-structurées :**
   
   - **Langages de balisage :** XML.
   - **Formats de données :**
     - JSON : orienté données.
     - YAML : orienté configuration et lisibilité.

### **Comparaison JSON vs XML**

#### **1. JSON :**

- **Structure :**
  - Les annotations (métadonnées) sont placées sur les **arêtes** du graphe.
  - Les valeurs sont stockées dans les **nœuds** du graphe.
  
- **Exemple visuel :**
  - Attributs comme `firstName`, `lastName`, et `age` sont reliés au nœud principal `winner`.
  - Les valeurs comme `Rodger`, `Federer`, et `34` sont dans les nœuds terminaux.

#### **2. XML :**

- **Structure :**
  - Les annotations et les valeurs sont **ensemble sur les nœuds**.
  - La structure est plus lourde, chaque donnée étant encapsulée dans une balise.
  
- **Exemple visuel :**
  - Chaque élément (`winner`, `firstName`, `lastName`, `age`) contient directement sa valeur dans les nœuds.

#### **Observation :**

- **JSON :**
  - Léger, simple, orienté données, facile à manipuler.
  - Idéal pour des structures flexibles.
  
- **XML :**
  - Plus verbeux, orienté balisage.
  - Offre plus de métadonnées mais peut être plus complexe à traiter.

![Image](Pasted%20image%2020241118172806.png)

### **JSON (JavaScript Object Notation)**

#### **1. Introduction :**

- Format d’échange de données simple et efficace.
- **Caractéristiques :**
  - Exploitable par les machines et lisible par les humains.
  - Sous-ensemble de JavaScript mais indépendant de tout langage.
  - Format de sérialisation d’objets JavaScript (et bien plus).
  - Devenu un standard pour le Web.

---

#### **2. Principes structuraux :**

- **Paires clé/valeur :**
  - Réification selon les langages : objet, enregistrement, ou table associative.
  - Exemple : `"lastName": "Federer"`.
  
- **Tableaux de valeurs :**
  - Collection ordonnée de valeurs (de types différents).
  - Réification : tableau, vecteur, liste, ou suite.

### **Principes Généraux de CouchDB**

#### **1. CouchDB : Un système orienté documents**

- **Caractéristiques principales :**
  - Chaque document est une unité autonome, textuelle ou multimédia (images, sons, etc.).
  - Fonctionnalités : gestion des versions, évolution, restructuration, réplication et synchronisation.
  
- **Documents comme ressources web :**
  - Identifiés par une **URI**.
  - Manipulables via une architecture **REST** et le protocole **HTTP**.

#### **2. Architecture REST :**

- Basée sur des échanges simples pour gérer les ressources :
  - **GET** : Récupérer une ressource.
  - **PUT** : Créer ou mettre à jour une ressource.
  - **POST** : Ajouter une ressource à une collection ou envoyer des données à un processus.
  - **DELETE** : Supprimer une ressource.

#### **3. Vision document :**

- Modèle de données flexible basé sur des graphes :
  - **Auto-description des données**.
  - Utilisation d’agrégats comme listes, ensembles et enregistrements.
  - **Structure et typage flexibles** : Contraintes définies a priori ou vérifiées a posteriori.
  - Contenu sérialisable en chaînes de caractères.

### **GeoJSON et son utilisation**

1. **Description :**
   - Format dédié aux entités spatiales.
   
2. **Exemple d’objet GeoJSON :**

   ```json
   {
     "type": "Feature",
     "geometry": {
       "type": "Point",
       "coordinates": [2.25, 48.84]
     },
     "properties": {
       "name": "The French Open",
       "year": 2015,
       "director": {"lastName": "Ysern", "firstName": "Gilbert"},
       "players": ["Williams", "Nadal", "Murray", "Simon", "Tsonga", "Cornet"]
     }
   }
   ```

3. **Visualisation :**
   - Fichiers GeoJSON interprétés sous QGIS pour des données géospatiales (exemple : Roland Garros).

### **5. Partitionnement et réplication**

#### **1. Partitionnement :**

- Les données sont divisées en **shards** (partitions horizontales) distribuées sur différents nœuds.
- **Stratégies courantes :**
  - **Hachage cohérent** (`consistent hashing`).
```markdown
Hashage coherent
### **Réponse :**

CouchDB s’inspire de DynamoDB avec deux principes majeurs :

---

### **1. Architecture multi-maîtres**  
Tous les nœuds peuvent accepter des lectures et des écritures. Cela supprime le **Single Point of Failure (SPOF)** grâce à la réplication et au **contrôle de version (MVCC)** pour résoudre les conflits entre nœuds.

---

### **2. Hachage cohérent**  
Les données sont réparties sur les nœuds en utilisant une **fonction de hachage** sur les identifiants des documents. Cela facilite la **scalabilité horizontale** : lors de l’ajout d’un nœud, seuls ses voisins sont impactés.

---

### **Schéma du Hachage cohérent**  


+---------+         +---------+
| Nœud 1  | ------> | Nœud 2  |
+---------+         +---------+
     ↑                   ↓
+---------+ <------ +---------+
| Nœud 4  |         | Nœud 3  |
+---------+         +---------+


Chaque nœud gère une partie des clés, assurant une répartition équilibrée et efficace.

```
  - **Partitionnement par plage** (`range partitioning`).
  
- **Exemple :**
  - Une base avec 24 shards distribués sur 3 nœuds (8 shards/nœud).

#### **2. Réplication :**

- Les shards sont répliqués pour éviter un SPOF (Single Point of Failure).
- **Exemple :** Système **multi-master** où chaque nœud peut traiter les écritures.

#### **Paramètres clés :**

- **`N`** : Nombre de copies des données (exemple : 3).
- **`R`** : Quorum de nœuds pour une lecture.
- **`W`** : Quorum de nœuds pour une écriture.

---

### **6. Système distribué et principes CAP**

1. **CAP Theorem :**
   - **C** : Cohérence (consistency).
   - **A** : Disponibilité (availability).
   - **P** : Tolérance aux partitions (partition tolerance).
   
2. **CouchDB :**
   - Se concentre sur **A** et **P**, avec une cohérence éventuelle.
   - Adapté à la **scalabilité horizontale** : ajout de nœuds facile et gestion transparente.
   - QNRW:
```md
Le système **QNRW** est un modèle utilisé dans les bases de données distribuées pour équilibrer **scalabilité**, **réplication** et **partitionnement**, tout en respectant le **postulat CAP**. Les données sont d’abord divisées en **shards** grâce au partitionnement, permettant leur répartition sur plusieurs nœuds pour une meilleure scalabilité. Chaque shard est ensuite **répliqué** sur plusieurs nœuds, garantissant la tolérance aux pannes et évitant la perte de données. Le **postulat CAP** impose de choisir entre cohérence, disponibilité et tolérance aux pannes réseau. Les paramètres **Q, N, R, W** définissent ce fonctionnement : **N** correspond au nombre total de réplicas, **R** au quorum de réplicas nécessaires pour lire, et **W** pour écrire. Par exemple, avec **N=3, R=2, W=2**, une opération est validée dès qu’elle atteint un quorum, assurant un équilibre entre cohérence et disponibilité.
```

#### **Stratégie d’organisation :**

- Les nœuds voisins sont impactés lors de l’ajout/suppression de nœuds.
- Placement basé sur une **fonction de hachage** appliquée à l’ID.

---

### **7. Instructions pour gestion distribuée**

1. **Lister les nœuds du cluster :**
   
   ```bash
   curl -s $COUCH3/_membership | jq
   ```

2. **Lister les partitions :**
   
   ```bash
   curl -s $COUCH3/tennis/_shards | jq
   ```

3. **Informations sur un document spécifique (exemple : `caroGa_93`) :**
   
   ```bash
   curl -s $COUCH3/tennis/_shards/caroGa_93 | jq
   ```

---
