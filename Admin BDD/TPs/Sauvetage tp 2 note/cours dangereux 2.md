
# Transactions et Bases de Données

## Introduction

- Objectif : garantir la cohérence et la pérennité des données en présence de multiples utilisateurs effectuant des accès simultanés (concurrence), sur de gros volumes de données et/ou des données distribuées.
- Définitions :
    1. **Transaction** : séquence cohérente d’actions, appliquée à une BD cohérente pour produire une BD cohérente.
    2. **Accès concurrent** : accès simultanés à la BD (table, tuple...) pouvant créer des conflits.
    3. **Session** : période pendant laquelle un client communique avec le serveur de données (série de transactions).
    4. **Connexion** : ouverture de la session, souvent associée à une authentification.

### Illustration : Accès Concurrents

Des clients (programmes Java, PHP, interpréteurs SQL...) accèdent à un serveur de BD gérant des données partagées. La BD doit garantir cohérence et intégrité malgré l’accès concurrent.

### Domaines cibles

- Systèmes bancaires (transferts monétaires)
- Réservations (train, avion, hôtel...)
- Centrales d’achat
- Systèmes de santé, etc.

## Transaction

### Définition

Une transaction est une unité de traitement séquentiel (séquence d’actions), exécutée pour le compte d’un usager, transformant un état cohérent de la BD en un autre état cohérent.

- Début de transaction : implicite (au premier ordre SQL ou après la fin de la transaction précédente) ou explicite (BEGIN, SET TRANSACTION...).
- Fin de transaction : explicite via COMMIT (valider) ou ROLLBACK (annuler), ou implicite (ex. exécution d’une commande DDL comme CREATE/ALTER/DROP valide automatiquement la transaction en cours).
- En cas d’arrêt normal ou déconnexion : COMMIT implicite.
- En cas d’arrêt anormal : ROLLBACK implicite.

#### Exemple

```sql
CREATE TABLE Compte (numC INTEGER PRIMARY KEY, typeC VARCHAR2(10), solde FLOAT);
INSERT INTO Compte VALUES (2, 'courant', -200);
INSERT INTO Compte VALUES (3, 'courant', 500);
INSERT INTO Compte VALUES (4, 'courant', 100.50);
COMMIT;

UPDATE Compte SET solde = solde + 100 WHERE numC=3;
ROLLBACK;

DELETE FROM Compte WHERE numC=3;
ALTER TABLE Compte ADD numAg INTEGER;
```

Compter les transactions (début-fin) pour comprendre leur délimitation.

### Propriétés ACID d’une Transaction

- **Atomicité** : Toutes les actions de la transaction s’exécutent ou aucune.
- **Cohérence** : Les modifications respectent les contraintes d’intégrité.
- **Isolation** : Les transactions s’exécutent comme si elles étaient seules (pas de contamination par des données non validées d’autres transactions).
- **Durabilité (Permanence)** : Les effets d’une transaction validée persistent malgré les pannes.

### Atomicité et Validation

- La transaction commence, effectue des actions (reads/writes).
- Au COMMIT, on calcule la validité (certification). Si OK, la transaction est validée.
- Le COMMIT marque le « point de validation ».
- Si problème : ROLLBACK (annulation).

### Isolation

Les transactions concurrentes doivent être ordonnancées (entrelacées) de façon à éviter les incohérences. L’ordonnancement résultant doit être équivalent à une exécution séquentielle (sérialisable).

#### Problèmes d’entrelacement

- Perte de mise à jour
- Incohérences globales
- Lectures impropres (données non validées)
- Lectures non reproductibles
- Lectures de fantômes (nouvelles données apparaissant entre deux lectures)

#### Sérialisabilité

Une exécution est sérialisable si équivalente à une exécution séquentielle. On peut garantir la sérialisabilité via verrous.

### Verrous (Locks)

- Verrouillage (lock) et déverrouillage (unlock) assurent l’isolation.
- Inconvénients : attente, famine, interblocage (deadlock).
- Granularité du verrou : BD, table, bloc, tuple, attribut.
- Verrous explicites en SQL :
    
    ```sql
    SELECT * FROM Compte WHERE num_compte=12345 FOR UPDATE;
    LOCK TABLE Compte IN EXCLUSIVE MODE;
    ```
    

### Types de Verrous

- S (partagé) : lecture
- X (exclusif) : écriture

Matrice de compatibilité :

||S|X|
|---|---|---|
|S|Y|N|
|X|N|N|

### Niveaux d’Isolation

Quatre niveaux définis par SQL-92 :

- READ UNCOMMITTED : dirty reads autorisées
- READ COMMITTED : pas de dirty read, mais non-repeatable read et phantom possibles
- REPEATABLE READ : pas de dirty read, pas de non-repeatable read, phantom possible
- SERIALIZABLE : pas de dirty read, pas de non-repeatable read, pas de phantom

En Oracle :

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### Points de reprise (SAVEPOINT)

Possibilité de mettre des points de restauration intermédiaires :

```sql
UPDATE Compte SET solde=100 WHERE numC=2;
SAVEPOINT Compte_2;
UPDATE Compte SET solde=-1000 WHERE numC=4;
ROLLBACK TO SAVEPOINT Compte_2;
```

### Durabilité

Les modifications validées sont enregistrées sur disque (avec journaux de redo log, undo segments) pour survivre aux pannes. Stratégie « Refaire - Défaire » pour récupérer après panne.

## Privilèges

Les privilèges contrôlent qui peut faire quoi sur la BD.

### Privilèges Objets (table, vue, fonction...)

- GRANT pour accorder des privilèges :
    
    ```sql
    GRANT INSERT, UPDATE ON Compte TO user1;
    GRANT ALL ON Compte TO public;
    GRANT EXECUTE ON f_transfert TO public;
    ```
    
- Possibilité de donner des privilèges sur certaines colonnes :
    
    ```sql
    GRANT UPDATE(solde) ON Compte TO user3;
    ```
    
- REVOKE pour retirer des privilèges :
    
    ```sql
    REVOKE INSERT, UPDATE ON Compte FROM user1;
    ```
    

### Privilèges Systèmes

- user_sys_privs, user_role_privs, dba_roles pour inspecter les privilèges système.
- Exemples :
    
    ```sql
    SELECT * FROM user_sys_privs;
    CREATE ROLE M1_IC;
    GRANT CREATE PUBLIC DATABASE LINK TO M1_IC;
    GRANT M1_IC TO user1;
    ```
    

## Conclusion

Ce cours présente :

- Les principes fondamentaux des transactions et des propriétés ACID.
- Les problématiques de concurrence, les niveaux d’isolation, l’usage des verrous et les interblocages.
- Les mécanismes de durabilité (journalisation).
- Les privilèges d’accès aux objets et les privilèges système, la création de rôles, la gestion des droits sur les objets.

L’ensemble vise à garantir l’intégrité, la cohérence et la sécurité des données dans des environnements multi-utilisateurs.

---
