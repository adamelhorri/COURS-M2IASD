---
tags:
  - AdminBDD
  - tps
---
### **Une Histoire de TP : Lez et Marta dans le Monde des Index et Arbres B+**

Dans une petite salle de classe de l’université, Lez et Marta, deux étudiants en informatique, s’apprêtent à commencer leur TP sur les bases de données. Lez, toujours enthousiaste, adore se lancer à toute vitesse dans les exercices, tandis que Marta, plus posée, préfère prendre son temps et vérifier chaque étape. Aujourd’hui, leur TP porte sur la gestion des index, des arbres B+ et des structures de données complexes. Ensemble, ils vont parcourir ce chemin en se corrigeant et en apprenant à chaque étape.

#### **Étape 1 : La Création des Tables**

Tout commence par la création de tables. Lez s’assoit à son poste et ouvre son éditeur SQL.

"Ok, Marta, je commence avec la table **COMMUNE**," dit-il avec confiance, en tapant rapidement sur son clavier.

Il crée une table avec les colonnes **CODEINSEE** et **NUMDEP** et ajoute une **clé primaire** sur **CODEINSEE**. Il est certain que tout est parfait, jusqu’à ce que Marta, qui jette un œil sur son écran, remarque quelque chose.

"Lez, tu n’as pas ajouté la contrainte de clé primaire correctement. Il te manque la partie **CONSTRAINT** pour nommer la clé. Et aussi, n’oublie pas de créer la table **DEPARTEMENT** avec une **clé étrangère** pour relier les deux," explique-t-elle calmement.

Lez grogne un peu, mais il sait qu’elle a raison. Il corrige rapidement son erreur.

```sql
CREATE TABLE COMMUNE (
    CODEINSEE VARCHAR2(10),
    NUMDEP NUMBER,
    PRIMARY KEY (CODEINSEE) CONSTRAINT COMMUNE_PK
);

CREATE TABLE DEPARTEMENT (
    NUMDEP NUMBER PRIMARY KEY
);

ALTER TABLE COMMUNE 
ADD CONSTRAINT FK_NUMDEP 
FOREIGN KEY (NUMDEP) 
REFERENCES DEPARTEMENT(NUMDEP);
```

"Merci, Marta," dit Lez en appuyant sur le bouton pour exécuter le code. "Ça marche !"

#### **Étape 2 : Création d’un Index Non Unique**

Les deux amis passent ensuite à la création d’un **index non unique** sur la colonne **NUMDEP**. Lez commence à écrire la commande SQL, mais cette fois, il est un peu trop confiant et ne prête pas attention aux détails.

"Je vais ajouter l’index sur NUMDEP pour rendre les jointures plus rapides. Regarde !" s’exclame Lez en tapant :

```sql
CREATE INDEX numdep_idx ON COMMUNE(NUMDEP);
```

Marta hoche la tête avec approbation. "Cette fois, tu as bien fait," dit-elle. "Ça va vraiment aider à optimiser les recherches."

Lez est content de lui, mais il sait que les choses vont se compliquer rapidement.

#### **Étape 3 : Consulter les Statistiques des Index**

Marta prend la main pour la suite. "Lez, avant de continuer, il faut consulter les statistiques des index pour savoir si tout est bien en ordre," dit-elle.

Lez la regarde, un peu perdu. "Qu’est-ce que tu veux dire par 'statistiques des index' ?"

"Tu dois vérifier la **hauteur** de l'index et voir combien d’espace il occupe. C’est important de garder un œil sur ces détails pour s’assurer que tout fonctionne bien."

Marta lui montre comment mettre à jour et consulter les statistiques de l’index. Ensemble, ils exécutent les commandes suivantes :

```sql
ANALYZE INDEX COMMUNE_PK VALIDATE STRUCTURE;

SELECT name, btree_space, most_repeated_key, lf_rows, br_rows, height 
FROM INDEX_STATS 
WHERE name = 'COMMUNE_PK';
```

"Regarde, Lez, la hauteur de l’arbre est équilibrée. Ça veut dire que l’index est optimisé pour des recherches rapides," explique Marta.

Lez hoche la tête, impressionné. "Ok, je comprends maintenant pourquoi c’est important."

#### **Étape 4 : Calcul du Facteur de Blocage**

Ils arrivent maintenant à une partie un peu plus technique : le calcul du **facteur de blocage**. Marta prend les devants cette fois, expliquant chaque détail à Lez.

"Lez, imagine que chaque bloc de données est une boîte, et chaque enregistrement est un objet. Le facteur de blocage nous dit combien d'objets peuvent tenir dans chaque boîte," commence Marta.

"Ah, je vois... donc on doit savoir combien d’enregistrements tiennent dans un bloc ?" demande Lez.

"Exactement. On va calculer ça en fonction de la taille moyenne d’un enregistrement."

Marta lui montre comment récupérer la taille moyenne des enregistrements dans la table COMMUNE.

```sql
SELECT blocks, avg_row_len 
FROM USER_TABLES 
WHERE table_name = 'COMMUNE';
```

Lez regarde les résultats. "Alors, on peut mettre environ 70 enregistrements par bloc. C’est ça le facteur de blocage ?"

"Oui, exactement," répond Marta, ravie de voir Lez comprendre. "Maintenant, il faut s’assurer que la taille des blocs est utilisée de manière optimale."

#### **Étape 5 : Manipuler les ROWIDs**

Les choses se compliquent encore un peu plus lorsque Lez doit maintenant manipuler les **ROWIDs**. Marta le laisse faire cette fois, pour voir comment il s’en sort. Lez écrit une procédure pour récupérer tous les enregistrements dans le même bloc que celui d’un enregistrement donné.

Il tape rapidement, mais lorsqu'il exécute son code, une erreur apparaît.

"Argh ! Pourquoi ça ne marche pas ?!" s’exclame Lez, frustré.

Marta regarde son écran et rit doucement. "Tu as oublié d’utiliser la fonction **DBMS_ROWID** pour récupérer le numéro de bloc. Laisse-moi te montrer."

Elle corrige son code :

```plsql
CREATE OR REPLACE PROCEDURE MEMEBLOCQUE(p_codeINSEE IN VARCHAR2) AS
    row_id ROWID;
BEGIN
    SELECT ROWID INTO row_id FROM COMMUNE WHERE codeINSEE = p_codeINSEE;

    FOR r IN (
        SELECT codeINSEE, nomcommaj 
        FROM COMMUNE 
        WHERE DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) = DBMS_ROWID.ROWID_BLOCK_NUMBER(row_id)
    ) LOOP
        DBMS_OUTPUT.PUT_LINE(r.codeINSEE || ' ' || r.nomcommaj);
    END LOOP;
END;
```

Lez soupire de soulagement lorsque le code fonctionne enfin. "Merci, Marta, sans toi, j’aurais été bloqué ici toute la journée."

#### **Étape 6 : Gestion des Arbres B-Tree**

Le TP avance bien, et maintenant, ils doivent gérer les **arbres B-Tree**. Lez, toujours enthousiaste, tente de créer un **index unique** sur la colonne **code_insee** de la table COMMUNE. Cependant, il oublie un petit détail crucial.

"Pourquoi est-ce que mon index ne fonctionne pas après la création ?" demande-t-il, confus.

Marta jette un coup d’œil à son écran. "Tu as désactivé ton index sans le reconstruire après. Si tu ne le reconstruis pas, il ne sera plus utilisé par la base de données."

Lez corrige rapidement son erreur, suivant les conseils de Marta.

```sql
ALTER INDEX commune_pk REBUILD;
```

Une fois l'index reconstruit, Lez voit que tout fonctionne comme prévu.

"Ça marche ! Je suis vraiment en train de comprendre, grâce à toi," dit-il, reconnaissant.

#### **Étape 7 : Index Bitmap**

Ils arrivent maintenant aux **index bitmap**, un concept que Lez ne connaît pas très bien. Il lit rapidement l’énoncé du TP et se lance dans la création d’un index bitmap sur la colonne **fonction** de la table EMP.

"Je pense que c’est comme un index normal, non ?" demande-t-il en tapant rapidement la commande.

Marta le corrige doucement. "Pas exactement, un index bitmap est utilisé lorsque la colonne a un nombre limité de valeurs différentes, comme **genre** ou **fonction**. Il est plus efficace pour ce genre de données."

Elle lui montre comment faire correctement :

```sql
CREATE BITMAP INDEX fonction_idx ON EMP(fonction);
```

Lez lance la commande, et tout fonctionne correctement. "Wow, ça a l'air vraiment utile pour ce genre de données !"

#### **Étape 8 : Index de Hachage**

Enfin, ils arrivent à la dernière partie : les **index de hachage**. Lez a un peu de mal à comprendre comment fonctionnent ces index, mais Marta est là pour le guider.

"Les index de hachage sont utilisés pour des recherches précises et rapides sur des valeurs exactes, comme un identifiant unique," explique-t-elle.

Lez tente de créer un cluster avec un index de hachage, mais fait une petite erreur de syntaxe.

"Regarde bien ton code," dit Marta. "Tu n’as pas défini correctement la taille du cluster et le nombre de clés de hachage. Il faut les ajuster."

Ensemble, ils corrigent le code.

```sql
CREATE CLUSTER emp_cluster (num NUMBER(5,0)) 
    SIZE 500 
    HASHKEYS 1500;

CREATE TABLE employe (
    num NUMBER(5,0) PRIMARY KEY,
    name VARCHAR(15)
) CLUSTER emp_cluster (num);
```

Lez pousse un soupir

 de soulagement lorsque tout fonctionne enfin.

#### **Étape 9 : Questions Théoriques sur les Arbres B+**

Ils arrivent à la dernière partie du TP, où ils doivent répondre à des questions théoriques sur les **arbres B+**. Marta, toujours méthodique, commence à expliquer à Lez comment calculer le **coût d'accès** pour récupérer un enregistrement dans une table.

"Lez, tu dois comprendre que chaque niveau d’un arbre B+ représente un coût en termes de blocs à parcourir. Le coût total dépend de la hauteur de l’arbre," explique-t-elle.

Lez fait quelques calculs rapides avec son aide, simulant l’ajout de nouveaux enregistrements pour voir quand un **nouveau niveau** serait nécessaire dans l’arbre B+.

"Wow, tout ce travail m’a vraiment ouvert les yeux," dit Lez, impressionné par tout ce qu'il vient d'apprendre.

#### **Conclusion : Une Leçon sur la Collaboration et l'Apprentissage**

Après plusieurs heures de travail acharné, Lez et Marta finissent leur TP, satisfaits. Bien que Lez ait fait plusieurs erreurs en cours de route, Marta était toujours là pour l’aider et le guider. Grâce à elle, il a non seulement corrigé ses erreurs, mais aussi compris les concepts plus en profondeur.

"Tu sais, Marta, je n’aurais jamais pu y arriver sans toi. J’ai vraiment appris plein de choses aujourd’hui," dit Lez en souriant.

"Moi aussi, j’ai appris en t’aidant," répond Marta avec un clin d’œil.

Ensemble, ils quittent la salle de classe, fiers de leur travail et prêts à relever de nouveaux défis.