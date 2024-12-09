---
tags:
  - nosql
---
# Sommaire

1. [1. Cours Neo4J Théorique](#1-cours-neo4j-théorique)
   1.1. [1.1 Comparaison entre `CREATE INDEX` et `CREATE CONSTRAINT`](#11-comparaison-entre-create-index-et-create-constraint)
   1.2. [1.2 APOC : Awesome Procedures on Cypher](#12-apoc-awesome-procedures-on-cypher)
   1.3. [1.3 Neo4j et Neosemantics : Une Introduction Simplifiée](#13-neo4j-et-neosemantics--une-introduction-simplifiée)
      1.3.1. [1.3.1 RDF vs LPG : Quelle différence ?](#133-rdf-vs-lpg--quelle-différence-)
      1.3.2. [1.3.2 Ce que Neosemantics permet de faire](#1332-ce-que-neosemantics-permet-de-faire)
      1.3.3. [1.3.3 Exemples simples](#1333-exemples-simples)
      1.3.4. [1.3.4 Conclusion](#1334-conclusion)

---

## 1. Cours Neo4J Théorique

### 1.1. Comparaison entre `CREATE INDEX` et `CREATE CONSTRAINT`

| **Aspect**                    | **CREATE INDEX**                          | **CREATE CONSTRAINT**                        |
|-------------------------------|-------------------------------------------|----------------------------------------------|
| **Recherche**                 | Basée sur un index simple.                | Utilise un index unique.                     |
| **Garantie d'unicité**        | Aucune garantie d'unicité.                | Garantit l'unicité des données.              |
| **Performance**               | Améliore les recherches sur une propriété.| Optimise les recherches et garantit l'intégrité. |
| **Utilisation typique**       | Recherches rapides sur des propriétés non uniques.| Scénarios où l'unicité des valeurs est requise.|

### 1.2. APOC : Awesome Procedures on Cypher

- **Qu'est-ce que c'est ?**  
  Une bibliothèque de procédures pour enrichir Cypher, implémentée en Java et utilisable via des fichiers `.jar` placés dans le répertoire `plugins`.  
  APOC facilite les tâches avancées, optimise les performances, et intègre facilement Neo4j à d'autres systèmes.

- **Principales fonctionnalités**
  1. **Gestion du schéma** :  
    
     ```cypher
     CALL db.schema();
     CALL db.constraints();
     CALL db.labels();
     CALL db.indexes();
     ```
  
  2. **Export de données** :  
     - Commande :  
       ```cypher
       CALL apoc.export.json.query(
         "MATCH (a) RETURN id(a), labels(a), a.name",
         "test.json",
         {}
       );
       ```
     - Résultat (JSON) :  
       ```json
       {"id(a)":1,"labels(a)":["Personne"],"a.name":"bob"}
       {"id(a)":20,"labels(a)":["Institution"],"a.name":"UM"}
       {"id(a)":82,"labels(a)":["City"],"a.name":"Montpellier"}
       ```
  
  3. **Autres fonctionnalités** :  
     - Traversées de graphes.  
     - Recherche plein texte.  
     - Fonctions spatiales.  
     - Migration entre SGBD.  
     - Conversion de formats (JSON, CSV).

### 1.3. Neo4j et Neosemantics : Une Introduction Simplifiée

**Neosemantics** est une extension de Neo4j qui permet de travailler avec des données au format **RDF** (Resource Description Framework). RDF est utilisé pour représenter des informations sous forme de triplets : **sujet, prédicat, objet**. Neosemantics facilite l'import, l'export, et le mapping de ces données vers le modèle de graphe **LPG** (Labeled Property Graph) utilisé par Neo4j.

---

#### 1.3.1. RDF vs LPG : Quelle différence ?

| **RDF** | **LPG (Neo4j)** |
|---------|------------------|
| Représente les données en **triplets** (ex. Ville -> a -> Montpellier). | Utilise des **nœuds**, des **relations**, et des **propriétés**. |
| Utilisé dans le **web sémantique**. | Utilisé pour des applications internes. |
| **Exemple RDF** :  
  ```rdf
  @prefix ex: <http://example.org/>.
  ex:Ville a ex:Classe; ex:nom "Montpellier".
  ```  
| **Exemple LPG** :  
  ```cypher
  CREATE (v:Ville {nom: "Montpellier"});
  ``` |

---

#### 1.3.2. Ce que Neosemantics permet de faire

1. **Importer des données RDF** :  
   Charger des données RDF dans Neo4j depuis des fichiers ou des URL.  
   **Exemple** :  
   ```cypher
   CREATE INDEX ON :Resource(uri);
   CALL semantics.importRDF("http://example.org/data.ttl", "Turtle");
   ```
   Cela crée des nœuds et relations dans Neo4j à partir des triplets RDF.

2. **Exporter des données LPG en RDF** :  
   Convertir les graphes Neo4j au format RDF pour les partager ou les utiliser ailleurs.  
   **Exemple** :  
   ```
   :POST /rdf/cypher { "cypher":"MATCH (v:Ville) RETURN v", "format":"N3" }
   ```

3. **Requêter des données RDF** :  
   Utiliser Cypher pour manipuler ou interroger des données RDF importées.  
   **Exemple** :  
   ```cypher
   :POST /rdf/cypher { "cypher":"MATCH (v:Ville) RETURN v.nom", "format":"N3" }
   ```

---

#### 1.3.3. Exemples simples

1. **Créer un graphe avec des maires et une ville** (LPG) :  
   ```cypher
   CREATE (v:Ville {nom:"Montpellier"})
   <-[r:administree_par {date_debut:1997, date_fin:2004}]-
   (p:Personne {nom:"Georges Freche"});
   ```
   Cela représente la ville Montpellier administrée par Georges Freche entre 1997 et 2004.

2. **Exporter cette relation en RDF** :  
   ```rdf
   @prefix ex: <http://example.org/>.
   ex:Montpellier a ex:Ville;
     ex:administree_par [ ex:nom "Georges Freche"; ex:date_debut "1997"; ex:date_fin "2004" ].
   ```

3. **Importer des données RDF sur Shakespeare** :  
   Charger un fichier contenant des œuvres de Shakespeare :  
   ```cypher
   CREATE INDEX ON :Resource(uri);
   CALL semantics.importRDF("http://example.org/Shakespeare.ttl", "Turtle");
   ```

4. **Problème de perte de données** :  
   Lors du mapping RDF vers LPG, certaines propriétés des relations (ex. dates) peuvent être perdues.  
   **Exemple** :  
   - RDF d'origine :  
     ```rdf
     ex:Montpellier ex:administree_par [ ex:date_debut "1997"; ex:date_fin "2004" ].
     ```
   - En Neo4j, les propriétés `date_debut` et `date_fin` peuvent ne pas être incluses par défaut.

---

#### 1.3.4. Conclusion

- **Neosemantics simplifie l'intégration RDF↔Neo4j** :  
  - Importer et exporter des données RDF.  
  - Mapper les triplets RDF vers des graphes LPG.  
  
- **Limites** :  
  - Certaines propriétés peuvent être perdues (ex. attributs sur les relations).  
  
- **Applications pratiques** :  
  - Intégrer des données externes (ex. open data).  
  - Exporter des graphes pour des usages interopérables (web sémantique).