---
tags:
  - nosql
  - annales
---
cours entier: [[Cours 1 Neo4J]]
### **1. Concepts Clés NoSQL**

- **NoSQL = Not Only SQL** (base de données non relationnelle).
- **Flexible** : Adapté aux changements fréquents de schéma.
- **Scalable** : Scalabilité horizontale (ajouter des serveurs).
- **Types** :
    - Clé/valeur (ex : Redis).
    - Colonne (ex : Cassandra).
    - Document (ex : MongoDB, CouchDB).
    - Graphe (ex : Neo4J).

---

### **2. Le Théorème CAP**

- **C**ohérence (Consistency) : Données synchronisées sur tous les nœuds.
- **A**vailability (Disponibilité) : Réponse assurée pour chaque requête.
- **P**artition tolerance : Supporte les pannes réseau.
- **Choix** : Impossible d'avoir les 3 (CAP), on choisit 2.

---

### **3. Neo4J : Système Orienté Graphe**

- **Nœuds** : Entités (ex : Ville, Personne).
- **Relations** : Arcs orientés (ex : Connexions, Proximité).
- **Propriétés** : Attributs associés aux nœuds et relations.
- **Applications** : Réseaux sociaux, systèmes géographiques, systèmes de recommandation.

---

### **4. Requêtes Cypher : Essentiel**

#### **4.1 Création**

- **Créer un nœud** :
    
    cypher
    
    Copier le code
    
    `CREATE (n:Commune {nom: 'Montpellier', codeinsee: '34172'})`
    
- **Créer une relation** :
    
    cypher
    
    Copier le code
    
    `CREATE (m:Commune {nom: 'Montpellier'})-[:NEARBY]->(n:Commune {nom: 'Lattes'})`
    

#### **4.2 Lecture**

- **Rechercher des nœuds** :
    
    cypher
    
    Copier le code
    
    `MATCH (n:Commune) WHERE n.nom = 'Montpellier' RETURN n`
    
- **Rechercher des relations** :
    
    cypher
    
    Copier le code
    
    `MATCH (m:Commune)-[:NEARBY]->(n:Commune) WHERE m.nom = 'Montpellier' RETURN m, n`
    

#### **4.3 Suppression**

- **Supprimer un nœud et ses relations** :
    
    cypher
    
    Copier le code
    
    `MATCH (n:Commune {nom: 'Montpellier'}) DETACH DELETE n`
    

---

### **5. Requêtes Cypher Avancées**

- **Chemin le plus court** :
    
    cypher
    
    Copier le code
    
    `MATCH p = shortestPath((m:Commune)-[:NEARBY*]-(n:Commune))  WHERE m.nom='Montpellier'  RETURN p`
    
- **Requête avec filtre sur propriété** :
    
    cypher
    
    Copier le code
    
    `MATCH (m:Commune)-[:NEARBY]->(n:Commune)  WHERE m.nom='Montpellier' AND n.population > 5000  RETURN m, n`
    

---

### **6. Modèle RDF et Limitations**

- **Neo4J = modèle de graphe riche** (propriétés sur relations).
- **RDF** : Modèle de triplet simple (sujet-prédicat-objet).
- **Conversion difficile** : Perte d’informations sur les propriétés des relations.

---

### **7. Stratégies de Révision**

1. **Maîtriser les bases de Cypher** :
    
    - **Créer, Rechercher, Supprimer** nœuds et relations.
    - Pratiquez sur des exemples de villes, connexions entre villes.
2. **Comprendre CAP et NoSQL** :
    
    - Révisez les compromis du théorème CAP.
    - Apprenez quand utiliser NoSQL, en fonction des besoins (scalabilité, flexibilité).
3. **Exercices pratiques avec Neo4J** :
    
    - Créez des graphes simples (communes et relations de proximité).
    - Simulez des requêtes avec Cypher (requêtes simples et avancées).
4. **Apprenez à lire les résultats en RDF** :
    
    - Comparez les résultats Cypher avec ceux en format RDF.
    - Pratiquez l'import/export de données RDF avec Neo4J.

---

### **8. Méthodes de Mémorisation**

- **Cartes mentales** : Visualisez les concepts sous forme de graphes.
- **Quiz auto-générés** : Créez des questions et répondez-y régulièrement.
- **Dessiner des graphes** : Représentez visuellement les connexions (relations).
- **Fiche récapitulative rapide** : Répétez régulièrement les définitions de NoSQL, CAP, Cypher.

---

### **9. Applications Pratiques (Exemples)**

- **Réseaux sociaux** : Relations entre utilisateurs, recommandations d’amis.
- **Systèmes de navigation** : Connexions géographiques entre villes, calculs de trajets optimaux.
- **Systèmes de recommandation** : Relations utilisateur-produit, suggestions basées sur les connexions.

---

### **10. Astuces d'Examen**

1. **Lisez attentivement les énoncés** : Comprendre le type de requête demandée (création, lecture, suppression).
2. **Tracez des schémas avant d'écrire Cypher** : Aidez-vous des visuels pour structurer vos requêtes.
3. **Gérez votre temps** : Répartissez vos efforts entre les questions de cours et les exercices pratiques.
4. **Révisez les concepts CAP et RDF** : Questions courtes, mais pointues.