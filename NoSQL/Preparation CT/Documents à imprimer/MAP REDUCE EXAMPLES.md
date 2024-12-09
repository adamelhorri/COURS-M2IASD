---
tags:
  - nosql
---
# Fonctions Map et Map-Reduce dans CouchDB

Ce document présente une compilation complète des fonctions **Map** et **Map-Reduce** utilisées dans CouchDB, telles qu'extraites de votre cours. Chaque fonction est numérotée, accompagnée de son code et d'une explication succincte, illustrée par des exemples concrets basés sur les documents fournis.

---

## Table des Matières

1. [Liste Tous Noms Étudiants](#1-liste-tous-noms-étudiants)
2. [Liste Documents avec Révisions](#2-liste-documents-avec-révisions)
3. [Indexation Joueurs par Prénom](#3-indexation-joueurs-par-prénom)
4. [Compte Tournois par Ville Année](#4-compte-tournois-par-ville-année)
5. [Compte Joueurs par Ville](#5-compte-joueurs-par-ville)
6. [Compte Joueurs par Nationalité](#6-compte-joueurs-par-nationalité)
7. [Compte Joueurs par Genre Nationalité](#7-compte-joueurs-par-genre-nationalité)
8. [Liste Prénoms par Nationalité](#8-liste-prénoms-par-nationalité)
9. [Compte Salles par Bâtiment Type](#9-compte-salles-par-bâtiment-type)
10. [Compte Formations Féminines](#10-compte-formations-féminines)
11. [Agrégation Formations par Critère](#11-agrégation-formations-par-critère)
12. [Max Distance Ligne 1](#12-max-distance-ligne-1)
13. [Index Régions Occitanie](#13-index-régions-occitanie)
14. [Index Destinations par Station](#14-index-destinations-par-station)
15. [Agrégation Salles par Bâtiment](#15-agrégation-salles-par-bâtiment)
16. [Vue Collation Concaténée](#16-vue-collation-concaténée)
17. [Liste Toutes Nationalités](#17-liste-toutes-nationalités)

---

## 1. Liste Tous Noms Étudiants

```javascript
{
  "views": {
    "all": {
      "map": "function(doc) {emit(null, doc.name) }"
    }
  }
}
```

**Explication :**  
Cette fonction Map émet le nom de chaque document sans clé spécifique. Elle permet de récupérer une liste de tous les noms présents dans la base de données.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "etudiant1",
  "name": "Jean"
}
```
La vue émettra :
```
Key: null, Value: "Jean"
```

---

## 2. Liste Documents avec Révisions

```javascript
{
  "views": {
    "allDocs": {
      "map": "function(doc) {emit(doc._id, {\"rev\" : doc._rev}); }"
    }
  }
}
```

**Explication :**  
Cette fonction Map associe chaque identifiant de document (`_id`) à sa révision (`_rev`). Elle facilite le suivi et la gestion des versions des documents dans la base de données.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "etudiant1",
  "_rev": "1-abc123",
  "name": "Jean"
}
```
La vue émettra :
```
Key: "etudiant1", Value: {"rev": "1-abc123"}
```

---

## 3. Indexation Joueurs par Prénom

```javascript
{
  "views": {
    "players": {
      "map": "function(doc) {if (doc.type=='player') {emit(doc.firstname, doc.nationality);}}"
    }
  }
}
```

**Explication :**  
Cette fonction Map filtre les documents de type 'player' et émet le prénom comme clé et la nationalité comme valeur. Elle permet de créer un index des joueurs par prénom.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "player1",
  "type": "player",
  "firstname": "Andy",
  "nationality": "Great Britain"
}
```
La vue émettra :
```
Key: "Andy", Value: "Great Britain"
```

---

## 4. Compte Tournois par Ville Année

### MAP

```javascript
function (doc) { 
  if (doc.tournaments) { 
    for (var i in doc.tournaments)
      emit([doc.tournaments[i].city, doc.tournaments[i].year], 1); 
  }
}
```

### REDUCE

```javascript
_count
```

**Explication :**  
La fonction Map émet une clé composite composée de la ville et de l'année pour chaque tournoi. La fonction Reduce `_count` compte le nombre d'occurrences par ville et année.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "tournament1",
  "tournaments": [
    {"city": "Marseille", "year": 2017},
    {"city": "Paris", "year": 2018}
  ]
}
```
La vue émettra :
```
Key: ["Marseille", 2017], Value: 1
Key: ["Paris", 2018], Value: 1
```

Après réduction :
```
Key: ["Marseille", 2017], Value: 1
Key: ["Paris", 2018], Value: 1
```

---

## 5. Compte Joueurs par Ville

### MAP

```javascript
function (doc) { 
  if (doc.tournaments) { 
    for (var i in doc.tournaments)
      emit(doc.tournaments[i].city, 1); 
  }
}
```

### REDUCE

```javascript
_count
```

**Explication :**  
La fonction Map émet chaque ville de tournoi avec une valeur de 1. La fonction Reduce `_count` totalise le nombre de joueurs par ville.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "player1",
  "tournaments": [
    {"city": "Marseille", "year": 2017},
    {"city": "Paris", "year": 2018}
  ]
}
```
La vue émettra :
```
Key: "Marseille", Value: 1
Key: "Paris", Value: 1
```

Après réduction :
```
Key: "Marseille", Value: 1
Key: "Paris", Value: 1
```

---

## 6. Compte Joueurs par Nationalité

### MAP

```javascript
function (doc) {
  if (doc.type == 'player') {
    emit(doc._id, {"nationality": doc.nationality});
  }
}
```

### REDUCE

```javascript
_count
```

**Explication :**  
La fonction Map émet l'identifiant du joueur et sa nationalité. La fonction Reduce `_count` permet de compter le nombre de joueurs par nationalité.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "player1",
  "type": "player",
  "nationality": "France"
}
```
La vue émettra :
```
Key: "player1", Value: {"nationality": "France"}
```

Après réduction :
```
Key: "France", Value: 1
```

---

## 7. Compte Joueurs par Genre Nationalité

### MAP

```javascript
function (doc) { 
  if (doc.type == 'player') { 
    emit([doc.gender, doc.nationality, doc._id], 1);
  }
}
```

### REDUCE

```javascript
_count
```

**Explication :**  
Cette fonction Map émet une clé composite composée du genre, de la nationalité et de l'identifiant du joueur. La fonction Reduce `_count` permet de compter les joueurs par genre et nationalité.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "player2",
  "type": "player",
  "gender": "F",
  "nationality": "France"
}
```
La vue émettra :
```
Key: ["F", "France", "player2"], Value: 1
```

Après réduction :
```
Key: ["F", "France"], Value: 2
```

---

## 8. Liste Prénoms par Nationalité

### MAP

```javascript
function(doc) { 
  if (doc.type == 'player') {
    emit(doc.nationality, doc.firstname);
  }
}
```

### REDUCE

```javascript
function(keys, values) {
  var firstnames = [];
  values.forEach(function(v) {
    firstnames = firstnames.concat(v); 
  });
  return firstnames; 
}
```

**Explication :**  
La fonction Map associe la nationalité des joueurs à leurs prénoms. La fonction Reduce personnalisée agrège les prénoms par nationalité, retournant un tableau des prénoms pour chaque pays.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "player3",
  "type": "player",
  "firstname": "Caroline",
  "nationality": "France"
}
```
La vue émettra :
```
Key: "France", Value: "Caroline"
```

Après réduction :
```
Key: "France", Value: ["Caroline", "Jo", "Gael", "Kristina"]
```

---

## 9. Compte Salles par Bâtiment Type

### MAP

```javascript
function (doc) { 
  if (doc.type == 'batiment' && doc.destination == 'enseignement') {
    if (Array.isArray(doc.salles)) {
      for (var s in doc.salles)
        emit([doc.ode, doc.salles[s].type, doc.salles[s].apaite], 1);
    }
  }
}
```

**Explication :**  
Cette fonction Map filtre les documents de type `batiment` destinés à l'enseignement et parcourt le tableau des salles (`salles`). Elle émet une clé composite composée de l'identifiant du bâtiment (`ode`), du type de salle (`type`) et de l'appartenance (`apaite`), avec une valeur de `1`. Cela permet de compter les salles par critère.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "batiment2",
  "type": "batiment",
  "destination": "enseignement",
  "ode": "B102",
  "salles": [
    {"type": "salle de classe", "apaite": "A1"},
    {"type": "laboratoire", "apaite": "L1"}
  ]
}
```
La vue émettra :
```
Key: ["B102", "salle de classe", "A1"], Value: 1
Key: ["B102", "laboratoire", "L1"], Value: 1
```

---

## 10. Compte Formations Féminines

```javascript
function (doc) { 
  if (doc.type == 'etudiant' && doc.genre == 'femme') {
    if (Array.isArray(doc.formations)) {
      for (var f in doc.formations)
        emit([doc.formations[f].niveau, doc.formations[f].codeF, doc.formations[f].annee], 1);
    }
  }
}
```

**Explication :**  
Cette fonction Map filtre les documents de type `etudiant` et de genre `femme`. Elle parcourt le tableau des formations (`formations`) de chaque étudiante et émet une clé composite composée du niveau (`niveau`), du code de formation (`codeF`) et de l'année (`annee`) de chaque formation, avec une valeur de `1`. Cela permet de compter les formations par critère.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "etudiant2",
  "type": "etudiant",
  "genre": "femme",
  "prenom": "Marie",
  "age": 21,
  "formations": [
    {"niveau": "Licence", "codeF": "INF101", "annee": 2021},
    {"niveau": "Master", "codeF": "INF201", "annee": 2022}
  ]
}
```
La vue émettra :
```
Key: ["Licence", "INF101", 2021], Value: 1
Key: ["Master", "INF201", 2022], Value: 1
```

---

## 11. Agrégation Formations par Critère

### MAP

```javascript
function (doc) { 
  if (doc.type == 'etudiant' && doc.genre == 'femme') {
    if (Array.isArray(doc.formations)) {
      for (var f in doc.formations)
        emit([doc.formations[f].niveau, doc.formations[f].codeF, doc.formations[f].annee], 1);
    }
  }
}
```

### REDUCE

```javascript
_sum
```

**Explication :**  
Cette vue Map/Reduce utilise la fonction Map précédente et la fonction Reduce intégrée `_sum` pour totaliser le nombre de formations par niveau, code de formation et année. Elle permet d'obtenir des agrégats sur les formations des étudiantes.

**Résultat Illustré :**  
Avec les mêmes documents que précédemment, la réduction `_sum` comptera le nombre de formations pour chaque combinaison de niveau, code de formation et année :
```
Key: ["Licence", "INF101", 2021], Value: 1
Key: ["Master", "INF201", 2022], Value: 1
```
Si plusieurs étudiantes ont les mêmes formations, les valeurs seront additionnées.

---

## 12. Max Distance Ligne 1

### MAP

```javascript
function(doc) {
    if (doc.type === 'station') {
        for (var idx in doc.correspondances) {
            var correspondance = doc.correspondances[idx];
            if (correspondance.numLigne === 1) {
                emit(doc._id, correspondance.distance);
            }
        }
    }
}
```

### REDUCE

```javascript
function(keys, values, rereduce) {
    return Math.max.apply(null, values);
}
```

**Explication :**  
La fonction Map filtre les documents de type `station` et parcourt leurs correspondances. Elle émet l'identifiant de la station (`_id`) et la distance associée uniquement si `numLigne === 1`. La fonction Reduce personnalisée calcule la distance maximale à partir des valeurs émises. Cela permet de déterminer la distance la plus longue des correspondances de la ligne 1 pour chaque station.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "station1",
  "type": "station",
  "nom": "Gare du Nord",
  "correspondances": [
    {"destination": "Paris", "numLigne": 1, "distance": 500},
    {"destination": "Lyon", "numLigne": 2, "distance": 300}
  ]
}
```
La vue émettra :
```
Key: "station1", Value: 500
```
Après réduction :
```
Key: "station1", Value: 500
```
Si plusieurs correspondances de `numLigne === 1` existent pour une même station, la fonction Reduce retournera la distance maximale parmi elles.

---

## 13. Index Régions Occitanie

```javascript
function(doc) {
    if (doc.nom === 'Occitanie') {
        emit([doc._id, doc.latitude, doc.longitude], doc.correspondances);
    }
}
```

**Explication :**  
Cette fonction Map filtre les documents où le champ `nom` est "Occitanie". Elle émet une clé composite composée de l'identifiant du document (`_id`), de la latitude (`latitude`) et de la longitude (`longitude`), associée aux correspondances (`correspondances`). Cela permet d'indexer les documents spécifiques à "Occitanie" pour des requêtes géographiques.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "region1",
  "nom": "Occitanie",
  "latitude": 43.6045,
  "longitude": 1.4442,
  "correspondances": ["Correspondance1", "Correspondance2"]
}
```
La vue émettra :
```
Key: ["region1", 43.6045, 1.4442], Value: ["Correspondance1", "Correspondance2"]
```

---

## 14. Index Destinations par Station

```javascript
function(doc) {
    if (doc.type === 'station') {
        for (var idx in doc.correspondances) {
            var correspondance = doc.correspondances[idx];
            emit(doc._id, correspondance.destination);
        }
    }
}
```

**Explication :**  
Cette fonction Map filtre les documents de type `station`. Pour chaque correspondance dans le document, elle émet l'identifiant de la station (`_id`) comme clé et la destination de la correspondance (`destination`) comme valeur. Cela permet de créer un index des destinations associées à chaque station.

**Résultat Illustré :**  
Pour un document :
```json
{
  "_id": "station1",
  "type": "station",
  "nom": "Gare du Nord",
  "correspondances": [
    {"destination": "Paris", "numLigne": 1, "distance": 500},
    {"destination": "Lyon", "numLigne": 2, "distance": 300}
  ]
}
```
La vue émettra :
```
Key: "station1", Value: "Paris"
Key: "station1", Value: "Lyon"
```

---

## 15. Agrégation Salles par Bâtiment

### MAP

```javascript
function (doc) { 
  if (doc.type == 'batiment' && doc.destination == 'enseignement') {
    if (Array.isArray(doc.salles)) {
      for (var s in doc.salles)
        emit([doc.ode, doc.salles[s].type, doc.salles[s].apaite], 1);
    }
  }
}
```

### REDUCE

```javascript
function(keys, values, rereduce) {
  return sum(values);
}
```

**Explication :**  
Cette vue Map/Reduce utilise la fonction Map précédente et une fonction Reduce personnalisée qui somme les valeurs. Elle permet de totaliser le nombre de salles par bâtiment, type et appartenance, facilitant l'analyse des infrastructures éducatives.

**Résultat Illustré :**  
Avec le même document que précédemment :
```json
{
  "_id": "batiment2",
  "type": "batiment",
  "destination": "enseignement",
  "ode": "B102",
  "salles": [
    {"type": "salle de classe", "apaite": "A1"},
    {"type": "laboratoire", "apaite": "L1"}
  ]
}
```
La vue émettra :
```
Key: ["B102", "salle de classe", "A1"], Value: 1
Key: ["B102", "laboratoire", "L1"], Value: 1
```
Après réduction :
```
Key: ["B102", "salle de classe", "A1"], Value: 1
Key: ["B102", "laboratoire", "L1"], Value: 1
```

---

## 16. Vue Collation Concaténée

```javascript
function(doc) {
  if (doc.type == "country") {
    emit([doc._id, 0], [{"pop": doc.population}, {"players": doc.players}]);
  } else if (doc.type == "player") {
    emit([doc.nationality, 1], [{"name": doc.firstname}, {"rank": doc.ranking}]);
  }
}
```

**Explication :**  
Cette fonction Map émet des données des pays et des joueurs avec une clé composite pour permettre une vue consolidée. Elle facilite l'agrégation des informations nationales et des joueurs associés.

**Résultat Illustré :**  
Pour des documents :
```json
{
  "_id": "France",
  "type": "country",
  "population": 50000000,
  "players": "60000"
},
{
  "_id": "player1",
  "type": "player",
  "firstname": "Jo",
  "ranking": 12,
  "nationality": "France"
}
```
La vue émettra :
```
Key: ["France", 0], Value: [{"pop":50000000}, {"players":"60000"}]
Key: ["France", 1], Value: [{"name":"Jo"}, {"rank":12}]
```

Après réduction :
```
Key: ["France", 0], Value: [{"pop":50000000}, {"players":"60000"}]
Key: ["France", 1], Value: [{"name":"Jo"}, {"rank":12}]
```

---

## 17. Liste Toutes Nationalités

```javascript
function (doc) {
  if (doc.type == 'player') {
    emit(doc.nationality, doc.firstname);
  }
}
```

### REDUCE

```javascript
function(keys, values) {
  var firstnames = [];
  values.forEach(function(v) {
    firstnames = firstnames.concat(v); 
  });
  return firstnames; 
}
```

**Explication :**  
La fonction Map associe la nationalité des joueurs à leurs prénoms. La fonction Reduce personnalisée agrège les prénoms par nationalité, retournant un tableau des prénoms pour chaque pays.

**Résultat Illustré :**  
Pour des documents :
```json
{
  "_id": "player3",
  "type": "player",
  "firstname": "Caroline",
  "nationality": "France"
},
{
  "_id": "player4",
  "type": "player",
  "firstname": "Novak",
  "nationality": "Serbia"
}
```
La vue émettra :
```
Key: "France", Value: "Caroline"
Key: "Serbia", Value: "Novak"
```
Après réduction :
```
Key: "France", Value: ["Caroline", "Jo", "Gael", "Kristina"]
Key: "Serbia", Value: ["Novak"]
```
