---
tags:
  - nosql
---
### **CouchDB et la flexibilité des données**

---

#### **Introduction à JSON**

- **Structure semi-structurée** : Gestion flexible des attributs (manquants ou supplémentaires), auto-descriptivité (valeurs + schéma), et support de valeurs atomiques ou collections.

---

#### **Langages pour données semi-structurées**

- **XML** : Langage de balisage.
- **JSON** : Format orienté données.
- **YAML** : Format orienté configuration et lisibilité.

---

### **Comparaison JSON vs XML**

- **JSON** :
    
    - Métadonnées sur les **arêtes**, valeurs sur les **nœuds**.
    - Léger, simple, idéal pour structures flexibles.
    - Exemple :  
    ```json
    { "winner": { "firstName": "Rodger", "lastName": "Federer", "age": 34 } }
    ```

- **XML** :
    
    - Métadonnées et valeurs combinées sur les **nœuds**.
    - Plus verbeux, offre davantage de métadonnées.
    - Exemple :  
    ```xml
    <winner><firstName>Rodger</firstName><lastName>Federer</lastName><age>34</age></winner>
    ```

---

#### **CouchDB : Principes de base**

- Construction, alimentation, consultation, organisation et distribution des documents.

---

### **JSON : Introduction et caractéristiques**

- Format d’échange léger, lisible par humains et machines.
- Indépendant des langages, standard du Web.
- Sérialise objets JavaScript et plus.

---

#### **Principes structuraux**

- **Paires clé/valeur** : Objets ou tables associatives (`"lastName": "Federer"`).
- **Tableaux** : Collections ordonnées (`"players": ["Ozaka", "Garcia", "Sinner", "Alcaraz"]`).

---

#### **Exemples de structures JSON**

- **Objets simples** : 
    ```json
    { "lastName": "Federer", "age": 40 }
    ```

- **Objets composés** : 
    ```json
    { "winner": { "lastName": "Federer", "firstName": "Rodger", "age": 40 } }
    ```

- **Objets complexes** :  
    ```json
    { 
        "tournament": "The French Open", 
        "year": 2015, 
        "director": { "lastName": "Ysern", "firstName": "Gilbert" },
        "players": ["Williams", "Nadal", "Djokovic", "Murray"] 
    }
    ```

---

#### **Formats spécifiques**

- **GeoJSON** : Géographique.
- **BioJSON** : Biologique.
- **TopoJSON** : Topologique.

### **Interactions avec le client REST CURL pour CouchDB** (Version condensée)

---

#### **1. Introduction : Interactions de base**

- **Obtenir les infos du serveur** :  
    `curl -X GET localhost:5984`  
    → Réponse :  
    ```json
    {"couchdb":"Welcome","version":"3.3.3", ...}
    ```
    
- **Créer une base de données** :  
    `curl -X PUT admin:pwd@localhost:5984/db_test`  
    → Réponse :  
    ```json
    {"ok":true}
    ```
    
- **Lister toutes les bases** :  
    `curl -X GET admin:pwd@localhost:5984/_all_dbs`  
    → Réponse :  
    ```json
    ["db_test","exam","tennis",...]
    ```
    

---

#### **2. Gestion des documents**

- **Ajouter un document** :  
    `curl -X PUT admin:pwd@localhost:5984/db_test/o1 -d '{"name":"Nadal"}'`  
    → Réponse :  
    ```json
    {"ok":true,"id":"o1","rev":"1-..."}
    ```
    
- **Récupérer un document** :  
    `curl -X GET admin:pwd@localhost:5984/db_test/o1`  
    → Réponse :  
    ```json
    {"_id":"o1","_rev":"1-...","name":"Nadal"}
    ```
    

---

#### **3. Gestion des versions**

- **Échec de mise à jour sans version** :  
    `curl -X PUT admin:pwd@localhost:5984/db_test/o1 -d '{"name":"Nadal", "age":36}'`  
    → Réponse :  
    ```json
    {"error":"conflict"}
    ```
    
- **Mise à jour avec version** :  
    `curl -X PUT admin:pwd@localhost:5984/db_test/o1 -d '{"_rev":"1-...","name":"Nadal", "age":36}'`  
    → Réponse :  
    ```json
    {"ok":true,"id":"o1","rev":"2-..."}
    ```
    

---

#### **4. UUIDs et métadonnées**

- **Générer un UUID unique** :  
    `curl -X GET admin:pwd@localhost:5984/_uuids`  
    → Réponse :  
    ```json
    {"uuids":["415da9c4f3bd245ea2fbbeaab600057c"]}
    ```
    
- **Afficher les métadonnées** :  
    `curl admin:pwd@localhost:5984/db_test/_all_docs`
    

---

#### **5. Chargement de fichiers JSON**

- **Via PUT (avec ID)** :  
    `curl -X PUT admin:pwd@localhost:5984/db_test/o5 -d @tsonga.json -H "Content-Type: application/json"`
    
- **Via POST (ID inclus dans JSON)** :  
    `curl -X POST admin:pwd@localhost:5984/db_test -d @monfils.json -H "Content-Type: application/json"`
    

---

#### **6. Chargement par lots**

- **Ajouter plusieurs documents** :  
    `curl -X POST admin:pwd@localhost:5984/db_test/_bulk_docs -d @murrayDjoko.json -H "Content-Type: application/json"`  
    → Réponse :  
    ```json
    [{"ok":true,"id":"murray","rev":"1-..."}, {"ok":true,"id":"djoko","rev":"1-..."}]
    ```

---

#### **7. Suppression**

- **Document (suppression logique)** :  
    `curl -X DELETE admin:pwd@localhost:5984/db_test/o1?rev=2-...`  
    → Réponse :  
    ```json
    {"ok":true,"id":"o1","rev":"3-..."}
    ```
    
- **Base de données entière** :  
    `curl -X DELETE admin:pwd@localhost:5984/other_db`  
    → Réponse :  
    ```json
    {"ok":true}
    ```
    

---

### **Résumé des propriétés importantes**

- Chaque document :
    
    - `_id` : Identifiant unique.
    - `_rev` : Numéro de version.
- **CouchDB et MVCC** :
    
    - Gestion multi-versions.
    - Résolution des conflits via versionnage.
### **Requêtage et vues dans CouchDB** (Version condensée)

---

#### **Définition des vues**

- Vue : Document JSON définissant une tâche Map/Reduce en JavaScript.  
    Résultat : Document JSON.

---

#### **Types de vues**

- **Vue permanente** :
    
    - Stockée dans un design document, indexée via B+Tree.
    - Performante pour requêtes répétées.
- **Vue temporaire** :
    
    - Calculée à la volée.
    - Moins efficace, déconseillée pour usage fréquent.

---

#### **Organisation des vues**

- Regroupées dans des design documents, associées à une même collection de données.

---

#### **Exemples de fonctions Map**

- **Lister tous les noms** :
    ```json
    {
      "views": {
        "all": {
          "map": "function(doc) {emit(null, doc.name); }"
        }
      }
    }
    ```
    
- **Identifiants et révisions** :
    ```json
    {
      "views": {
        "allDocs": {
          "map": "function(doc) {emit(doc._id, {\"rev\": doc._rev}); }"
        }
      }
    }
    ```
    
- **Indexer sur une clé** (prénoms → nationalités) :
    ```json
    {
      "views": {
        "players": {
          "map": "function(doc) {if (doc.type=='player') {emit(doc.firstname, doc.nationality); }}"
        }
      }
    }
    ```

---

#### **Intérêt des B+Trees**

- Indexation rapide et navigation optimisée.
- Utilisés pour stocker et organiser les vues permanentes.

---

#### **Principes Map/Reduce**

- **Map** : Applique une fonction, génère des paires clé/valeur.
- **Reduce** : Agrège les résultats générés par Map.

---

#### **Applicatif Web Fauxton**

- Interface web intégrée pour gérer CouchDB.
- Permet de visualiser, créer et exécuter des vues.

### **Synthèse : Concepts avancés de CouchDB**

---

#### **1. Différents

 niveaux d’agrégats**

- **Total global** :  
    `curl -X GET admin:pwd@localhost:5984/tennis/_design/tournois/_view/testAgregat`  
    Résultat :
    ```json
    {"rows":[{"key":null,"value":6}]}
    ```
    
- **Par genre (premier attribut)** :  
    `curl -X GET admin:pwd@localhost:5984/tennis/_design/tournois/_view/testAgregat?group_level=1`  
    Résultat :
    ```json
    {"rows":[   {"key":["F"],"value":2},   {"key":["M"],"value":4} ]}
    ```
    
- **Par genre et nationalité (deux premiers attributs)** :  
    `curl -X GET admin:pwd@localhost:5984/tennis/_design/tournois/_view/testAgregat?group_level=2`  
    Résultat :
    ```json
    {"rows":[   {"key":["F","France"],"value":2},   {"key":["M","France"],"value":3},   {"key":["M","Serbia"],"value":1} ]}
    ```

---

#### **2. Fonction Reduce personnalisée**

**Objectif** : Créer une liste des prénoms des joueurs par nationalité.

- **Map** :
    ```javascript
    function(doc) {   if (doc.type == 'player') emit(doc.nationality, doc.firstname); }
    ```
    
- **Reduce** :
    ```javascript
    function(keys, values) {   return values.reduce((firstnames, v) => firstnames.concat(v), []); }
    ```
    
- **Résultat** :
    ```json
    {"rows":[   {"key":"France","value":["Caroline","Jo","Gael"]},   {"key":"Great Britain","value":["Andy"]},   {"key":"Serbia","value":["Novak"]} ]}
    ```

---

#### **3. Organisation des données sans schéma**

- **Bonnes pratiques** :
    
    - Utiliser `_id` comme identifiant unique (UUID ou identifiant naturel).
    - Ajouter un champ `type` pour différencier les entités (ex. `player`, `country`).
    - Inclure des timestamps pour les suivis (création/mise à jour).
- **Dénormalisation** :  
    Regrouper toutes les informations dans un seul document, adapté aux données rarement mises à jour.
    
- **Référence** :  
    Structurer les données complexes en reliant les documents :
    ```json
    {   "_id": "murray",   "firstname": "Andy",   "type": "player",   "nationality": "Great Britain" },
    {   "_id": "Great Britain",   "type": "country",   "population": 60000000 }
    ```

---

#### **4. Vue combinée (Collation View)**

**Objectif** : Simuler une jointure.

- **Map** :
    ```javascript
    function(doc) {   if (doc.type == "country") {     emit([doc._id, 0], {"pop": doc.population});   } else if (doc.type == "player") {     emit([doc.nationality, 1], {"name": doc.firstname, "rank": doc.ranking});   } }
    ```
    
- **Résultats** :
    ```json
    {"rows":[   {"key":["France",0],"value":{"pop":50000000}},   {"key":["France",1],"value":{"name":"Jo","rank":12}},   {"key":["Great Britain",0],"value":{"pop":60000000}},   {"key":["Great Britain",1],"value":{"name":"Andy","rank":80}} ]}
    ```

---

#### **5. Partitionnement et réplication**

- **Partitionnement** :  
    Division des données en **shards** (partitions horizontales), distribuées sur différents nœuds.
    
    - Stratégies :
        - Hachage cohérent.
        - Partitionnement par plage.
    - Exemple : 24 shards répartis sur 3 nœuds (8 shards/nœud).
- **Réplication** :  
    Réplication des shards pour éviter un SPOF (Single Point of Failure).
    
    - **Paramètres clés** :
        - `N` : Nombre de copies (ex. 3).
        - `R` : Quorum pour lecture.
        - `W` : Quorum pour écriture.

---

#### **6. Système distribué et principes CAP**

- **CAP Theorem** :
    - **C** : Cohérence.
    - **A** : Disponibilité.
    - **P** : Tolérance aux partitions.
- **CouchDB** :
    - Priorise **A** et **P**, avec une cohérence éventuelle.
    - Scalabilité horizontale : Ajout de nœuds simple et transparent.

---

#### **7. Commandes pour gestion distribuée**

- **Lister les nœuds du cluster** :  
    `curl -s $COUCH3/_membership | jq`
- **Lister les partitions** :  
    `curl -s $COUCH3/tennis/_shards | jq`
- **Infos sur un document (ex. `caroGa_93`)** :  
    `curl -s $COUCH3/tennis/_shards/caroGa_93 | jq`

---

### **Résumé**

1. **Agrégats flexibles** : Par niveaux ou globalement via `group_level`.
2. **Personnalisation Reduce** : Manipuler et agréger des données complexes.
3. **Modèle sans schéma** : Dénormalisation ou référence selon le cas d’usage.
4. **Jointures simulées** : Collation Views pour regrouper données liées.
5. **Architecture distribuée** : Partitionnement et réplication pour fiabilité et scalabilité.
6. **CAP dans CouchDB** : Axé sur disponibilité et tolérance aux pannes.