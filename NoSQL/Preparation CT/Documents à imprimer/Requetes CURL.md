---
tags:
  - nosql
---

## Table des Matières

1. [Afficher le Banner du Serveur](#1-afficher-le-banner-du-serveur)
2. [Créer une Base de Données](#2-créer-une-base-de-données)
3. [Obtenir les Détails d'une Base](#3-obtenir-les-détails-dune-base)
4. [Lister Toutes les Bases de Données](#4-lister-toutes-les-bases-de-données)
5. [Ajouter un Document avec ID Explicite](#5-ajouter-un-document-avec-id-explicite)
6. [Récupérer un Document](#6-récupérer-un-document)
7. [Mettre à Jour un Document (Échec)](#7-mettre-à-jour-un-document-échoué)
8. [Mettre à Jour un Document avec Révision](#8-mettre-à-jour-un-document-avec-révision)
9. [Obtenir un UUID](#9-obtenir-un-uuid)
10. [Lister les Documents avec Métadonnées](#10-lister-les-documents-avec-métadonnées)
11. [Récupérer un Document avec Révisions](#11-récupérer-un-document-avec-révisions)
12. [Ajouter un Document via PUT](#12-ajouter-un-document-via-put)
13. [Ajouter un Document via POST](#13-ajouter-un-document-via-post)
14. [Ajouter des Documents par Lots (Échec)](#14-ajouter-des-documents-par-lots-échoué)
15. [Ajouter des Documents par Lots Correctement](#15-ajouter-des-documents-par-lots-correctement)
16. [Supprimer un Document](#16-supprimer-un-document)
17. [Supprimer une Base de Données](#17-supprimer-une-base-de-données)

---
Ces requêtes **cURL** illustrent comment interagir avec Apache CouchDB pour gérer les bases de données, ajouter, récupérer, mettre à jour et supprimer des documents, ainsi que pour gérer la réplication et le partitionnement. Maîtriser ces commandes est essentiel pour administrer efficacement votre environnement CouchDB.

## 1. Afficher le Banner du Serveur

```bash
curl -X GET localhost:5984
```

**Explication :**  
Cette requête GET envoie une demande au serveur CouchDB pour obtenir le banner de bienvenue, incluant la version du serveur et des informations sur le fournisseur.

**Résultat Illustré :**
```json
{
  "couchdb": "Welcome",
  "version": "3.3.3",
  "vendor": {
    "name": "The Apache Software Foundation"
  }
}
```

---

## 2. Créer une Base de Données

```bash
curl -X PUT admin:pwd@localhost:5984/db_test
```

**Explication :**  
Cette requête PUT crée une nouvelle base de données nommée `db_test`. L'authentification est requise (ici, `admin` avec le mot de passe `pwd`).

**Résultat Illustré :**
```json
{"ok":true}
```

---

## 3. Obtenir les Détails d'une Base

```bash
curl -X GET admin:pwd@localhost:5984/db_test
```

**Explication :**  
Cette requête GET récupère les détails de la base de données `db_test`, tels que le nom de la base, le temps de démarrage de l'instance, et d'autres statistiques.

**Résultat Illustré :**
```json
{
  "instance_start_time": "1727864848",
  "db_name": "db_test",
  "purge_seq": 0,
  ...
}
```

---

## 4. Lister Toutes les Bases de Données

```bash
curl -X GET admin:pwd@localhost:5984/_all_dbs
```

**Explication :**  
Cette requête GET liste toutes les bases de données présentes sur le serveur CouchDB.

**Résultat Illustré :**
```json
["db_test","exam","tennis",...]
```

---

## 5. Ajouter un Document avec ID Explicite

```bash
curl -X PUT admin:pwd@localhost:5984/db_test/o1 -d '{"name":"Nadal"}'
```

**Explication :**  
Cette requête PUT ajoute un document à la base `db_test` avec un identifiant explicite `o1`. Le contenu du document est fourni dans le corps de la requête.

**Résultat Illustré :**
```json
{
  "ok": true,
  "id": "o1",
  "rev": "1-f28a4a5baf607e908cea5863c324d147"
}
```

---

## 6. Récupérer un Document

```bash
curl -X GET admin:pwd@localhost:5984/db_test/o1
```

**Explication :**  
Cette requête GET récupère le document avec l'identifiant `o1` de la base `db_test`.

**Résultat Illustré :**
```json
{
  "_id": "o1",
  "_rev": "1-f28a4a5baf607e908cea5863c324d147",
  "name": "Nadal"
}
```

---

## 7. Mettre à Jour un Document (Échec)

```bash
curl -X PUT admin:pwd@localhost:5984/db_test/o1 -d '{"name":"Nadal", "age":36}'
```

**Explication :**  
Cette requête PUT tente de mettre à jour le document `o1` sans fournir la révision actuelle (`_rev`). Cela entraîne une erreur de conflit car CouchDB utilise le contrôle de concurrence multi-version (MVCC).

**Résultat Illustré :**
```json
{
  "error": "conflict",
  "reason": "Document update conflict."
}
```

---

## 8. Mettre à Jour un Document avec Révision

```bash
curl -X PUT admin:pwd@localhost:5984/db_test/o1 -d '{"_rev":"1-f28a4a5baf607e908cea5863c324d147","name":"Nadal", "age":36}'
```

**Explication :**  
Cette requête PUT met à jour le document `o1` en fournissant la révision actuelle (`_rev`). Cela permet à CouchDB de valider la mise à jour et d'éviter les conflits.

**Résultat Illustré :**
```json
{
  "ok": true,
  "id": "o1",
  "rev": "2-b3a6b46d95c7671046d98826c2010595"
}
```

---

## 9. Obtenir un UUID

```bash
curl -X GET admin:pwd@localhost:5984/_uuids
```

**Explication :**  
Cette requête GET récupère un ou plusieurs UUIDs universellement uniques, utilisés pour identifier de manière unique les documents.

**Résultat Illustré :**
```json
{
  "uuids": ["415da9c4f3bd245ea2fbbeaab600057c"]
}
```

---

## 10. Lister les Documents avec Métadonnées

```bash
curl admin:pwd@localhost:5984/db_test/_all_docs
```

**Explication :**  
Cette requête GET liste tous les documents de la base `db_test` avec leurs identifiants et leurs révisions.

**Résultat Illustré :**
```json
{
  "total_rows": 1,
  "offset": 0,
  "rows": [
    {
      "id": "o1",
      "key": "o1",
      "value": { "rev": "2-b3a6b46d95c7671046d98826c2010595" }
    }
  ]
}
```

---

## 11. Récupérer un Document avec Révisions

```bash
curl -X GET admin:pwd@localhost:5984/db_test/o1?revs=true
```

**Explication :**  
Cette requête GET récupère le document `o1` de `db_test` avec les informations sur toutes ses révisions.

**Résultat Illustré :**
```json
{
  "_id": "o1",
  "_rev": "2-b3a6b46d95c7671046d98826c2010595",
  "name": "Nadal",
  "age": 36,
  "_revisions": {
    "start": 2,
    "ids": ["b3a6b46d95c7671046d98826c2010595", "f28a4a5baf607e908cea5863c324d147"]
  }
}
```

---

## 12. Ajouter un Document via PUT

```bash
curl -X PUT admin:pwd@localhost:5984/db_test/o5 -d @tsonga.json -H "Content-Type: application/json"
```

**Explication :**  
Cette requête PUT ajoute un document à la base `db_test` avec l'identifiant explicite `o5`. Le contenu du document est chargé à partir du fichier `tsonga.json` et le type de contenu est spécifié comme JSON.

**Résultat Illustré :**
```json
{
  "ok": true,
  "id": "o5",
  "rev": "1-87f50d9c01b37db251d7e12a8ad0ce69"
}
```

---

## 13. Ajouter un Document via POST

```bash
curl -X POST admin:pwd@localhost:5984/db_test -d @monfils.json -H "Content-Type: application/json"
```

**Explication :**  
Cette requête POST ajoute un document à la base `db_test` en utilisant l'identifiant spécifié dans le contenu du document lui-même. Le fichier `monfils.json` est envoyé avec le type de contenu JSON.

**Résultat Illustré :**
```json
{
  "ok": true,
  "id": "laMonf",
  "rev": "1-341422aac64aa47ff3a933bb9d62c5b1"
}
```

---

## 14. Ajouter des Documents par Lots (Échec)

```bash
curl -d @murrayDjoko.json -X POST admin:pwd@localhost:5984/db_test/_bulk_docs
```

**Explication :**  
Cette requête POST tente d'ajouter plusieurs documents à la base `db_test` en une seule opération via l'API `_bulk_docs`. Cependant, elle échoue car le type de contenu (`Content-Type`) n'est pas spécifié.

**Résultat Illustré :**
```json
{
  "error": "bad_request",
  "reason": "Content-Type is required"
}
```

---

## 15. Ajouter des Documents par Lots Correctement

```bash
curl -X POST admin:pwd@localhost:5984/db_test/_bulk_docs -d @murrayDjoko.json -H "Content-Type: application/json"
```

**Explication :**  
Cette requête POST ajoute correctement plusieurs documents à la base `db_test` en spécifiant le type de contenu JSON. Le fichier `murrayDjoko.json` contient une collection de documents.

**Résultat Illustré :**
```json
[
  {
    "ok": true,
    "id": "murray",
    "rev": "1-7d4b633c14d3b66fd2c333947627f7ef"
  },
  {
    "ok": true,
    "id": "djoko",
    "rev": "1-0620108d9d04bbaa9b55c826664a244d"
  }
]
```

---

## 16. Supprimer un Document

```bash
curl -X DELETE admin:pwd@localhost:5984/db_test/o1?rev=2-8e0031775b443f73681b8c2566895f8b
```

**Explication :**  
Cette requête DELETE supprime le document `o1` de la base `db_test` en spécifiant la révision actuelle (`_rev`). CouchDB effectue une suppression logique en créant une nouvelle révision marquée comme supprimée.

**Résultat Illustré :**
```json
{
  "ok": true,
  "id": "o1",
  "rev": "3-c3e4361ab165df874c0ee1d92966d8de"
}
```

---

## 17. Supprimer une Base de Données

```bash
curl -X DELETE admin:pwd@localhost:5984/other_db
```

**Explication :**  
Cette requête DELETE supprime complètement la base de données nommée `other_db` du serveur CouchDB.

**Résultat Illustré :**
```json
{
  "ok": true
}
```

---


