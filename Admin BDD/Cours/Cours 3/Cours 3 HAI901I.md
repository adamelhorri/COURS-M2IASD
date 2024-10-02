---
tags:
  - AdminBDD
  - cours
---

### **SGBD : Généralités**

Un **Système de Gestion de Bases de Données** (SGBD) est un logiciel complexe qui permet de gérer, stocker, et interagir avec les données de manière persistante. Les principales fonctionnalités d'un SGBD sont les suivantes :
- **Création de bases de données** : Le SGBD permet de créer des bases de données pour organiser et stocker les données de manière structurée.
- **Gestion des données** : Permet de persister, interroger, modifier et optimiser l'accès aux données.
- **Gestion de la concurrence** : Le SGBD doit être capable de gérer plusieurs utilisateurs simultanément sans conflit.
- **Sécurité** : Contrôle des accès, gestion des droits, et récupération en cas de panne.

### **Utilisateurs d'un SGBD**

Les utilisateurs d'un SGBD Oracle peuvent être divisés en plusieurs catégories :
1. **Administrateurs (DBA)** : Ils créent les bases de données, gèrent les utilisateurs, et assurent la maintenance évolutive et corrective des bases de données.
2. **Utilisateurs interrogateurs** : Ceux-ci créent et exécutent des requêtes pour récupérer des informations.
3. **Utilisateurs spécifiques (concepteurs d'applications)** : Ils développent des applications qui interagissent avec la base de données. L'administrateur de données (ou **Data Manager**) se préoccupe du respect des besoins métiers et de la gestion des données.

### **Le Rôle du DBA**

Le **DBA** (Database Administrator) a la responsabilité d'installer les serveurs Oracle et les applications clientes, tout en s'assurant que les configurations garantissent à la fois la performance et la sécurité. Pour cela, le DBA doit :
- Connaître les composants de l'architecture Oracle.
- Comprendre l'utilité de chaque composant.

### **L'Architecture Oracle**

L'architecture Oracle repose sur deux éléments principaux : **l'instance** et **la base de données**. 

1. **L'instance Oracle** : Une instance est composée de processus et de structures mémoire qui permettent d'interagir avec les données de la base de données.
   - **Processus** : Ces processus permettent l'accès concurrent aux données et assurent la gestion des transactions.
   - **Fichiers** : Oracle utilise des fichiers pour stocker les données de manière permanente.
   - **Structures mémoire** : Des zones mémoire sont allouées pour améliorer les performances de traitement des données.

2. **La base de données** : Représente les fichiers physiques qui contiennent les données de manière persistante. Oracle garantit que ces données restent disponibles même en cas de panne.

### **Structures Mémoire de l'Instance Oracle**

La principale structure mémoire de l'instance Oracle est la **System Global Area (SGA)**, qui regroupe plusieurs sous-structures utilisées pour optimiser les opérations de lecture et d'écriture dans la base de données.

#### **Composition de la SGA** :
- **Database Buffer Cache** (50%) : Cette zone contient les blocs de données fréquemment utilisés, ce qui permet de réduire les temps d'accès aux données.
- **Shared Pool** (40%) : Contient des informations partagées telles que les instructions SQL et PL/SQL compilées ainsi que le dictionnaire de données.
- **Redo Log Buffer** (10%) : Stocke les informations sur les transactions en cours pour permettre la récupération en cas de panne.

La vue **V$SGASTAT** permet de consulter l'état et la taille des différentes structures de la SGA.

### **Le Shared Pool**

Le **Shared Pool** est une zone clé de la SGA qui permet d'améliorer les performances en réduisant le temps d'exécution des requêtes SQL et PL/SQL.

#### **Composition du Shared Pool** :
- **Dictionary Cache** : Stocke les objets du dictionnaire de données (ex : tables, index).
- **Library Cache** : Contient les instructions SQL et PL/SQL partagées et analysées.

Les vues **V$LIBRARYCACHE** et **V$SQLAREA** permettent de consulter les instructions stockées dans le Library Cache.

### **Le Database Buffer Cache**

Le **Database Buffer Cache** est utilisé pour stocker temporairement les blocs de données, y compris les blocs de tables, d'index, de rollback (ou undo), et de données temporaires. L'objectif est de minimiser les accès aux disques, qui sont plus lents que la mémoire.

Le Database Buffer Cache fonctionne selon des **algorithmes de gestion de la mémoire** :
- **LRU (Least Recently Used)** : Les blocs de données les moins récemment utilisés sont remplacés pour faire de la place aux nouveaux blocs.
- **MRU (Most Recently Used)** et **LFU (Least Frequently Used)** : D'autres algorithmes qui optimisent l'usage des blocs en fonction de la fréquence ou de la récence des accès.

Le calcul du **cache hit ratio** permet de mesurer l'efficacité de ce cache.

### **Le Redo Log Buffer**

Le **Redo Log Buffer** contient les informations de journalisation des transactions. Chaque transaction est enregistrée dans ce buffer pour permettre la restauration en cas de panne. Les vues **V$LOG** et **V$LOG_HISTORY** fournissent des informations sur les journaux de transactions.

### **Les Vues du Méta-schéma**

Le méta-schéma d'Oracle permet de surveiller les performances et l'état de la base de données via des vues dynamiques et statiques :
- **Vues statiques** : Informations sur les objets de la base de données.
- **Vues dynamiques** : Informations sur l'activité en temps réel de l'instance, comme les processus ou les sessions.

Les vues **V$SESSION** et **V$PROCESS** permettent de surveiller les utilisateurs et les processus en cours dans la base de données.

### **Les Vues Utiles pour le DBA**

Le DBA peut utiliser les vues suivantes pour surveiller et optimiser la base de données :
- **V$SQLAREA** : Contient les plans d'exécution SQL pour analyser et optimiser les requêtes.
- **V$LIBRARYCACHE** : Permet de surveiller l'utilisation du Library Cache.
- **V$LOG** et **V$LOG_HISTORY** : Informations sur la journalisation des transactions pour garantir la récupération des données.

### **Gestion de la Concurrence et Sécurité**

Oracle permet de gérer plusieurs utilisateurs accédant simultanément aux données grâce à des mécanismes de contrôle de la concurrence (ex : locks, transactions). Le système garantit également la sécurité des données grâce à des contrôles d'accès basés sur les rôles et les privilèges des utilisateurs.

### **Les Processus dans Oracle**

Oracle repose sur une architecture orientée processus. Plusieurs processus sont indispensables pour assurer le bon fonctionnement d'une instance Oracle, en plus des processus optionnels.

#### **Processus Obligatoires**

Il existe **quatre processus obligatoires** pour exécuter une instance Oracle :

1. **PMON** (Process Monitor) : Surveille et nettoie les connexions interrompues ou les processus terminés de manière anormale. Il libère les verrous et les ressources allouées par les processus en erreur.
2. **SMON** (System Monitor) : Gère la restauration automatique de l'instance et récupère l'espace occupé par des segments temporaires inutilisés. Il fusionne également les espaces libres dans les fichiers de données.
3. **DBWR** (Database Writer) : Ce processus écrit les tampons modifiés depuis la mémoire (SGA) vers les fichiers de données. Il utilise l'algorithme **LRU** (Least Recently Used) pour garder en mémoire les blocs de données les plus récents.
4. **LGWR** (Log Writer) : Il écrit les entrées du **Redo Log Buffer** dans les fichiers redo log. Ce processus est déclenché lors d'un commit, lorsque le buffer redo log est rempli à un tiers, lors d'un checkpoint, ou lorsqu'un time-out se produit.

Si l'un de ces processus échoue, l'instance Oracle sera arrêtée et devra être redémarrée.

#### **Processus Optionnels**

En plus des processus obligatoires, il existe des processus optionnels tels que :

- **CKPT** (Checkpoint) : Ce processus marque un point de contrôle dans les journaux redo log pour garantir la cohérence de la base.
- **ARCH** (Archiver) : Ce processus sauvegarde les fichiers redo log archivés pour garantir la récupération après une panne.

### **Les Processus Utilisateurs et Serveurs**

1. **Processus Utilisateurs** : Ces processus représentent les outils ou applications clients (par exemple SQL*Plus, Oracle Forms) qui envoient des requêtes SQL au serveur Oracle et reçoivent les résultats. Ils exécutent les commandes SQL au niveau du client.
    
2. **Processus Serveurs** : Ces processus sont responsables de l'analyse et de l'exécution des commandes SQL reçues. Ils accèdent aux blocs de données stockés sur le disque et les chargent dans la mémoire (SGA) pour traitement. Le processus serveur envoie ensuite les résultats des requêtes SQL au processus utilisateur.
    

Dans une **architecture dédiée**, chaque processus utilisateur dispose d'un processus serveur dédié, tandis que dans une **architecture partagée**, plusieurs processus utilisateurs partagent un même processus serveur.

### **Organisation du Stockage Oracle**

#### **Structure Physique**

La base de données Oracle repose sur plusieurs fichiers pour stocker les données de manière persistante :

- **Fichiers de contrôle** : Contiennent des informations vitales sur la structure de la base de données, comme les emplacements des fichiers de données et les checkpoints.
- **Fichiers de données** : Stockent les blocs de données, les segments d'index, et autres informations utilisateur.
- **Fichiers Redo Log** : Contiennent les journaux de transactions, nécessaires pour la récupération en cas de panne.
- **Fichiers de paramètres** : Contiennent les configurations de l'instance Oracle.
- **Fichiers de trace** : Utilisés pour surveiller et diagnostiquer les problèmes rencontrés par l'instance.

#### **Structure Logique**

L'organisation logique des données dans Oracle se divise en plusieurs composants :

1. **Tablespaces** : La base de données est divisée en plusieurs tablespaces, qui contiennent des segments logiques comme les tables, index, ou segments de rollback. Chaque tablespace est composé de blocs de données et de fichiers de données.
    - **Tablespace SYSTEM** : Indispensable pour le fonctionnement de la base de données, il contient les informations du dictionnaire de données, les définitions des procédures stockées, des triggers, etc.
    - **Tablespaces d'utilisateur** : Destinés aux données utilisateur (tables, index, etc.).
    - **Tablespace TEMP** : Utilisé pour stocker des segments temporaires lors des opérations complexes comme les tris ou les joints.
    - **Tablespace RBS** : Contient les segments de rollback (ou undo), utilisés pour gérer les transactions et garantir la cohérence des données en cas d'échec d'une transaction.

Les tablespaces permettent de segmenter et de gérer de manière flexible les différents types de données au sein d'une base de données Oracle. Cela offre également une grande flexibilité pour l'administration, en isolant les objets spécifiques dans des segments distincts.

### **Résumé de l'Organisation Physique et Logique**

L'organisation **physique** de la base de données Oracle est composée de fichiers persistants (fichiers de contrôle, fichiers de données, fichiers de journalisation), tandis que l'organisation **logique** divise ces données en segments logiques tels que des tablespaces, qui eux-mêmes contiennent des tables, des index, des segments temporaires et des segments de rollback.

### **Traitement d'une Requête dans Oracle**

Le traitement d'une requête SQL dans Oracle suit trois étapes clés : **Parse**, **Execute**, et **Fetch** (pour les requêtes de type `SELECT`).

1. **Parse** : Cette étape décompose et analyse la requête SQL pour vérifier sa syntaxe et sa structure. Le parse peut être de deux types :
    
    - **Soft Parse** : Oracle réutilise un plan d'exécution déjà en cache, réduisant ainsi les temps de traitement.
    - **Hard Parse** : Oracle doit analyser la requête de bout en bout, y compris la création d'un nouveau plan d'exécution, ce qui est plus coûteux en ressources.
    
    Le **Library Cache** joue un rôle essentiel lors du parse, car il stocke les plans d'exécution SQL déjà analysés pour accélérer les prochaines exécutions.
    
2. **Execute** : Durant cette phase, Oracle exécute la requête SQL, accède aux données stockées dans les fichiers de la base, et réalise les modifications ou récupérations demandées.
    
3. **Fetch** : Pour les requêtes `SELECT`, cette étape consiste à récupérer et à renvoyer les lignes de données obtenues en mémoire (SGA) pour les envoyer au processus utilisateur.
    

---

### **Création d'une Base Oracle**

Le rôle principal du **DBA (Administrateur de Base de Données)** est de créer et configurer les bases de données en fonction des besoins spécifiques de l'entreprise. Voici les composants essentiels à la création d'une base Oracle :

1. **Fichiers de paramètres** : Ils contiennent les configurations nécessaires pour l'instance Oracle, y compris les tailles mémoire, les chemins des fichiers, et d'autres options de démarrage.
2. **Fichiers de données** : Ils stockent les données réelles de la base, incluant les tables et index.
3. **Fichiers de contrôle** : Ces fichiers contiennent des informations critiques sur la structure de la base de données, le nom de la base, et les informations nécessaires à la récupération après une panne.
4. **Fichiers Redo Log** : Ces fichiers journalisent toutes les modifications effectuées sur la base de données et sont essentiels pour la restauration des données en cas d'incident.
5. **Fichiers d'alerte** : Ce sont des journaux chronologiques qui enregistrent les messages système et les erreurs importantes rencontrées par l'instance Oracle.
6. **Fichiers de trace** : Ils contiennent des informations détaillées sur les erreurs internes détectées par les processus serveurs, permettant d'investiguer les incidents.

#### **Processus de Démarrage d'une Base Oracle**

Le démarrage d'une base Oracle s'effectue en trois phases :

1. **Création de la base** : Le DBA définit les fichiers et paramètres nécessaires.
2. **Démarrage de l'instance** : Oracle initialise les structures mémoire (SGA) et les processus obligatoires.
3. **Montage et Ouverture de la base** : La base est montée (rendue accessible aux administrateurs) puis ouverte (accessible aux utilisateurs autorisés).

---

### **Création et Gestion des Utilisateurs**

Une fois la base Oracle en place, le DBA doit créer et gérer les utilisateurs, ainsi que leurs droits d'accès. Voici les étapes et les considérations lors de la gestion des utilisateurs :

1. **Allouer des Privilèges** : Le DBA attribue des privilèges aux utilisateurs en fonction de leurs besoins. Les privilèges peuvent concerner :
    
    - **Droits d'exécution SQL** : Permet l'exécution de requêtes SQL spécifiques.
    - **Ressources** : Gère l'accès aux ressources comme les sessions, le temps CPU, et la taille mémoire allouée.
2. **Création de Rôles** : Un rôle est un regroupement de privilèges que le DBA peut attribuer à un utilisateur ou un groupe d'utilisateurs.
    
3. **Problèmes de Sécurité** : Si les données sont sensibles, le DBA doit s'assurer que les utilisateurs non autorisés ne puissent pas y accéder. Cela inclut la configuration des privilèges et le chiffrement des données.
    

---

### **Gestion des Schémas et des Données**

Le **Data Manager** ou administrateur de données a pour rôle de gérer les schémas des utilisateurs et les données stockées dans la base Oracle. Ses principales tâches incluent :

1. **Création de Schémas** : Le schéma représente une collection logique d'objets de base de données appartenant à un utilisateur, tels que les tables, vues, index, et contraintes.
    
2. **Gestion des Objets du Schéma** :
    
    - **Tables et Vues** : Stockent les données et permettent de les consulter.
    - **Contraintes** : Garantissent l'intégrité des données (par exemple, les clés primaires et étrangères).
    - **Triggers** : Déclenchent des actions automatiques en réponse à des événements (insertion, mise à jour, suppression).
    - **Procédures et Packages** : Regroupent des procédures stockées et des fonctions pour faciliter la gestion et la réutilisation du code.
3. **Allouer des Droits** : Le Data Manager doit également gérer les permissions des utilisateurs sur les objets du schéma. Par exemple, un utilisateur peut être autorisé à consulter une table, mais pas à la modifier.
    
4. **Vérification de la Cohérence des Données** : L'administrateur doit s'assurer que les données restent cohérentes et qu'aucune violation des règles d'intégrité n'a lieu.