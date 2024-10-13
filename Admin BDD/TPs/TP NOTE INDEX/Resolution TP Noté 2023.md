---
tags:
  - AdminBDD
  - tps
---
### **TP2 : Index (Noté) – Durée : 3 heures**

---
BLABLA Marta
## **1. Schéma exploité**

### **Introduction**
Dans cette première section, nous allons créer deux tables : `COMMUNE` et `DEPARTEMENT`. Nous définirons des contraintes de clé primaire et une contrainte de clé étrangère pour lier ces deux tables. 

### **1.1 Question 1 (2 points)**
**Créer les tables `COMMUNE` et `DEPARTEMENT` avec les contraintes appropriées.**

#### **Étapes à suivre :**
1. **Créer la table `COMMUNE`** à partir de la table existante dans le schéma utilisateur `P00000009432`.
2. **Définir une contrainte de clé primaire** nommée `COMMUNE_PK` sur l’attribut `CODEINSEE` de la table `COMMUNE`.
3. **Créer la table `DEPARTEMENT`** de manière similaire.
4. **Définir une contrainte de clé primaire** pour la table `DEPARTEMENT`.
5. **Définir une contrainte de clé étrangère** sur l’attribut `NUMDEP` de `COMMUNE` qui référence `NUMDEP` de `DEPARTEMENT`.

#### **Concepts Clés :**
- **Clé Primaire (Primary Key) :** Un attribut ou un ensemble d’attributs qui identifie de manière unique chaque enregistrement dans une table.
- **Clé Étrangère (Foreign Key) :** Un attribut qui crée une relation entre deux tables en faisant référence à la clé primaire d’une autre table.

#### **Ordre de Création :**
1. Créer la table `DEPARTEMENT` avec sa clé primaire.
2. Créer la table `COMMUNE` avec sa clé primaire.
3. Ajouter la contrainte de clé étrangère sur `NUMDEP` dans `COMMUNE` qui référence `DEPARTEMENT`.

#### **Code SQL :**

```sql
-- 1. Créer la table DEPARTEMENT avec la clé primaire NUMDEP
CREATE TABLE DEPARTEMENT (
    NUMDEP VARCHAR2(3) NOT NULL, -- Numéro du département, par exemple '001'
    NOMDEP VARCHAR2(50),         -- Nom du département
    -- Ajoutez d'autres attributs si nécessaire
    CONSTRAINT DEPARTEMENT_PK PRIMARY KEY (NUMDEP) -- Définition de la clé primaire
);

-- 2. Créer la table COMMUNE avec la clé primaire CODEINSEE
CREATE TABLE COMMUNE (
    CODEINSEE VARCHAR2(5) NOT NULL, -- Code INSEE de la commune, par exemple '34172'
    NOMCOM VARCHAR2(100),           -- Nom de la commune
    NUMDEP VARCHAR2(3) NOT NULL,    -- Numéro du département (clé étrangère)
    -- Ajoutez d'autres attributs si nécessaire
    CONSTRAINT COMMUNE_PK PRIMARY KEY (CODEINSEE) -- Définition de la clé primaire
);

-- 3. Ajouter la contrainte de clé étrangère sur NUMDEP dans COMMUNE
ALTER TABLE COMMUNE
ADD CONSTRAINT COMMUNE_FK_NUMDEP
FOREIGN KEY (NUMDEP) -- Attribut dans COMMUNE
REFERENCES DEPARTEMENT (NUMDEP); -- Attribut référencé dans DEPARTEMENT
```

#### **Explications :**
- **CREATE TABLE DEPARTEMENT :** Crée la table `DEPARTEMENT` avec deux colonnes principales : `NUMDEP` et `NOMDEP`. La contrainte `DEPARTEMENT_PK` assure que chaque `NUMDEP` est unique et non nul.
- **CREATE TABLE COMMUNE :** Crée la table `COMMUNE` avec `CODEINSEE` comme clé primaire, garantissant que chaque commune a un code unique. `NUMDEP` est une clé étrangère qui lie chaque commune à un département spécifique.
- **ALTER TABLE COMMUNE ADD CONSTRAINT :** Ajoute la contrainte de clé étrangère `COMMUNE_FK_NUMDEP` pour assurer que chaque `NUMDEP` dans `COMMUNE` existe dans `DEPARTEMENT`.

---

### **1.2 Question 2 (1 point)**
**Définir un index non unique `numdep_idx` sur l’attribut `NUMDEP` de `COMMUNE` pour faciliter les jointures avec `DEPARTEMENT`.**

#### **Concepts Clés :**
- **Index Non Unique :** Un index qui permet de rapidement rechercher des lignes basées sur les valeurs d’une ou plusieurs colonnes, sans garantir l’unicité des valeurs.

#### **Ordre de Création :**
1. Créer la table `DEPARTEMENT` et `COMMUNE` avec leurs clés primaires et étrangères comme dans la Question 1.
2. Créer l’index `numdep_idx` sur `NUMDEP` dans `COMMUNE`.

#### **Code SQL :**

```sql
-- Création de l'index non unique numdep_idx sur NUMDEP dans COMMUNE
CREATE INDEX numdep_idx
ON COMMUNE (NUMDEP);
```

#### **Explications :**
- **CREATE INDEX numdep_idx :** Crée un index nommé `numdep_idx` sur la colonne `NUMDEP` de la table `COMMUNE`. Cet index améliorera les performances des requêtes qui filtrent ou joignent sur `NUMDEP`, notamment lors des jointures avec la table `DEPARTEMENT`.

---

## **2. Rappel sur la consultation des vues du méta-schéma relatives aux index**

### **Introduction**
Les vues `USER_INDEXES` et `INDEX_STATS` permettent d’obtenir des informations sur les index définis dans le schéma utilisateur. Ces vues sont utiles pour comprendre la structure et les performances des index dans la base de données.

### **Concepts Clés :**
- **USER_INDEXES :** Vue qui contient des informations sur tous les index appartenant à l'utilisateur courant.
- **INDEX_STATS :** Vue qui fournit des statistiques détaillées sur les index, telles que l’espace occupé, le nombre de blocs, etc.

### **Exemples de Requêtes :**
- **Lister les index :**
  ```sql
  SELECT index_name, blevel, table_name FROM USER_INDEXES;
  ```
- **Obtenir des statistiques détaillées après avoir mis à jour les statistiques :**
  ```sql
  -- Mettre à jour les statistiques pour un index spécifique
  ANALYZE INDEX <index_name> VALIDATE STRUCTURE;

  -- Sélectionner les statistiques
  SELECT name, btree_space, most_repeated_key, lf_rows, br_rows, height FROM INDEX_STATS;
  ```

---

## **3. Manipulation du paquetage DBMS_ROWID**

### **Introduction**
Le paquetage `DBMS_ROWID` permet de manipuler et d’extraire des informations à partir des identifiants de ligne (`ROWID`). Chaque enregistrement dans une table possède un `ROWID` unique qui contient des informations sur l'emplacement physique de la ligne dans la base de données.

### **Concepts Clés :**
- **ROWID :** Un identifiant unique pour chaque ligne dans une table, incluant des informations telles que le numéro de bloc et l’adresse relative.
- **Paquetage DBMS_ROWID :** Fournit des fonctions pour extraire des informations spécifiques à partir d’un `ROWID`.

### **Exemples :**
```sql
DECLARE
    object_no INTEGER;
    row_no INTEGER;
    row_id ROWID;
BEGIN
    -- Récupérer le ROWID d'une commune spécifique
    SELECT ROWID INTO row_id FROM COMMUNE
    WHERE CODEINSEE = '34172';
    
    -- Extraire le numéro de l'objet (table ou index) à partir du ROWID
    object_no := DBMS_ROWID.ROWID_OBJECT(row_id);
    
    -- Extraire le numéro de la ligne à partir du ROWID
    row_no := DBMS_ROWID.ROWID_ROW_NUMBER(row_id);
    
    -- Afficher les informations extraites
    DBMS_OUTPUT.PUT_LINE('Le numéro de l\'objet est ' || object_no || ' et le numéro de la ligne est ' || row_no);
END;
/
```

#### **Explications :**
- **DBMS_ROWID.ROWID_OBJECT(row_id) :** Renvoie le numéro de l’objet associé au `ROWID`.
- **DBMS_ROWID.ROWID_ROW_NUMBER(row_id) :** Renvoie le numéro de la ligne dans l’objet.

---

## **1.2 Question 3 (6 points)**
**Exploiter les vues `INDEX_STATS` et `USER_INDEXES` pour répondre aux questions suivantes.**

### **Étapes à suivre :**
1. **Mettre à jour les statistiques** pour la table `COMMUNE` afin d’obtenir des informations précises sur les index.
2. **Exécuter des requêtes** sur les vues `USER_INDEXES` et `INDEX_STATS` pour répondre aux questions posées.

### **1. Mettre à jour les statistiques**

#### **Code SQL :**
```sql
-- Mettre à jour les statistiques pour la table COMMUNE
ANALYZE TABLE COMMUNE COMPUTE STATISTICS;

-- Ou, pour tout le schéma utilisateur
EXEC DBMS_UTILITY.ANALYZE_SCHEMA(USER, 'COMPUTE');
```

#### **Explications :**
- **ANALYZE TABLE ... COMPUTE STATISTICS :** Calcule les statistiques pour optimiser les performances des requêtes.
- **DBMS_UTILITY.ANALYZE_SCHEMA :** Calcule les statistiques pour toutes les tables du schéma utilisateur.

### **2. Requêtes pour répondre aux questions**

#### **Question 1 : Quelle est la hauteur de l’index `COMMUNE_PK` de la table `COMMUNE` ?**

##### **Code SQL :**
```sql
SELECT blevel
FROM USER_INDEXES
WHERE index_name = 'COMMUNE_PK';
```

##### **Explications :**
- **blevel :** Indique la hauteur de l’arbre de l’index sans compter le niveau des feuilles.
- **Résultat :** La hauteur de l’index `COMMUNE_PK`.

#### **Question 2 : Quels sont les nombres de blocs de branches et de feuilles réservés pour l’index `COMMUNE_PK` de la table `COMMUNE` ?**

##### **Étapes :**
1. **Mettre à jour les statistiques** pour l’index `COMMUNE_PK`.
2. **Sélectionner les informations** dans `INDEX_STATS`.

##### **Code SQL :**
```sql
-- Mettre à jour les statistiques pour l'index COMMUNE_PK
ANALYZE INDEX COMMUNE_PK VALIDATE STRUCTURE;

-- Sélectionner les blocs de branches et de feuilles
SELECT br_rows, lf_rows
FROM INDEX_STATS
WHERE name = 'COMMUNE_PK';
```

##### **Explications :**
- **br_rows :** Nombre de blocs de branches dans l’index.
- **lf_rows :** Nombre de blocs de feuilles dans l’index.
- **Résultat :** Les nombres de blocs de branches et de feuilles pour `COMMUNE_PK`.

#### **Question 3 : Quelle est la taille de chaque tuple (clé, pointeurs, ROWID) présent au niveau des blocs des feuilles de l’index `COMMUNE_PK` ?**

##### **Code SQL :**
```sql
SELECT btree_space / lf_rows AS taille_tuple
FROM INDEX_STATS
WHERE name = 'COMMUNE_PK';
```

##### **Explications :**
- **btree_space :** Espace total occupé par l’index.
- **lf_rows :** Nombre de tuples dans les blocs de feuilles.
- **taille_tuple :** Taille moyenne de chaque tuple dans les blocs de feuilles.

#### **Question 4 : Par comparaison, quelle est la taille moyenne de chaque tuple de la table `COMMUNE` et combien de tuples peuvent être stockés dans un bloc ?**

##### **Étapes :**
1. **Calculer la taille moyenne des tuples** de la table `COMMUNE`.
2. **Calculer le nombre de tuples par bloc** en tenant compte de `PCT_FREE`.

##### **Code SQL :**
```sql
-- Sélectionner la taille moyenne des tuples et le nombre de blocs
SELECT avg_row_len, blocks, pct_free
FROM user_tables
WHERE table_name = 'COMMUNE';
```

##### **Calcul :**
1. **Taille moyenne des tuples :** `avg_row_len` en octets.
2. **Facteur de blocage :** Calculer combien de tuples peuvent tenir dans un bloc en utilisant la formule suivante :

   \[
   \text{Nombre de tuples par bloc} = \frac{\text{Taille du bloc} \times (1 - \frac{\text{PCT_FREE}}{100})}{\text{Taille moyenne des tuples}}
   \]

   Supposons que la taille d’un bloc est de 8 192 octets (par défaut dans Oracle).

##### **Exemple de Calcul :**
- **avg_row_len :** 100 octets
- **PCT_FREE :** 10%
- **Taille utile par bloc :** 8 192 * (1 - 0,10) = 7 372,8 octets
- **Nombre de tuples par bloc :** 7 372,8 / 100 ≈ 73 tuples par bloc

#### **Question 5 : Quelle est la hauteur de l’index `NUMDEP_IDX` de la table `COMMUNE` ?**

##### **Code SQL :**
```sql
SELECT blevel
FROM USER_INDEXES
WHERE index_name = 'NUMDEP_IDX';
```

##### **Explications :**
- **blevel :** Hauteur de l’arbre de l’index `NUMDEP_IDX`.

#### **Question 6 : Expliquer ce que renvoient les valeurs des attributs `DISTINCT_KEYS` et `MOST_REPEATED_KEY` de la vue `INDEX_STATS` pour l’index `NUMDEP_IDX`.**

##### **Code SQL :**
```sql
SELECT distinct_keys, most_repeated_key
FROM INDEX_STATS
WHERE name = 'NUMDEP_IDX';
```

##### **Explications :**
- **distinct_keys :** Nombre de clés distinctes dans l’index. Pour `NUMDEP_IDX`, cela correspond au nombre de départements différents référencés.
- **most_repeated_key :** La clé la plus répétée dans l’index. Cela indique la valeur de `NUMDEP` qui apparaît le plus souvent dans la table `COMMUNE`.

---

## **3. Manipulation du paquetage DBMS_ROWID**

### **3.1 Question 4 (5 points)**
**Utiliser `DBMS_ROWID` pour différentes opérations sur les `ROWID`.**

#### **Sous-Questions :**
1. **Que renvoie la requête suivante ?**
   ```sql
   SELECT rowid, rownum, codeinsee FROM COMMUNE;
   ```

2. **Créer une procédure PL/SQL nommée `MEMEBLOCQUE` qui affiche tous les enregistrements (codeInsee, nomcom) dans le même bloc qu’un enregistrement donné.**

3. **Créer une procédure PL/SQL nommée `NBRETUPLESPARBLOC` qui renvoie le numéro des blocs et le nombre de tuples dans chaque bloc.**

#### **Réponse à la Sous-Question 1 :**

##### **Requête SQL :**
```sql
SELECT rowid, rownum, codeinsee FROM COMMUNE;
```

##### **Explications :**
- **ROWID :** Identifiant unique de chaque ligne dans la table `COMMUNE`, indiquant l'emplacement physique de la ligne.
- **ROWNUM :** Numéro de la ligne dans le résultat de la requête, attribué séquentiellement.
- **codeinsee :** Code INSEE de la commune.

##### **Ce que renvoie la requête :**
- **ROWID :** L’identifiant unique de chaque enregistrement.
- **ROWNUM :** Le numéro de ligne dans le jeu de résultats.
- **codeinsee :** Le code INSEE de chaque commune.

---

### **Procédure PL/SQL `MEMEBLOCQUE`**

**Objectif :** Afficher tous les enregistrements (codeInsee, nomcom) contenus dans le même bloc qu’un enregistrement spécifique de la table `COMMUNE`.

#### **Code PL/SQL :**
```sql
CREATE OR REPLACE PROCEDURE MEMEBLOCQUE (
    p_codeinsee IN COMMUNE.CODEINSEE%TYPE -- Paramètre d'entrée : code INSEE de la commune
) AS
    v_rowid ROWID; -- Variable pour stocker le ROWID de la commune spécifiée
BEGIN
    -- Récupérer le ROWID de la commune spécifiée
    SELECT ROWID INTO v_rowid
    FROM COMMUNE
    WHERE CODEINSEE = p_codeinsee;
    
    -- Afficher les enregistrements dans le même bloc
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
/
```

#### **Explications :**
- **Paramètre d'entrée `p_codeinsee` :** Le code INSEE de la commune dont on souhaite trouver les autres communes dans le même bloc.
- **v_rowid :** Variable pour stocker le `ROWID` de la commune spécifiée.
- **DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) :** Fonction qui retourne le numéro de bloc du `ROWID`.
- **Boucle FOR :** Parcourt toutes les communes ayant le même numéro de bloc que la commune spécifiée et affiche leurs `CODEINSEE` et `NOMCOM`.
- **Gestion des Exceptions :**
  - **NO_DATA_FOUND :** Si aucune commune n’est trouvée avec le `CODEINSEE` donné.
  - **OTHERS :** Capture toute autre erreur et affiche un message d’erreur.

#### **Comment exécuter la procédure :**
```sql
-- Activer l’affichage des messages
SET SERVEROUTPUT ON;

-- Appeler la procédure avec un code INSEE spécifique
BEGIN
    MEMEBLOCQUE('34172');
END;
/
```

---

### **Procédure PL/SQL `NBRETUPLESPARBLOC`**

**Objectif :** Renvoie le numéro des blocs (id) et le nombre de tuples contenus dans chacun des blocs de la table `COMMUNE`. Vérifier si le nombre de tuples est en accord avec le facteur de blocage calculé précédemment.

#### **Code PL/SQL :**
```sql
CREATE OR REPLACE PROCEDURE NBRETUPLESPARBLOC AS
BEGIN
    -- Afficher les blocs et le nombre de tuples dans chaque bloc
    FOR rec IN (
        SELECT DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) AS bloc_id,
               COUNT(*) AS nombre_tuples
        FROM COMMUNE
        GROUP BY DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID)
        ORDER BY bloc_id
    ) LOOP
        DBMS_OUTPUT.PUT_LINE('Bloc ID : ' || rec.bloc_id || ', Nombre de tuples : ' || rec.nombre_tuples);
    END LOOP;
    
    -- Vérifier le facteur de blocage
    DECLARE
        v_avg_row_len NUMBER;
        v_blocks NUMBER;
        v_pct_free NUMBER;
        v_facteur_bloc NUMBER;
    BEGIN
        -- Récupérer les statistiques de la table COMMUNE
        SELECT avg_row_len, blocks, pct_free
        INTO v_avg_row_len, v_blocks, v_pct_free
        FROM user_tables
        WHERE table_name = 'COMMUNE';
        
        -- Calculer le nombre de tuples par bloc
        -- Supposons que la taille d’un bloc est de 8192 octets
        v_facteur_bloc := (8192 * (1 - v_pct_free / 100)) / v_avg_row_len;
        
        DBMS_OUTPUT.PUT_LINE('Facteur de blocage calculé : ' || FLOOR(v_facteur_bloc));
    END;
    
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Erreur : ' || SQLERRM);
END NBRETUPLESPARBLOC;
/
```

#### **Explications :**
- **Boucle FOR :** Parcourt chaque bloc et compte le nombre de tuples (enregistrements) dans ce bloc.
- **Calcul du Facteur de Blocage :**
  - **avg_row_len :** Taille moyenne des tuples.
  - **blocks :** Nombre total de blocs.
  - **pct_free :** Pourcentage d’espace libre par bloc.
  - **v_facteur_bloc :** Calcul du nombre de tuples pouvant tenir dans un bloc en tenant compte de l’espace libre.
- **Comparaison :** Compare le nombre de tuples réel par bloc avec le facteur de blocage calculé.

#### **Comment exécuter la procédure :**
```sql
-- Activer l’affichage des messages
SET SERVEROUTPUT ON;

-- Appeler la procédure
BEGIN
    NBRETUPLESPARBLOC;
END;
/
```

---

### **3.2 Question 5 (6 points)**
**Créer deux procédures PL/SQL : `BLOCSDUDEPARTEMENT` et `DANSCACHE`.**

#### **Sous-Questions :**
1. **Procédure `BLOCSDUDEPARTEMENT` :** Renvoie le numéro des différents blocs contenant les tuples de `COMMUNE` d’un département donné.
2. **Procédure `DANSCACHE` :** Renvoie le numéro des différents blocs de `COMMUNE` ainsi que leurs enregistrements lorsqu’ils sont présents dans le cache de données (`v$bh`).

#### **Procédure PL/SQL `BLOCSDUDEPARTEMENT`**

**Objectif :** Renvoie le numéro des blocs contenant les tuples de `COMMUNE` d’un département donné.

##### **Code PL/SQL :**
```sql
CREATE OR REPLACE PROCEDURE BLOCSDUDEPARTEMENT (
    p_numdep IN DEPARTEMENT.NUMDEP%TYPE -- Paramètre d'entrée : numéro du département
) AS
BEGIN
    -- Afficher les numéros des blocs contenant les communes du département donné
    FOR rec IN (
        SELECT DISTINCT DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) AS bloc_id
        FROM COMMUNE
        WHERE NUMDEP = p_numdep
    ) LOOP
        DBMS_OUTPUT.PUT_LINE('Bloc ID contenant le département ' || p_numdep || ' : ' || rec.bloc_id);
    END LOOP;
    
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Aucune commune trouvée pour le département ' || p_numdep);
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Erreur : ' || SQLERRM);
END BLOCSDUDEPARTEMENT;
/
```

##### **Explications :**
- **Paramètre d'entrée `p_numdep` :** Numéro du département dont on souhaite trouver les blocs contenant ses communes.
- **SELECT DISTINCT :** Sélectionne les numéros de blocs uniques où les communes du département sont stockées.
- **Boucle FOR :** Parcourt chaque bloc identifié et affiche son numéro.
- **Gestion des Exceptions :**
  - **NO_DATA_FOUND :** Si aucun département n’est trouvé avec le numéro donné.
  - **OTHERS :** Capture toute autre erreur et affiche un message d’erreur.

##### **Comment exécuter la procédure :**
```sql
-- Activer l’affichage des messages
SET SERVEROUTPUT ON;

-- Appeler la procédure avec un numéro de département spécifique
BEGIN
    BLOCSDUDEPARTEMENT('34'); -- Exemple avec le département '34'
END;
/
```

#### **Procédure PL/SQL `DANSCACHE`**

**Objectif :** Renvoie le numéro des différents blocs de `COMMUNE` ainsi que leurs enregistrements (codeInsee, nomcom) lorsqu’ils sont présents dans le cache de données.

##### **Code PL/SQL :**
```sql
CREATE OR REPLACE PROCEDURE DANSCACHE AS
BEGIN
    -- Afficher les blocs et les enregistrements présents dans le cache
    FOR rec IN (
        SELECT DISTINCT bh.block#, c.CODEINSEE, c.NOMCOM
        FROM v$bh bh
        JOIN COMMUNE c ON DBMS_ROWID.ROWID_BLOCK_NUMBER(c.ROWID) = bh.block#
    ) LOOP
        DBMS_OUTPUT.PUT_LINE('Bloc ID : ' || rec.block# || ', Code INSEE : ' || rec.CODEINSEE || ', Nom Commune : ' || rec.NOMCOM);
    END LOOP;
    
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Erreur : ' || SQLERRM);
END DANSCACHE;
/
```

##### **Explications :**
- **v$bh :** Vue dynamique qui affiche les blocs actuellement en cache.
- **JOIN avec COMMUNE :** Associe les blocs en cache avec les enregistrements de la table `COMMUNE`.
- **SELECT DISTINCT :** Sélectionne les enregistrements uniques présents dans les blocs en cache.
- **Boucle FOR :** Parcourt chaque enregistrement et affiche le numéro du bloc ainsi que les informations de la commune.
- **Gestion des Exceptions :**
  - **OTHERS :** Capture toute erreur et affiche un message d’erreur.

##### **Comment exécuter la procédure :**
```sql
-- Activer l’affichage des messages
SET SERVEROUTPUT ON;

-- Appeler la procédure
BEGIN
    DANSCACHE;
END;
/
```

#### **Remarques :**
- **v$bh :** Cette vue est accessible uniquement par les utilisateurs ayant les privilèges nécessaires (par exemple, DBA).
- **Optimisation :** Assure-toi que les droits d’accès sont correctement configurés pour exécuter cette procédure.

---

## **Conclusion**

Nous avons couvert la création des tables `COMMUNE` et `DEPARTEMENT` avec leurs contraintes, la création d’index pour optimiser les jointures, l’utilisation des vues méta-schéma pour analyser les index, et la manipulation avancée des `ROWID` avec des procédures PL/SQL. Chaque étape a été expliquée en détail avec des exemples de code commentés pour une compréhension maximale. N’hésite pas à tester chaque morceau de code et à poser des questions si tu rencontres des difficultés !