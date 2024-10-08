---
tags:
  - NLP
  - tps
---

Bienvenue ! Ce guide est conçu pour t'expliquer les **grammaires AB** de manière simple et claire, sans nécessiter de connaissances préalables. Imagine que tu es un constructeur de phrases, et que chaque mot est une pièce de puzzle que tu dois assembler pour créer des phrases correctes. Allons-y !

---

## Table des Matières

1. [Introduction aux Grammaires AB](#1-introduction-aux-grammaires-ab)
2. [Catégories Grammaticales de Base](#2-catégories-grammaticales-de-base)
3. [Catégories Fonctionnelles](#3-catégories-fonctionnelles)
4. [Formules de Catégories pour les Mots](#4-formules-de-catégories-pour-les-mots)
5. [Règles de Combinaison des Mots](#5-règles-de-combinaison-des-mots)
6. [Arbres de Dérivation](#6-arbres-de-dérivation)
7. [Exemples Pratiques](#7-exemples-pratiques)
8. [Exercices pour S'entraîner](#8-exercices-pour-sentraînement)
9. [Résumé des Concepts Clés](#9-résumé-des-concepts-clés)
10. [Conclusion](#10-conclusion)

---

## 1. Introduction aux Grammaires AB

### Qu'est-ce qu'une Grammaire AB ?

Une **grammaire AB** est un système utilisé en linguistique pour analyser la structure des phrases. Elle nous aide à comprendre comment les mots se combinent pour former des phrases correctes. Pense-y comme un guide qui te dit quelles pièces de puzzle (mots) vont ensemble et comment les assembler.

### Pourquoi les Grammaires AB sont-elles Importantes ?

- **Compréhension** : Elles permettent de mieux comprendre la structure des phrases.
- **Création** : Elles aident à construire des phrases correctes.
- **Analyse** : Elles facilitent l'analyse grammaticale des textes.

---

## 2. Catégories Grammaticales de Base

Les catégories grammaticales sont comme les types de blocs dans un jeu de construction. Chaque mot appartient à une catégorie spécifique qui détermine comment il peut se combiner avec d'autres mots.

### a. **n (Nom)**

- **Définition** : Représente un nom, c'est-à-dire une personne, un lieu, une chose ou une idée.
- **Exemples** : *rabbit* (lapin), *hatter* (chapelier), *queen* (reine), *song* (chanson), *gryphon* (griffon).

### b. **np (Syntagme Nominal)**

- **Définition** : Un groupe de mots qui fonctionne comme un nom dans une phrase. Cela peut inclure des déterminants, des adjectifs et des noms.
- **Exemples** : *Alice*, *Tweedledum*, *Tweedledee*, *She* (elle), *her* (la, lui).

### c. **s (Phrase)**

- **Définition** : Une proposition complète qui a un sujet et un prédicat.
- **Exemple** : *Alice dreams.* (Alice rêve.)

### d. **adj (Adjectif)**

- **Définition** : Un mot qui décrit ou modifie un nom.
- **Exemples** : *slow* (lent), *Mad* (fou), *Red* (rouge).

---

## 3. Catégories Fonctionnelles

Ces catégories indiquent comment un mot attend un autre mot pour former une unité plus grande. Pense à cela comme une règle qui dit quel type de bloc peut se connecter à quel autre.

### a. **A/B et A\B**

- **A/B** : Une fonction qui attend un élément de catégorie **B** à sa **droite** pour former un élément de catégorie **A**.
  - **Exemple** : *(s \ np) / np* signifie que le mot attend un *np* (syntagme nominal) à droite pour former *(s \ np)* (une phrase manquant un sujet).

- **A\B** : Une fonction qui attend un élément de catégorie **B** à sa **gauche** pour former un élément de catégorie **A**.
  - **Exemple** : *s \ np* signifie que le mot attend un *np* (syntagme nominal) à gauche pour former une phrase complète *s*.

### b. **Exemples de Catégories Fonctionnelles**

- **Verbe Intransitif (ex. *dreams*)** : *s \ np*
  - Attends un sujet (*np*) à gauche pour former une phrase (*s*).
  
- **Verbe Transitif (ex. *likes*)** : *(s \ np) / np*
  - Attends un objet (*np*) à droite pour former un prédicat (*s \ np*).

- **Déterminant (ex. *the*)** : *np / n*
  - Attends un nom (*n*) à droite pour former un syntagme nominal (*np*).

- **Préposition (ex. *of*)** : *(n \ n) / np*
  - Attends un *np* à droite pour former un modificateur de nom *(n \ n)*.

- **Pronom Relatif (ex. *who*)** : *(n \ n) / (s \ np)*
  - Attends une phrase relative manquant un sujet (*s \ np*) pour former un modificateur de nom *(n \ n)*.

---

## 4. Formules de Catégories pour les Mots

Chaque mot se voit attribuer une **formule de catégorie** qui décrit sa fonction grammaticale et comment il se combine avec d'autres mots.

### a. **Exemples de Formules**

- **Alice** : *np*
- **dreams** : *s \ np*
- **likes** : *(s \ np) / np*
- **the** : *np / n*
- **of** : *(n \ n) / np*
- **who** : *(n \ n) / (s \ np)*
- **she** : *np*
- **her** : *np*
- **teases** : *(s \ np) / np*

### b. **Interprétation des Formules**

- ***(s \ np) / np*** : Un verbe transitif comme *likes* attend un objet (*np*) à droite pour former un prédicat (*s \ np*).
- ***(n \ n) / np*** : Une préposition comme *of* attend un *np* à droite pour modifier un nom en formant *(n \ n)*.

---

## 5. Règles de Combinaison des Mots

Les mots se combinent en utilisant des **règles d'application** pour former des phrases complètes. Il existe deux types principaux d'application :

### a. **Application Avant ( / )**

- **Définition** : La fonction attendue par un mot est à sa **droite**.
- **Exemple** : *likes* ((s \ np) / np) + *the song of the Gryphon* (np) → *s \ np*

### b. **Application Arrière ( \ )**

- **Définition** : La fonction attendée par un mot est à sa **gauche**.
- **Exemple** : *Alice* (np) + *dreams* (s \ np) → *s*

---

## 6. Arbres de Dérivation

Les **arbres de dérivation** sont des représentations visuelles montrant comment les mots se combinent pour former des phrases. Chaque niveau de l'arbre montre une étape de la combinaison des mots selon leurs catégories.

### a. **Comment Lire un Arbre de Dérivation**

- **Nœuds** : Chaque nœud représente une catégorie grammaticale (*np*, *s \ np*, *s*, etc.).
- **Branches** : Les lignes montrent comment les catégories se combinent.

### b. **Exemple Simplifié**

Pour la phrase *Alice dreams.* :

```
      s
     / \
   np  s\np
 Alice dreams
```

- **Explication** :
  - *Alice* (np) se combine avec *dreams* (s \ np) pour former une phrase complète *s*.

---

## 7. Exemples Pratiques

Voyons quelques exemples concrets pour mieux comprendre comment tout cela fonctionne.

### Exemple 1 : **Alice dreams.**

#### Assignation des Formules

- **Alice** : *np*
- **dreams** : *s \ np*

#### Dérivation

1. **Alice** (np) + **dreams** (s \ np) → **s**

#### Arbre de Dérivation

```
      s
     / \
   np  s\np
 Alice dreams
```

### Exemple 2 : **She likes Tweedledee.**

#### Assignation des Formules

- **She** : *np*
- **likes** : *(s \ np) / np*
- **Tweedledee** : *np*

#### Dérivation

1. **likes** ((s \ np) / np) + **Tweedledee** (np) → *s \ np*
2. **She** (np) + *s \ np* → **s**

#### Arbre de Dérivation

```
     s
   /   \
 np    s\np
She   /     \
 likes  Tweedledee
```

### Exemple 3 : **The Hatter who teases Alice.**

#### Assignation des Formules

- **The** : *np / n*
- **Hatter** : *n*
- **who** : *(n \ n) / (s \ np)*
- **teases** : *(s \ np) / np*
- **Alice** : *np*

#### Dérivation

1. **teases** ((s \ np) / np) + **Alice** (np) → *s \ np*
2. **who** ((n \ n) / (s \ np)) + *s \ np* → *(n \ n)*
3. **Hatter** (n) + *(n \ n)* → *n*
4. **The** (*np / n*) + *n* → *np*

#### Arbre de Dérivation

```
          np
        /    \
    np/n      n
    The    /   \
          n      n\n
        Hatter  /     \
               (n\n)/(s\np)  s\np
                  who      /     \
                        teases  Alice
```

---

## 8. Exercices pour S'entraîner

Maintenant, c'est à ton tour ! Voici quelques phrases à analyser. Suis les étapes pour déterminer les catégories et dessiner les arbres de dérivation.

### Exercice 1 : **The rabbit never dreams.**

### Exercice 2 : **Tweedledum irritates Tweedledee.**

### Exercice 3 : **Alice likes the song of the Gryphon.**

### Exercice 4 : **Tweedledee likes her.**

**Instructions :**

1. **Assigne une formule de catégorie à chaque mot.**
2. **Combines les mots en suivant les règles d'application.**
3. **Dessine l'arbre de dérivation.**

*Essaie de résoudre ces exercices par toi-même ! Consulte les exemples précédents si tu as besoin d'aide.*

---

## 9. Résumé des Concepts Clés

- **Catégories Grammaticales** : Chaque mot appartient à une catégorie (*n*, *np*, *s*, *adj*, etc.) qui détermine comment il peut se combiner avec d'autres mots.
  
- **Fonctions Grammaticales (A/B, A\B)** : Indiquent où les mots attendent leurs compléments (à droite ou à gauche).

- **Formules de Catégories** : Représentent la catégorie d'un mot et sa fonction dans la phrase.

- **Arbres de Dérivation** : Aident à visualiser comment les mots se combinent pour former des phrases complètes.

- **Application Avant et Arrière** : Méthodes pour combiner les mots en fonction de leurs catégories et fonctions.

---

## 10. Conclusion

Les **grammaires AB** peuvent sembler complexes au début, mais en les décomposant en petits morceaux :

1. **Identifie la catégorie de chaque mot.**
2. **Comprends comment ces catégories se combinent grâce aux fonctions A/B et A\B.**
3. **Utilise des arbres de dérivation pour visualiser le processus.**

C'est comme assembler un puzzle où chaque pièce a une place spécifique ! Avec de la pratique, tu te familiariseras avec ces concepts et tu pourras analyser et construire des phrases avec aisance.

**N'hésite pas à pratiquer avec les exercices !** Plus tu t'exerces, plus cela deviendra naturel.

Bonne chance ! 😊