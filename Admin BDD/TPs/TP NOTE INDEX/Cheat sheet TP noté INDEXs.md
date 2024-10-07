---
tags:
  - AdminBDD
  - cours
---
Voici le cheat sheet amélioré avec des commentaires détaillés dans chaque chunk de code. Les commentaires sont expliqués étape par étape comme si vous les expliquiez à une personne de 17 ans, afin de rendre le code plus compréhensible. pour comprendre ce qu'on fait lire d'abord [[Lez et marta discussion sur les INDEXs]]

---

### **1. Création de Table et Contraintes**

**Mots-clés :**
- Création de table
- Clé primaire
- Clé étrangère

**Exemple :**

- **Table `COMMUNE` avec clé primaire :**
    ```sql
    -- On crée une nouvelle table appelée COMMUNE avec deux colonnes : CODEINSEE et NUMDEP.
    CREATE TABLE COMMUNE (
        -- CODEINSEE est de type chaîne de caractères (VARCHAR2) de longueur 10
        CODEINSEE VARCHAR2(10),
        -- NUMDEP est un nombre (NUMBER)
        NUMDEP NUMBER,
        -- On définit la clé primaire sur la colonne CODEINSEE, 
        -- cela signifie que chaque valeur de CODEINSEE doit être unique et ne peut pas être vide.
        PRIMARY KEY (CODEINSEE) CONSTRAINT COMMUNE_PK
    );
    ```

- **Table `DEPARTEMENT` avec clé primaire et clé étrangère sur `NUMDEP` :**
    ```sql
    -- On crée une table appelée DEPARTEMENT avec une colonne NUMDEP
    CREATE TABLE DEPARTEMENT (
        -- NUMDEP est la clé primaire, chaque département aura un numéro unique
        NUMDEP NUMBER PRIMARY KEY
        -- Autres colonnes possibles comme le nom du département
    );

    -- On ajoute une contrainte de clé étrangère sur la table COMMUNE
    -- Cela signifie que la colonne NUMDEP de COMMUNE doit correspondre
    -- à une valeur existante de NUMDEP dans la table DEPARTEMENT
    ALTER TABLE COMMUNE 
    ADD CONSTRAINT FK_NUMDEP 
    FOREIGN KEY (NUMDEP) 
    REFERENCES DEPARTEMENT(NUMDEP);
    ```

---

### **2. Création d’un Index Non Unique**

**Mots-clés :**
- Index non unique
- Jointures
- Index sur colonne

**Exemple :**
- **Création d’un index non unique sur `NUMDEP` :**
    ```sql
    -- On crée un index appelé numdep_idx sur la colonne NUMDEP de la table COMMUNE.
    -- Cela permettra de rendre les recherches sur NUMDEP plus rapides lors de jointures entre COMMUNE et DEPARTEMENT.
    CREATE INDEX numdep_idx ON COMMUNE(NUMDEP);
    ```

---

### **3. Consultation des Statistiques d'Index**

**Mots-clés :**
- Vue `user_indexes`
- Statistiques d'index
- Hauteur de l'arbre
- Blocs branches et feuilles

**Exemple :**
- **Consulter la hauteur de l'index :**
    ```sql
    -- Cette requête sélectionne le nom de l'index, sa hauteur (blevel + 1) et le nom de la table associée
    -- L'attribut blevel correspond à la profondeur de l'arbre, auquel on ajoute 1 pour obtenir la hauteur complète.
    SELECT index_name, blevel+1 AS height 
    FROM USER_INDEXES 
    WHERE index_name = 'COMMUNE_PK'; -- On cherche l'index appelé 'COMMUNE_PK'
    ```

- **Collecter les statistiques d’un index :**
    ```sql
    -- On met à jour les statistiques de l'index COMMUNE_PK afin d'obtenir des informations précises sur sa structure.
    ANALYZE INDEX COMMUNE_PK VALIDATE STRUCTURE;

    -- Cette requête récupère le nom de l'index, l'espace mémoire qu'il occupe, 
    -- la clé la plus fréquente, le nombre de lignes dans les feuilles, 
    -- le nombre de lignes dans les branches et la hauteur de l'arbre
    SELECT name, btree_space, most_repeated_key, lf_rows, br_rows, height 
    FROM INDEX_STATS 
    WHERE name = 'COMMUNE_PK';
    ```

---

### **4. Calcul du Facteur de Blocage**

**Mots-clés :**
- Facteur de blocage
- Taille des tuples
- Espace mémoire

**Exemple :**
- **Calcul du facteur de blocage pour la table `COMMUNE` :**
    ```sql
    -- On récupère le nombre de blocs utilisés par la table COMMUNE et la longueur moyenne des enregistrements (avg_row_len).
    -- Cela nous aide à calculer combien de tuples (lignes) peuvent être stockés dans un bloc de données.
    SELECT blocks, avg_row_len 
    FROM USER_TABLES 
    WHERE table_name = 'COMMUNE';
    ```

- **Calcul de la taille d’un bloc pour les enregistrements :**
    - Taille d'un bloc disponible : **7000 octets** (on a retiré l'espace utilisé pour les métadonnées)
    - Si chaque enregistrement fait **100 octets**, on peut calculer combien de lignes tiennent dans un bloc :
    ```sql
    -- Facteur de blocage : c'est le nombre d'enregistrements qu'on peut mettre dans un bloc de 7000 octets.
    -- On divise simplement 7000 par la taille moyenne d'un enregistrement.
    SELECT 7000 / avg_row_len FROM USER_TABLES WHERE table_name = 'COMMUNE';
    ```

---

### **5. Manipulation des ROWID et PL/SQL**

**Mots-clés :**
- ROWID
- Paquetage `DBMS_ROWID`
- Procédure PL/SQL

**Exemple :**
- **Procédure pour afficher les enregistrements dans le même bloc que l'enregistrement donné :**
    ```plsql
    -- On crée une procédure PL/SQL qui récupère les enregistrements du même bloc que celui d'un enregistrement donné.
    CREATE OR REPLACE PROCEDURE MEMEBLOCQUE(p_codeINSEE IN VARCHAR2) AS
        row_id ROWID; -- On déclare une variable pour stocker le ROWID de l'enregistrement
    BEGIN
        -- On récupère le ROWID de l'enregistrement avec le code INSEE donné
        SELECT ROWID INTO row_id FROM COMMUNE WHERE codeINSEE = p_codeINSEE;
        
        -- On parcourt tous les enregistrements qui se trouvent dans le même bloc que celui du ROWID récupéré
        FOR r IN (
            SELECT codeINSEE, nomcommaj 
            FROM COMMUNE 
            WHERE DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) = DBMS_ROWID.ROWID_BLOCK_NUMBER(row_id)
        ) LOOP
            -- On affiche chaque enregistrement trouvé dans le même bloc
            DBMS_OUTPUT.PUT_LINE(r.codeINSEE || ' ' || r.nomcommaj);
        END LOOP;
    END;
    ```

- **Afficher les informations du ROWID pour un enregistrement donné :**
    ```sql
    -- Cette requête utilise le ROWID pour récupérer le numéro de bloc et l'objet auquel appartient l'enregistrement.
    SELECT DBMS_ROWID.ROWID_BLOCK_NUMBER(rowid), DBMS_ROWID.ROWID_OBJECT(rowid)
    FROM COMMUNE
    WHERE codeINSEE = '34172'; -- On récupère les informations pour le code INSEE 34172
    ```

---

### **6. Création et Gestion des Index B-Tree**

**Mots-clés :**
- Index B-Tree
- Organisation de table
- Index unique

**Exemple :**
- **Création d’un index unique sur `code_insee` dans la table `COMMUNE` :**
    ```sql
    -- On crée un index unique appelé com_idx sur la colonne code_insee.
    -- Un index unique signifie que les valeurs dans la colonne code_insee doivent être uniques.
    CREATE UNIQUE INDEX com_idx ON COMMUNE (code_insee);
    ```

- **Désactiver un index :**
    ```sql
    -- Cette commande désactive l'index appelé com_idx.
    -- Cela peut être utile si l'on veut temporairement ne plus utiliser cet index sans le supprimer.
    ALTER INDEX com_idx DISABLE;
    ```

- **Reconstruction d’un index :**
    ```sql
    -- On utilise cette commande pour reconstruire un index qui a été marqué comme inutilisable.
    -- Cela permet de le remettre en service après une désactivation ou une modification.
    ALTER INDEX commune_pk REBUILD;
    ```

---

### **7. Index Bitmap**

**Mots-clés :**
- Index bitmap
- Cardinalité faible
- Requêtes agrégées

**Exemple :**
- **Création d’un index bitmap sur la colonne `fonction` :**
    ```sql
    -- On crée un index bitmap sur la colonne fonction de la table EMP.
    -- Un index bitmap est efficace lorsque la colonne a un nombre limité de valeurs distinctes (comme des catégories).
    CREATE BITMAP INDEX fonction_idx ON EMP(fonction);
    ```

- **Requête d’agrégation avec index bitmap :**
    ```sql
    -- Cette requête compte combien d'employés ont la fonction de 'président'.
    -- Grâce à l'index bitmap, la recherche est rapide car on peut simplement compter les 1 dans le bitmap correspondant.
    SELECT COUNT(*) FROM EMP WHERE fonction = 'président';
    ```

---

### **8. Index de Hachage**

**Mots-clés :**
- Index de hachage
- Recherche exacte
- Fonction de hachage

**Exemple :**
- **Création d’une table avec partitionnement par hachage :**
```sql
    -- On crée un cluster de tables appelé emp_cluster qui regroupera les enregistrements basés sur la colonne num.
    -- Un cluster permet de stocker les enregistrements qui partagent une même valeur de clé ensemble.
    -- SIZE 500 indique la taille estimée de chaque tuple (en octets) et HASHKEYS indique le nombre de clés dans le cluster.
    CREATE CLUSTER emp_cluster (num NUMBER(5,0)) 
        SIZE 500 
        HASHKEYS 1500;

    -- Ensuite, on crée une table employe qui sera liée au cluster emp_cluster.
    -- Cela signifie que les enregistrements de la table employe seront stockés dans le cluster basé sur num.
    CREATE TABLE employe (
        num NUMBER(5,0) PRIMARY KEY, -- num est la clé primaire de la table employe
        name VARCHAR(15) -- name est le nom de l'employé
    ) CLUSTER emp_cluster (num); -- On indique que cette table est dans le cluster emp_cluster, basé sur la colonne num
```

---

### **9. Questions de TD sur les Index et Arbres B+**

**Mots-clés :**
- Hauteur d’un arbre B+
- Coût d'accès à un bloc
- Insertion dans un B-Tree

**Exemple :**
- **Calcul du facteur de blocage pour une table de 50 000 tuples :**
    ```sql
    -- On calcule combien de tuples (lignes) peuvent tenir dans un bloc de données.
    -- Taille disponible dans un bloc : 7000 octets
    -- Taille d’un tuple : 100 octets
    -- Facteur de blocage = 7000 / taille d’un tuple = nombre de tuples par bloc.
    SELECT 7000 / 100 AS facteur_de_blocage; -- Ici, chaque bloc peut contenir 70 tuples
    ```

- **Requête sur l'attribut clé avec un B+-Tree :**
    ```sql
    -- Cette requête recherche un enregistrement dans la table EMP basé sur l'attribut clé Num.
    -- Grâce à l'index B+Tree, on parcourt rapidement l'index pour trouver l'enregistrement correspondant à Num = 12345.
    SELECT COUNT(*) 
    FROM EMP 
    WHERE Num = 12345; -- Num est le numéro unique de l'employé
    ```

- **Calcul de la hauteur d’un arbre B+ (Exemple TD) :**
    ```sql
    -- On calcule la hauteur de l'arbre B+ pour une table de 50 000 enregistrements.
    -- Taille d’un enregistrement d'index : 20 octets
    -- Taille du bloc : 8192 octets
    -- Facteur de blocage pour l'index = taille du bloc / taille d'un enregistrement d'index.
    -- On calcule ensuite combien de niveaux sont nécessaires pour stocker 50 000 enregistrements.
    SELECT 8192 / 20 AS facteur_de_blocage_index; -- Chaque bloc d’index peut contenir environ 409 enregistrements

    -- Ensuite, on calcule le nombre de blocs nécessaires pour stocker 50 000 enregistrements dans l'index.
    -- Le résultat nous donnera la hauteur de l'arbre B+ (en nombre de niveaux).
    SELECT CEIL(50000 / 409) AS nombre_de_blocs;
    ```

- **Coût d'accès avec et sans tuple dans un B+-Tree :**
    ```sql
    -- Si le tuple existe dans la table :
    -- On parcourt en moyenne log2(n) blocs, où n est le nombre total de blocs de l'index.
    -- Le coût en nombre de blocs pour récupérer un tuple existant dans un arbre B+ est log2(nombre de blocs dans l'arbre).
    SELECT LOG(2, nombre_de_blocs) AS cout_acces_si_existe;
    
    -- Si le tuple n'existe pas, on doit parcourir tous les blocs à la recherche du tuple manquant.
    -- Le coût est donc plus élevé.
    SELECT nombre_de_blocs AS cout_acces_si_pas_existe;
    ```

- **Ajout d’un niveau intermédiaire dans un B-Tree :**
    ```sql
    -- On suppose que l'arbre a actuellement une hauteur de 2 niveaux et peut contenir 50 000 tuples.
    -- On cherche combien de tuples supplémentaires il faudrait ajouter pour qu'un nouveau niveau soit nécessaire.
    -- Chaque niveau peut contenir un certain nombre de pointeurs vers les blocs enfants.
    -- On calcule combien de tuples peuvent être contenus dans un arbre de hauteur h+1.
    SELECT (409 * 409) AS capacite_arbre_niveau_supplementaire; 
    -- Ici, 409 est le facteur de blocage pour chaque niveau, et on le multiplie par lui-même pour calculer la capacité d'un niveau supplémentaire.
    ```

---

### **Résumé et Astuces**

- Utilisez **les vues d'index** (`USER_INDEXES`, `INDEX_STATS`) pour récupérer des informations précises sur les index dans votre base de données.
- Utilisez des **index B-Tree** pour des colonnes ayant des valeurs uniques ou des plages de valeurs à rechercher.
- Préférez les **index bitmap** pour les colonnes à faible cardinalité (nombre limité de valeurs différentes).
- Les **index de hachage** sont parfaits pour des recherches exactes et rapides.
- **Analysez régulièrement vos index** avec `ANALYZE INDEX` pour garantir des performances optimales.

Ce cheat sheet vous fournit les éléments essentiels pour gérer les index et les B-Trees dans un TP ou un exercice. N’hésitez pas à copier, commenter, et adapter les commandes SQL selon vos besoins spécifiques.