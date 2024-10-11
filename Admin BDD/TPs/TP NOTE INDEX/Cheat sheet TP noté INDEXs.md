---
tags:
  - AdminBDD
  - cours
---
# **Documentation Complète sur les Index, Contraintes, Vues Méta-Schéma et Paquetage DBMS_ROWID**

---

Cette documentation est conçue pour vous aider à réussir n'importe quel TP (travaux pratiques) similaire portant sur les index, les contraintes, la consultation des vues méta-schéma, et la manipulation du paquetage PL/SQL `DBMS_ROWID`. Elle est organisée par thèmes et comprend des explications détaillées ainsi que des exemples de code que vous pouvez utiliser directement.

---

## **Table des Matières**

1. [Création et Gestion des Tables et Contraintes](#section1)
   - Création de tables
   - Contraintes de clé primaire
   - Contraintes de clé étrangère
   - Contraintes CHECK et UNIQUE
2. [Index en Bases de Données](#section2)
   - Création d'index
   - Types d'index
   - Gestion des index
3. [Consultation des Vues Méta-Schéma](#section3)
   - Vues USER_INDEXES et INDEX_STATS
   - Mise à jour des statistiques
   - Analyse des index
4. [Paquetage PL/SQL DBMS_ROWID](#section4)
   - Compréhension des ROWID
   - Fonctions du paquetage DBMS_ROWID
   - Manipulation avancée des ROWID
5. [Procédures PL/SQL](#section5)
   - Création de procédures
   - Gestion des exceptions
   - Exemples de procédures
6. [Optimisation des Requêtes et des Index](#section6)
   - Utilisation des index pour optimiser les requêtes
   - Analyse des plans d'exécution
   - Facteur de blocage et performance
7. [Sécurité et Permissions](#section7)
   - Gestion des permissions sur les tables et index
   - Vues d'administration
8. [Bonnes Pratiques et Conseils](#section8)
   - Bonnes pratiques pour les index
   - Maintenance des index
   - Documentation et organisation du code

---

<a name="section1"></a>
## **1. Création et Gestion des Tables et Contraintes**

### **1.1. Création de Tables**

#### **Syntaxe Générale**

```sql
CREATE TABLE nom_table (
    colonne1 TYPE_DONNÉE [CONTRAINTES],
    colonne2 TYPE_DONNÉE [CONTRAINTES],
    ...
    [CONTRAINTE_TABLE]
);
```

#### **Exemple : Création de la table DEPARTEMENT**

```sql
CREATE TABLE DEPARTEMENT (
    NUMDEP VARCHAR2(3) NOT NULL, -- Numéro du département
    NOMDEP VARCHAR2(50),         -- Nom du département
    CONSTRAINT DEPARTEMENT_PK PRIMARY KEY (NUMDEP) -- Clé primaire sur NUMDEP
);
```

### **1.2. Contraintes de Clé Primaire**

- Une **clé primaire** est un attribut ou un ensemble d'attributs qui identifie de manière unique chaque enregistrement d'une table.
- Définie au niveau de la colonne ou de la table.

#### **Au Niveau de la Colonne**

```sql
colonne TYPE_DONNÉE PRIMARY KEY
```

#### **Au Niveau de la Table**

```sql
CONSTRAINT nom_contrainte PRIMARY KEY (colonne1, colonne2)
```

#### **Exemple : Clé primaire sur CODEINSEE dans COMMUNE**

```sql
CREATE TABLE COMMUNE (
    CODEINSEE VARCHAR2(5) NOT NULL,
    NOMCOM VARCHAR2(100),
    NUMDEP VARCHAR2(3) NOT NULL,
    CONSTRAINT COMMUNE_PK PRIMARY KEY (CODEINSEE)
);
```

### **1.3. Contraintes de Clé Étrangère**

- Une **clé étrangère** établit une relation entre deux tables en faisant référence à la clé primaire d'une autre table.
- Empêche l'insertion de valeurs qui n'existent pas dans la table référencée.

#### **Syntaxe**

```sql
ALTER TABLE nom_table
ADD CONSTRAINT nom_contrainte
FOREIGN KEY (colonne_locale)
REFERENCES table_referencée (colonne_referencée);
```

#### **Exemple : Clé étrangère sur NUMDEP dans COMMUNE**

```sql
ALTER TABLE COMMUNE
ADD CONSTRAINT COMMUNE_FK_NUMDEP
FOREIGN KEY (NUMDEP)
REFERENCES DEPARTEMENT (NUMDEP);
```

### **1.4. Contraintes CHECK et UNIQUE**

#### **Contrainte CHECK**

- Assure que les valeurs d'une colonne respectent une condition donnée.

```sql
ALTER TABLE nom_table
ADD CONSTRAINT nom_contrainte
CHECK (condition);
```

#### **Exemple : Contraindre POPULATION à être positive**

```sql
ALTER TABLE COMMUNE
ADD CONSTRAINT COMMUNE_CHK_POPULATION
CHECK (POPULATION >= 0);
```

#### **Contrainte UNIQUE**

- Garantit l'unicité des valeurs dans une colonne ou un ensemble de colonnes.

```sql
ALTER TABLE nom_table
ADD CONSTRAINT nom_contrainte
UNIQUE (colonne1, colonne2);
```

#### **Exemple : Unicité sur NOMCOM**

```sql
ALTER TABLE COMMUNE
ADD CONSTRAINT COMMUNE_UNQ_NOMCOM
UNIQUE (NOMCOM);
```

---

<a name="section2"></a>
## **2. Index en Bases de Données**

### **2.1. Création d'Index**

- Un **index** améliore les performances des requêtes en permettant un accès rapide aux lignes d'une table.
- Créé sur une ou plusieurs colonnes.

#### **Syntaxe**

```sql
CREATE [UNIQUE] INDEX nom_index
ON nom_table (colonne1 [, colonne2, ...]);
```

#### **Exemple : Index non unique sur NUMDEP**

```sql
CREATE INDEX numdep_idx
ON COMMUNE (NUMDEP);
```

### **2.2. Types d'Index**

#### **Index Unique**

- Empêche les valeurs dupliquées dans les colonnes indexées.

```sql
CREATE UNIQUE INDEX nom_index
ON nom_table (colonne);
```

#### **Index Composite**

- Indexe plusieurs colonnes ensemble.

```sql
CREATE INDEX nom_index
ON nom_table (colonne1, colonne2);
```

#### **Index Bitmap**

- Utilisé pour des colonnes avec peu de valeurs distinctes (faible cardinalité).

```sql
CREATE BITMAP INDEX nom_index
ON nom_table (colonne);
```

#### **Index Fonctionnel**

- Indexe le résultat d'une fonction ou d'une expression.

```sql
CREATE INDEX nom_index
ON nom_table (fonction(colonne));
```

#### **Exemple : Index fonctionnel sur UPPER(NOMCOM)**

```sql
CREATE INDEX idx_upper_nomcom
ON COMMUNE (UPPER(NOMCOM));
```

### **2.3. Gestion des Index**

#### **Suppression d'un Index**

```sql
DROP INDEX nom_index;
```

#### **Exemple : Supprimer l'index numdep_idx**

```sql
DROP INDEX numdep_idx;
```

#### **Renommer un Index**

```sql
ALTER INDEX nom_index_original
RENAME TO nouveau_nom_index;
```

#### **Exemple : Renommer COMMUNE_PK en pk_commune_codeinsee**

```sql
ALTER INDEX COMMUNE_PK
RENAME TO pk_commune_codeinsee;
```

---

<a name="section3"></a>
## **3. Consultation des Vues Méta-Schéma**

### **3.1. Vues Principales**

#### **USER_INDEXES**

- Contient des informations sur les index de l'utilisateur.

```sql
SELECT * FROM USER_INDEXES;
```

#### **INDEX_STATS**

- Fournit des statistiques détaillées sur un index après analyse.

```sql
SELECT * FROM INDEX_STATS;
```

### **3.2. Mise à Jour des Statistiques**

- Les statistiques doivent être à jour pour obtenir des informations précises.

#### **Analyser une Table**

```sql
ANALYZE TABLE nom_table COMPUTE STATISTICS;
```

#### **Analyser un Index**

```sql
ANALYZE INDEX nom_index VALIDATE STRUCTURE;
```

#### **Exemple : Analyser l'index COMMUNE_PK**

```sql
ANALYZE INDEX COMMUNE_PK VALIDATE STRUCTURE;
```

### **3.3. Exemples de Requêtes**

#### **Obtenir la Hauteur d'un Index**

```sql
SELECT blevel + 1 AS hauteur
FROM USER_INDEXES
WHERE index_name = 'NOM_INDEX';
```

#### **Obtenir les Statistiques d'un Index**

```sql
SELECT name, btree_space, lf_rows, br_rows, height
FROM INDEX_STATS;
```

#### **Exemple : Hauteur de l'index NUMDEP_IDX**

```sql
SELECT blevel + 1 AS hauteur
FROM USER_INDEXES
WHERE index_name = 'NUMDEP_IDX';
```

---

<a name="section4"></a>
## **4. Paquetage PL/SQL DBMS_ROWID**

### **4.1. Compréhension des ROWID**

- Un **ROWID** est un identifiant unique pour chaque ligne d'une table, contenant des informations sur l'emplacement physique de la ligne.
- Structure du ROWID :
  - Objet : Identifie l'objet (table ou index).
  - Fichier : Numéro du fichier de données.
  - Bloc : Numéro du bloc contenant la ligne.
  - Ligne : Position de la ligne dans le bloc.

### **4.2. Fonctions du Paquetage**

- **DBMS_ROWID.ROWID_OBJECT(rowid)** : Retourne le numéro de l'objet.
- **DBMS_ROWID.ROWID_RELATIVE_FNO(rowid)** : Retourne le numéro du fichier relatif.
- **DBMS_ROWID.ROWID_BLOCK_NUMBER(rowid)** : Retourne le numéro du bloc.
- **DBMS_ROWID.ROWID_ROW_NUMBER(rowid)** : Retourne le numéro de ligne dans le bloc.

### **4.3. Exemples d'Utilisation**

#### **Obtenir le Numéro de Bloc d'une Ligne**

```sql
SELECT DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) AS bloc_id
FROM nom_table
WHERE condition;
```

#### **Exemple : Numéro de bloc pour une commune donnée**

```sql
SELECT DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) AS bloc_id
FROM COMMUNE
WHERE CODEINSEE = '34172';
```

---

<a name="section5"></a>
## **5. Procédures PL/SQL**

### **5.1. Création de Procédures**

#### **Syntaxe Générale**

```sql
CREATE OR REPLACE PROCEDURE nom_procedure (paramètres) AS
BEGIN
    -- Corps de la procédure
EXCEPTION
    -- Gestion des exceptions
END nom_procedure;
```

### **5.2. Gestion des Exceptions**

- **NO_DATA_FOUND** : Aucune donnée n'a été trouvée.
- **TOO_MANY_ROWS** : La requête a retourné plus d'une ligne.
- **OTHERS** : Capture toutes les autres exceptions.

#### **Exemple**

```sql
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Aucune donnée trouvée.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Erreur : ' || SQLERRM);
```

### **5.3. Exemples de Procédures**

#### **Procédure MEMEBLOCQUE**

- **Objectif** : Afficher les enregistrements dans le même bloc qu'une ligne donnée.

```sql
CREATE OR REPLACE PROCEDURE MEMEBLOCQUE (
    p_codeinsee IN COMMUNE.CODEINSEE%TYPE
) AS
    v_rowid ROWID;
BEGIN
    SELECT ROWID INTO v_rowid
    FROM COMMUNE
    WHERE CODEINSEE = p_codeinsee;

    FOR rec IN (
        SELECT CODEINSEE, NOMCOM
        FROM COMMUNE
        WHERE DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) = DBMS_ROWID.ROWID_BLOCK_NUMBER(v_rowid)
    ) LOOP
        DBMS_OUTPUT.PUT_LINE('Code INSEE : ' || rec.CODEINSEE || ', Nom Commune : ' || rec.NOMCOM);
    END LOOP;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Aucune commune trouvée avec le code INSEE ' || p_codeinsee);
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Erreur : ' || SQLERRM);
END MEMEBLOCQUE;
```

#### **Procédure NBRETUPLESPARBLOC**

- **Objectif** : Compter le nombre de tuples par bloc.

```sql
CREATE OR REPLACE PROCEDURE NBRETUPLESPARBLOC AS
BEGIN
    FOR rec IN (
        SELECT DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) AS bloc_id,
               COUNT(*) AS nombre_tuples
        FROM COMMUNE
        GROUP BY DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID)
        ORDER BY bloc_id
    ) LOOP
        DBMS_OUTPUT.PUT_LINE('Bloc ID : ' || rec.bloc_id || ', Nombre de tuples : ' || rec.nombre_tuples);
    END LOOP;
END NBRETUPLESPARBLOC;
```

---

<a name="section6"></a>
## **6. Optimisation des Requêtes et des Index**

### **6.1. Utilisation des Index pour Optimiser les Requêtes**

- Les index améliorent les performances des requêtes SELECT avec des clauses WHERE, JOIN, etc.
- **Exemple** : Requête optimisée grâce à un index sur NUMDEP.

```sql
SELECT *
FROM COMMUNE
WHERE NUMDEP = '34';
```

### **6.2. Analyse des Plans d'Exécution**

- **EXPLAIN PLAN** : Affiche le plan d'exécution d'une requête.

#### **Syntaxe**

```sql
EXPLAIN PLAN FOR
SELECT * FROM COMMUNE WHERE NUMDEP = '34';

SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

#### **Interprétation**

- **TABLE ACCESS BY INDEX ROWID** : Indique que l'index est utilisé.
- **INDEX RANGE SCAN** : Type de parcours de l'index.

### **6.3. Facteur de Blocage et Performance**

- **Facteur de Blocage** : Nombre de tuples pouvant être stockés dans un bloc.

#### **Calcul**

```plaintext
Facteur de Blocage = (Taille du Bloc * (1 - PCT_FREE/100)) / Taille Moyenne des Tuples
```

- **Taille du Bloc** : Généralement 8 192 octets.
- **PCT_FREE** : Pourcentage d'espace libre réservé dans un bloc.
- **Taille Moyenne des Tuples** : Peut être obtenue via `avg_row_len` dans `USER_TABLES`.

---

<a name="section7"></a>
## **7. Sécurité et Permissions**

### **7.1. Gestion des Permissions sur les Tables et Index**

#### **Accorder des Permissions**

```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON nom_table TO nom_utilisateur;
GRANT CREATE INDEX ON nom_table TO nom_utilisateur;
```

#### **Révoquer des Permissions**

```sql
REVOKE SELECT ON nom_table FROM nom_utilisateur;
```

### **7.2. Vues d'Administration**

- **DBA_OBJECTS** : Contient des informations sur tous les objets de la base de données.
- **ALL_TABLES**, **ALL_INDEXES** : Informations sur les tables et index accessibles à l'utilisateur.

---

<a name="section8"></a>
## **8. Bonnes Pratiques et Conseils**

### **8.1. Bonnes Pratiques pour les Index**

- **Limiter le nombre d'index** : Trop d'index peuvent ralentir les opérations DML (INSERT, UPDATE, DELETE).
- **Indexer les colonnes fréquemment utilisées** dans les clauses WHERE et JOIN.
- **Éviter d'indexer des colonnes avec peu de valeurs distinctes**, sauf si un index bitmap est approprié.
- **Mettre à jour régulièrement les statistiques** pour que l'optimiseur puisse générer des plans d'exécution efficaces.

### **8.2. Maintenance des Index**

- **Rebuild des Index** : Reconstituer un index pour réduire la fragmentation.

```sql
ALTER INDEX nom_index REBUILD;
```

- **Surveiller les Performances** : Utiliser les vues méta-schéma pour identifier les index peu efficaces.

### **8.3. Documentation et Organisation du Code**

- **Nommer clairement les objets** : Utiliser des noms significatifs pour les tables, colonnes, contraintes, et index.
- **Commenter le code** : Ajouter des commentaires pour expliquer les parties complexes du code.
- **Organiser les scripts** : Séparer les scripts de création, de modification, et de suppression.

---

## **Utilisation des Exemples de Code**

- **Copier-Coller** : Vous pouvez copier les exemples de code directement dans votre éditeur SQL.
- **Adaptation** : Modifiez les noms des objets et des colonnes pour qu'ils correspondent à votre schéma.
- **Exécution** : N'oubliez pas d'activer l'affichage des résultats si nécessaire (par exemple, `SET SERVEROUTPUT ON;` en SQL*Plus).

---

## **Recherche Rapide avec Ctrl+F**

Utilisez les mots-clés suivants pour trouver rapidement les informations :

- **Création de table** : `CREATE TABLE`
- **Clé primaire** : `PRIMARY KEY`
- **Clé étrangère** : `FOREIGN KEY`
- **Contrainte CHECK** : `CHECK`
- **Contrainte UNIQUE** : `UNIQUE`
- **Création d'index** : `CREATE INDEX`
- **Index unique** : `UNIQUE INDEX`
- **Index bitmap** : `BITMAP INDEX`
- **Index fonctionnel** : `FUNCTIONAL INDEX`
- **Suppression d'index** : `DROP INDEX`
- **Renommer un index** : `ALTER INDEX RENAME`
- **Vues méta-schéma** : `USER_INDEXES`, `INDEX_STATS`
- **Mise à jour des statistiques** : `ANALYZE`
- **ROWID** : `DBMS_ROWID`
- **Procédures PL/SQL** : `CREATE OR REPLACE PROCEDURE`
- **Exceptions PL/SQL** : `EXCEPTION`, `NO_DATA_FOUND`, `OTHERS`
- **Optimisation des requêtes** : `EXPLAIN PLAN`
- **Facteur de blocage** : `Facteur de Blocage`
- **Permissions** : `GRANT`, `REVOKE`
- **Bonnes pratiques** : `Bonnes Pratiques`

---

## **Conclusion**

Cette documentation vise à vous fournir toutes les informations nécessaires pour aborder avec succès des TPs similaires sur les bases de données relationnelles, en particulier sur les index, les contraintes, l'utilisation des vues méta-schéma, et la manipulation avancée avec PL/SQL. N'hésitez pas à explorer chaque section et à tester les exemples pour renforcer votre compréhension.

---

**Note** : Cette documentation est destinée à des fins éducatives et doit être utilisée conformément aux politiques académiques en vigueur. Assurez-vous de comprendre le code et les concepts plutôt que de simplement copier-coller, afin de développer vos compétences en base de données.