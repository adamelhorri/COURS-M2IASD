---
tags:
  - nosql
---
#### **CouchDB et la flexibilité des données**

- **Introduction à JSON :**
    
    - Structure semi-structurée :
        - Flexible : gestion des attributs manquants ou supplémentaires.
        - Attributs pouvant contenir des valeurs atomiques ou des collections.
        - Données auto-descriptives (valeurs et schéma coexistent).
- **Langages pour données semi-structurées :**
    
    - **Langages de balisage :** XML.
    - **Formats de données :**
        - JSON : orienté données.
        - YAML : orienté configuration et lisibilité.

---

#### **Comparaison JSON vs XML**

### **Comparaison JSON vs XML**

#### **JSON :**

- **Structure :**
    - Les annotations (métadonnées) sont placées sur les **arêtes** du graphe.
    - Les valeurs sont stockées dans les **nœuds** du graphe.
- **Exemple visuel :**
    - Attributs comme `firstName`, `lastName`, et `age` sont reliés au nœud principal `winner`.
    - Les valeurs comme `Rodger`, `Federer`, et `34` sont dans les nœuds terminaux.

#### **XML :**

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

- **JSON :**
    
    - Annotations sur les arêtes et valeurs sur les nœuds.
    - Exemple :
        
        json
        
        Copier le code
        
        `{   "winner": {     "firstName": "Rodger",     "lastName": "Federer",     "age": 34   } }`
        
- **XML :**
    
    - Annotations et valeurs sur les nœuds.
    - Exemple :
        
        xml
        
        Copier le code
        
        `<winner>   <firstName>Rodger</firstName>   <lastName>Federer</lastName>   <age>34</age> </winner>`
        

---

#### **CouchDB : Principes de base**

- **Construction et alimentation.**
- **Consultation.**
- **Organisation des documents.**
- **Distribution.**

### **JSON (JavaScript Object Notation)**

#### **Introduction :**

- Format d’échange de données simple et efficace.
- **Caractéristiques :**
    - Exploitable par les machines et lisible par les humains.
    - Sous-ensemble de JavaScript mais indépendant de tout langage.
    - Format de sérialisation d’objets JavaScript (et bien plus).
    - Devenu un standard pour le Web.

---

#### **Principes structuraux :**

- **Paires clé/valeur :**
    - Réification selon les langages : objet, enregistrement, ou table associative.
    - Exemple : `"lastName": "Federer"`.
- **Tableaux de valeurs :**
    - Collection ordonnée de valeurs (de types différents).
    - Réification : tableau, vecteur, liste, ou suite.
    - Exemple :
        
        json
        
        Copier le code
        
        `"players": ["Ozaka", "Garcia", "Sinner", "Alcaraz"]`
        

---

#### **Exemples de structures JSON :**

- **Objets simples :**
    - Exemple : `{ "lastName": "Federer", "age": 40 }`.
- **Objets composés :**
    - Exemple :
        
        json
        
        Copier le code
        
        `{   "winner": {     "lastName": "Federer",     "firstName": "Rodger",     "age": 40   } }`
        
- **Objets complexes (vision document) :**
    - Exemple :
        
        json
        
        Copier le code
        
        `{   "tournament": "The French Open",   "year": 2015,   "director": {     "lastName": "Ysern",     "firstName": "Gilbert"   },   "players": ["Williams", "Nadal", "Djokovic", "Murray"] }`
        

---

#### **Formats dédiés :**

- Pour des thématiques spécifiques :
    - **GeoJSON** : Données géographiques.
    - **BioJSON** : Données biologiques.
    - **TopoJSON** : Données topologiques.
    - Autres formats similaires.
### **Principes Généraux de CouchDB**

#### **CouchDB : Un système orienté documents**

- **Caractéristiques principales :**
    - Chaque document est une unité autonome, textuelle ou multimédia (images, sons, etc.).
    - Fonctionnalités : gestion des versions, évolution, restructuration, réplication et synchronisation.
- **Documents comme ressources web :**
    - Identifiés par une **URI**.
    - Manipulables via une architecture **REST** et le protocole **HTTP**.

#### **Architecture REST :**

- Basée sur des échanges simples pour gérer les ressources :
    - **GET** : Récupérer une ressource.
    - **PUT** : Créer ou mettre à jour une ressource.
    - **POST** : Ajouter une ressource à une collection ou envoyer des données à un processus.
    - **DELETE** : Supprimer une ressource.

#### **Vision document :**

- Modèle de données flexible basé sur des graphes :
    - **Auto-description des données**.
    - Utilisation d’agrégats comme listes, ensembles et enregistrements.
    - **Structure et typage flexibles** : Contraintes définies a priori ou vérifiées a posteriori.
    - Contenu sérialisable en chaînes de caractères.

---

### **GeoJSON et son utilisation**

- Format dédié aux entités spatiales.
- Exemple d’objet GeoJSON :
    
    json
    
    Copier le code
    
    `{   "type": "Feature",   "geometry": {     "type": "Point",     "coordinates": [2.25, 48.84]   },   "properties": {     "name": "The French Open",     "year": 2015,     "director": {"lastName": "Ysern", "firstName": "Gilbert"},     "players": ["Williams", "Nadal", "Murray", "Simon", "Tsonga", "Cornet"]   } }`
    
- **Visualisation :** Fichiers GeoJSON interprétés sous QGIS pour des données géospatiales (exemple : Roland Garros).

---

### **Systèmes NoSQL orientés documents**

- **Exemples de solutions populaires :**
    - MongoDB (utilise BSON : Binary JSON).
    - Apache CouchDB (inspiré de Lotus Notes).
    - CouchBase (dérivé de CouchDB).
    - IBM Cloudant.
    - Realm (base de données embarquée, rachetée par MongoDB).

#### **CouchDB :**

- Conçu pour tourner sur des infrastructures matérielles peu fiables (**Cluster of Unreliable Commodity Hardware**).
- Basé sur des principes simples et efficaces, adaptés au web.
### **Exemples d’interactions avec le client REST CURL (Client URL Library)**

#### **1. Introduction : Interactions de base**

- **Obtenir les informations du serveur CouchDB :**
    
    bash
    
    Copier le code
    
    `$ curl -X GET localhost:5984 {"couchdb":"Welcome","version":"3.3.3", ...}`
    
- **Créer une base de données (authentification requise) :**
    
    bash
    
    Copier le code
    
    `$ curl -X PUT admin:pwd@localhost:5984/db_test {"ok":true}`
    
- **Lister toutes les bases de données :**
    
    bash
    
    Copier le code
    
    `$ curl -X GET admin:pwd@localhost:5984/_all_dbs ["db_test","exam","tennis",...]`
    

---

#### **2. Gestion des documents**

- **Ajouter un document dans une base :**
    
    bash
    
    Copier le code
    
    `$ curl -X PUT admin:pwd@localhost:5984/db_test/o1 -d '{"name":"Nadal"}' {"ok":true,"id":"o1","rev":"1-f28a4a5baf607e908cea5863c324d147"}`
    
- **Récupérer un document avec son URI :**
    
    bash
    
    Copier le code
    
    `$ curl -X GET admin:pwd@localhost:5984/db_test/o1 {"_id":"o1","_rev":"1-f28a4a5baf607e908cea5863c324d147","name":"Nadal"}`
    

---

#### **3. Gestion des versions**

- **Échec lors de la mise à jour sans spécifier la version actuelle :**
    
    bash
    
    Copier le code
    
    `$ curl -X PUT admin:pwd@localhost:5984/db_test/o1 -d '{"name":"Nadal", "age":36}' {"error":"conflict","reason":"Document update conflict."}`
    
- **Mise à jour réussie avec la version actuelle :**
    
    bash
    
    Copier le code
    
    `$ curl -X PUT admin:pwd@localhost:5984/db_test/o1 -d '{"_rev":"1-f28a4a5baf607e908cea5863c324d147","name":"Nadal", "age":36}' {"ok":true,"id":"o1","rev":"2-b3a6b46d95c7671046d98826c2010595"}`
    

---

#### **4. Obtenir des UUIDs et métadonnées**

- **Générer un UUID unique :**
    
    bash
    
    Copier le code
    
    `$ curl -X GET admin:pwd@localhost:5984/_uuids {"uuids":["415da9c4f3bd245ea2fbbeaab600057c"]}`
    
- **Afficher les métadonnées des documents :**
    
    bash
    
    Copier le code
    
    `$ curl admin:pwd@localhost:5984/db_test/_all_docs`
    

---

#### **5. Chargement de fichiers JSON**

- **Charger un document sans identifiant via `PUT` :**
    
    bash
    
    Copier le code
    
    `$ curl -X PUT admin:pwd@localhost:5984/db_test/o5 -d @tsonga.json -H "Content-Type: application/json" {"ok":true,"id":"o5","rev":"1-87f50d9c01b37db251d7e12a8ad0ce69"}`
    
- **Charger un document avec un identifiant inclus via `POST` :**
    
    bash
    
    Copier le code
    
    `$ curl -X POST admin:pwd@localhost:5984/db_test -d @monfils.json -H "Content-Type: application/json" {"ok":true,"id":"laMonf","rev":"1-341422aac64aa47ff3a933bb9d62c5b1"}`
    

---

#### **6. Chargement par lots**

- **Ajouter plusieurs documents en une seule requête :**
    
    bash
    
    Copier le code
    
    `$ curl -X POST admin:pwd@localhost:5984/db_test/_bulk_docs -d @murrayDjoko.json -H "Content-Type: application/json" [{"ok":true,"id":"murray","rev":"1-7d4b633c14d3b66fd2c333947627f7ef"},  {"ok":true,"id":"djoko","rev":"1-0620108d9d04bbaa9b55c826664a244d"}]`
    

---

#### **7. Suppression de documents ou bases**

- **Supprimer un document (suppression logique) :**
    
    bash
    
    Copier le code
    
    `$ curl -X DELETE admin:pwd@localhost:5984/db_test/o1?rev=2-b3a6b46d95c7671046d98826c2010595 {"ok":true,"id":"o1","rev":"3-c3e4361ab165df874c0ee1d92966d8de"}`
    
- **Supprimer une base de données entière :**
    
    bash
    
    Copier le code
    
    `$ curl -X DELETE admin:pwd@localhost:5984/other_db {"ok":true}`
    

---

### **Résumé des propriétés importantes :**

- Chaque document possède :
    - **`_id`** : Clé ou racine unique de l'agrégat.
    - **`_rev`** : Numéro de version pour la gestion des modifications.
- CouchDB utilise le **MVCC (Multi-Version Concurrency Control)** :
    - Conserve plusieurs versions de documents.
    - Résout les conflits avec les versions.
### **Le requêtage et la notion de vue dans CouchDB**

#### **Définition des vues**

- Une vue dans CouchDB est similaire à un document JSON décrivant une tâche **Map/Reduce** définie en **JavaScript**.
- Le résultat d’une vue est également un document JSON.

#### **Types de vues**

1. **Vue permanente** :
    - Intégrée dans un document nommé "design document".
    - Matérialisée et indexée sur la clé grâce à une structure **B+Tree**.
    - Performante pour des requêtes répétées.
2. **Vue temporaire** :
    - Calculée à la volée.
    - Moins efficace et non recommandée pour des requêtes fréquentes.

---

### **Organisation des vues**

- Les vues sont organisées dans des **design documents**.
- Ces documents permettent de regrouper plusieurs vues liées à une même collection de données.

---

### **Exemples de fonctions Map**

1. **Fonction Map pour retourner tous les documents** :
    
    - Liste tous les noms des documents de la base.
    - Exemple :
        
        json
        
        Copier le code
        
        `{   "views": {     "all": {       "map": "function(doc) {emit(null, doc.name); }"     }   } }`
        
2. **Fonction Map identique à `all_docs` :**
    
    - Retourne les identifiants et les révisions des documents.
    - Exemple :
        
        json
        
        Copier le code
        
        `{   "views": {     "allDocs": {       "map": "function(doc) {emit(doc._id, {\"rev\": doc._rev}); }"     }   } }`
        
3. **Fonction Map pour indexer sur une clé :**
    
    - Indexe les prénoms des joueurs et pointe vers leurs nationalités.
    - Exemple :
        
        json
        
        Copier le code
        
        `{   "views": {     "players": {       "map": "function(doc) {if (doc.type=='player') {emit(doc.firstname, doc.nationality); }}"     }   } }`
        
    - Une fois enregistrée dans un **design document**, cette vue peut être rappelée à tout moment.

---

### **Intérêt des B+Trees**

- Les B+Trees permettent :
    - Une **indexation rapide** et efficace des clés.
    - Une navigation optimisée à travers de grandes collections de documents.
- **Illustration :** Les vues permanentes sont stockées et organisées en utilisant cette structure.

---

### **Principes Map/Reduce**

- **Map** : Applique une fonction à chaque document et génère une paire clé/valeur.
- **Reduce** : Combine les résultats de la fonction Map pour produire un résultat agrégé.

---

### **Applicatif Web Fauxton**

- Interface web intégrée pour gérer CouchDB.
- Permet de visualiser, créer, et exécuter des vues.
### **Synthèse détaillée des vues Map/Reduce avec CouchDB et exemples pratiques**

---

### **Exemple de document joueur**

Voici un exemple typique de document JSON représentant un joueur dans CouchDB :

json

Copier le code

`{   "_id": "caroGa_93",   "_rev": "1-82098e47935455bd782a636f08629008",   "lastname": "Garcia",   "firstname": "Caroline",   "type": "player",   "age": 31,   "gender": "F",   "ranking": 24,   "nationality": "France",   "tournaments": [     {"city": "Bogota", "year": 2012},     {"city": "Limoges", "year": 2014},     {"city": "Strasbourg", "year": 2015}   ] }`

---

### **Création de vues avec fonctions Map et Reduce**

#### **Exemple 1 : Vue `cityYear`**

- **Objectif** : Émettre les paires `[ville, année]` pour chaque tournoi.
- **Code JavaScript de la vue :**

json

Copier le code

`{   "_id": "_design/tournois",   "_rev": "10-97d92de913e026e418e305863b991054",   "views": {     "cityYear": {       "map": "function (doc) { if (doc.tournaments) { for (var i in doc.tournaments) emit([doc.tournaments[i].city, doc.tournaments[i].year], 1); } }",       "reduce": "_count"     }   },   "language": "javascript" }`

- **Requête cURL pour appeler cette vue :**

bash

Copier le code

`curl -X GET admin:pwd@localhost:5984/tennis/_design/tournois/_view/cityYear`

- **Résultats (JSON)** :

json

Copier le code

`{   "total_rows": 12,   "offset": 0,   "rows": [     {"id": "caroGa_93", "key": ["Bogota", 2012], "value": 1},     {"id": "caroGa_93", "key": ["Limoges", 2014], "value": 1},     {"id": "caroGa_93", "key": ["Strasbourg", 2015], "value": 1},     {"id": "laMonf", "key": ["Doha", 2018], "value": 1},     {"id": "JoTs", "key": ["Cassis", 2019], "value": 1}   ] }`

---

#### **Exemple 2 : Vue `city` avec agrégats**

- **Objectif** : Compter le nombre de joueurs ayant participé à un tournoi dans chaque ville.
- **Code JavaScript :**

json

Copier le code

`{   "views": {     "city": {       "map": "function (doc) { if (doc.tournaments) { for (var i in doc.tournaments) emit(doc.tournaments[i].city, 1); } }",       "reduce": "_count"     }   },   "language": "javascript" }`

- **Requête cURL avec `reduce=false` :**

bash

Copier le code

`curl -X GET localhost:5984/tennis/_design/tournois/_view/city?reduce=false`

- **Résultats :**

json

Copier le code

`{   "total_rows": 12,   "offset": 0,   "rows": [     {"id": "caroGa_93", "key": "Bogota", "value": 1},     {"id": "caroGa_93", "key": "Limoges", "value": 1},     {"id": "caroGa_93", "key": "Strasbourg", "value": 1},     {"id": "laMonf", "key": "Doha", "value": 1},     {"id": "JoTs", "key": "Cassis", "value": 1}   ] }`

- **Requête cURL avec `reduce=true` (agrégat) :**

bash

Copier le code

`curl -X GET localhost:5984/tennis/_design/tournois/_view/city`

- **Résultat agrégé (par ville) :**

json

Copier le code

`{   "rows": [     {"key": "Bogota", "value": 1},     {"key": "Limoges", "value": 1},     {"key": "Strasbourg", "value": 1},     {"key": "Doha", "value": 1},     {"key": "Cassis", "value": 1}   ] }`

---

#### **Exemple 3 : Vue `testAgregat` avec clé composite**

- **Objectif** : Émettre une clé composite `[genre, nationalité, ID]` pour les joueurs, et compter leur occurrence.
- **Code JavaScript :**

json

Copier le code

`{   "views": {     "testAgregat": {       "map": "function (doc) { if (doc.type=='player') emit([doc.gender, doc.nationality, doc._id], 1); }",       "reduce": "_count"     }   },   "language": "javascript" }`

- **Requête cURL pour regrouper par genre uniquement (`group_level=1`) :**

bash

Copier le code

`curl -X GET localhost:5984/tennis/_design/tournois/_view/testAgregat?group_level=1`

- **Résultat (agrégation par genre) :**

json

Copier le code

`{   "rows": [     {"key": "F", "value": 3},     {"key": "M", "value": 7}   ] }`

- **Requête cURL pour regrouper par genre et nationalité (`group_level=2`) :**

bash

Copier le code

`curl -X GET localhost:5984/tennis/_design/tournois/_view/testAgregat?group_level=2`

- **Résultat (agrégation par genre et nationalité) :**

json

Copier le code

`{   "rows": [     {"key": ["F", "France"], "value": 3},     {"key": ["M", "France"], "value": 4},     {"key": ["M", "Serbia"], "value": 3}   ] }`

---

### **Vue `allTennismen` : Nationalité des joueurs**

- **Objectif** : Retourner la nationalité de tous les joueurs.
- **Code JavaScript :**

json

Copier le code

`{   "_id": "_design/allTennismen",   "language": "javascript",   "views": {     "allT": {       "map": "function (doc) { if (doc.type=='player') emit(doc._id, {\"nationality\": doc.nationality}); }",       "reduce": "_count"     }   } }`

- **Requête cURL sans fonction `reduce` :**

bash

Copier le code

`curl -X GET localhost:5984/test_db/_design/allTennismen/_view/allT?reduce=false`

- **Résultats :**

json

Copier le code

`{   "total_rows": 6,   "offset": 0,   "rows": [     {"id": "caroGa_93", "key": "caroGa_93", "value": {"nationality": "France"}},     {"id": "djoko", "key": "djoko", "value": {"nationality": "Serbia"}},     {"id": "JoTs", "key": "JoTs", "value": {"nationality": "France"}}   ] }`

---

### **Résumé**

1. **Clé simple** : Retourner des données par ville, année ou identifiant.
2. **Clé composite** : Combiner plusieurs attributs pour des agrégations complexes.
3. **Réduction** : Agréger les données pour des analyses synthétiques (ex. nombre de joueurs par ville).
4. **CURL** : Tester les vues et afficher les résultats via l'interface REST.
### **Synthèse : Concepts avancés de CouchDB**

---

### **1. Différents niveaux d’agrégats**

- **Regrouper sans niveau (total global)** :
    
    bash
    
    Copier le code
    
    `curl -X GET admin:pwd@localhost:5984/tennis/_design/tournois/_view/testAgregat`
    
    **Résultat :**
    
    json
    
    Copier le code
    
    `{"rows":[{"key":null,"value":6}]}`
    
- **Regrouper par le premier attribut de la clé** (exemple : genre) :
    
    bash
    
    Copier le code
    
    `curl -X GET admin:pwd@localhost:5984/tennis/_design/tournois/_view/testAgregat?group_level=1`
    
    **Résultat :**
    
    json
    
    Copier le code
    
    `{"rows":[   {"key":["F"],"value":2},   {"key":["M"],"value":4} ]}`
    
- **Regrouper par les deux premiers attributs de la clé** (exemple : genre et nationalité) :
    
    bash
    
    Copier le code
    
    `curl -X GET admin:pwd@localhost:5984/tennis/_design/tournois/_view/testAgregat?group_level=2`
    
    **Résultat :**
    
    json
    
    Copier le code
    
    `{"rows":[{"key":["F","France"],"value":2}, ...]}`
    

---

### **2. Fonction Reduce personnalisée**

- **Objectif** : Créer une liste des prénoms des joueurs par nationalité.
    
- **Fonctions Map/Reduce** :
    
    - **Map :**
        
        javascript
        
        Copier le code
        
        `function(doc) {   if (doc.type == 'player') emit(doc.nationality, doc.firstname); }`
        
    - **Reduce :**
        
        javascript
        
        Copier le code
        
        `function(keys, values) {   var firstnames = [];   values.forEach(function(v) { firstnames = firstnames.concat(v); });   return firstnames; }`
        
- **Résultat de l’appel de la vue avec Reduce :**
    
    json
    
    Copier le code
    
    `{"rows":[   {"key":"France","value":["Caroline","Jo","Gael","Kristina"]},   {"key":"Great Britain","value":["Andy"]},   {"key":"Serbia","value":["Novak"]} ]}`
    

---

### **3. Organisation des données sans schéma**

#### **Bonnes pratiques :**

1. Utiliser **`_id`** comme seul identifiant unique.
    - Exemple : UUID ou identifiants naturels (interopérabilité avec d’autres systèmes).
2. Ajouter un champ **`type`** pour différencier les entités (ex. `player`, `country`).
3. Ajouter des **timestamps** pour suivre les dates de création et de mise à jour.
4. Utiliser :
    - **Champs simples** pour décrire une entité cible.
    - **Champs composés** pour représenter des relations.

#### **Dénormalisation vs Référence :**

- **Dénormalisation :**
    - Toutes les informations dans un document unique.
    - Utile pour les données rarement mises à jour.
- **Référence (liens entre documents) :**
    - Permet de structurer les données complexes.
    - Exemple :
        
        json
        
        Copier le code
        
        `{   "_id": "murray",   "firstname": "Andy",   "type": "player",   "nationality": "Great Britain" }, {   "_id": "Great Britain",   "population": 60000000,   "type": "country" }`
        

---

### **4. Collation View (Vue combinée pour les jointures)**

- **Objectif** : Contourner l’absence de jointure native.
    
- **Fonction Map** :
    
    javascript
    
    Copier le code
    
    `function(doc) {   if (doc.type == "country") {     emit([doc._id, 0], [{"pop": doc.population}, {"players": doc.players}]);   } else if (doc.type == "player") {     emit([doc.nationality, 1], [{"name": doc.firstname}, {"rank": doc.ranking}]);   } }`
    
- **Résultats :**
    
    json
    
    Copier le code
    
    `{"total_rows":10,"rows":[   {"id":"France","key":["France",0],"value":[{"pop":50000000},{"players":"60000"}]},   {"id":"JoTs","key":["France",1],"value":[{"name":"Jo"},{"rank":12}]},   {"id":"caroGa_93","key":["France",1],"value":[{"name":"Caroline"},{"rank":24}]},   {"id":"Great Britain","key":["Great Britain",0],"value":[{"pop":60000000},{"players":"10000"}]},   {"id":"murray","key":["Great Britain",1],"value":[{"name":"Andy"},{"rank":80}]} ]}`
    

---

### **5. Partitionnement et réplication**

- **Partitionnement** :
    - Les données sont divisées en **shards** (partitions horizontales) distribuées sur différents nœuds.
    - Stratégies courantes :
        - **Hachage cohérent** (`consistent hashing`).
        - **Partitionnement par plage** (`range partitioning`).
    - Exemple : Une base avec 24 shards distribués sur 3 nœuds (8 shards/nœud).
- **Réplication** :
    - Les shards sont répliqués pour éviter un SPOF (Single Point of Failure).
    - Exemple : Système **multi-master** où chaque nœud peut traiter les écritures.

#### **Paramètres clés :**

- **`N`** : Nombre de copies des données (exemple : 3).
- **`R`** : Quorum de nœuds pour une lecture.
- **`W`** : Quorum de nœuds pour une écriture.

---

### **6. Système distribué et principes CAP**

- **CAP Theorem** :
    - **C** : Cohérence (consistency).
    - **A** : Disponibilité (availability).
    - **P** : Tolérance aux partitions (partition tolerance).
- **CouchDB** :
    - Se concentre sur **A** et **P**, avec une cohérence éventuelle.
    - Adapté à la **scalabilité horizontale** : ajout de nœuds facile et gestion transparente.

#### **Stratégie d’organisation** :

- Les nœuds voisins sont impactés lors de l’ajout/suppression de nœuds.
- Placement basé sur une **fonction de hachage** appliquée à l’ID.

---

### **7. Instructions pour gestion distribuée**

- **Lister les nœuds du cluster :**
    
    bash
    
    Copier le code
    
    `curl -s $COUCH3/_membership | jq`
    
- **Lister les partitions :**
    
    bash
    
    Copier le code
    
    `curl -s $COUCH3/tennis/_shards | jq`
    
- **Informations sur un document spécifique (exemple : `caroGa_93`) :**
    
    bash
    
    Copier le code
    
    `curl -s $COUCH3/tennis/_shards/caroGa_93 | jq`
    

---

