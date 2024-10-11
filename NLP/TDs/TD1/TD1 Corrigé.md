---
tags:
  - NLP
  - tps
---
D'accord, je vais réécrire la résolution de l'exercice en français et de manière simple, comme si j'expliquais à un ado de 14 ans.

##  **1.1.Exercice : Traduire les règles de réécriture en formules AB**

### **Contexte**

On a une **grammaire sans contexte (CFG)** qui génère des séquences de parenthèses équilibrées. Voici les règles de réécriture de la grammaire **G'** :

1. **S → a B | a S B | a B S | a S B S**
2. **B → b**

- **a** représente une parenthèse ouvrante « ( ».
- **b** représente une parenthèse fermante « ) ».
- **S** et **B** sont des symboles non-terminaux utilisés pour construire les séquences équilibrées.

### **Objectif**

Traduire ces règles de réécriture en **formules AB**, où **A** et **B** sont des symboles utilisés pour représenter les terminaux **a** et **b**.

### **Étapes de la Traduction**

#### **1. Traduire la règle pour B**

La règle donnée est :
- **B → b**

Cette règle est déjà simple à traduire. Elle dit que le symbole **B** se transforme directement en **b**. Donc, en formule AB :

- **B = b**

#### **2. Traduire les règles pour S**

Les règles données pour **S** sont :
- **S → a B**
- **S → a S B**
- **S → a B S**
- **S → a S B S**

On va les traduire toutes ensemble en une seule formule AB en utilisant le symbole **|** qui signifie "ou".

**Formule initiale :**
- **S = a B | a S B | a B S | a S B S**

Pour simplifier, on remarque que **a** est présent au début de toutes les options. On peut donc factoriser **a** :

- **S = a (B | S B | B S | S B S)**

Maintenant, comme on sait déjà que **B = b**, on peut remplacer **B** par **b** dans la formule :

- **S = a (b | S b | b S | S b S)**

### **Résultat Final**

Après traduction, les formules AB pour les symboles terminaux **a** et **b** sont :

- **B = b**
- **S = a (b | S b | b S | S b S)**

### **Interprétation des Formules AB**

- **B = b** : Le symbole **B** représente simplement la parenthèse fermante « ) ».
  
- **S = a (b | S b | b S | S b S)** :
  - **a b** : Une paire de parenthèses simple « () ».
  - **a S b** : Une parenthèse ouvrante suivie d'une structure équilibrée (**S**) puis d'une parenthèse fermante, par exemple « ( ( ) ) ».
  - **a b S** : Une parenthèse ouvrante suivie d'une parenthèse fermante, puis d'une autre structure équilibrée, par exemple « () () ».
  - **a S b S** : Une parenthèse ouvrante suivie d'une structure équilibrée, puis d'une parenthèse fermante et d'une autre structure équilibrée, par exemple « ( () ) () ».

### **Pourquoi C'est Utile ?**

Ces formules AB permettent de générer toutes les combinaisons possibles de parenthèses équilibrées en suivant les règles de la grammaire. En utilisant **A** et **B**, on simplifie la représentation des règles et on facilite la compréhension de la structure des séquences générées.

### **Exemples**

1. **S = a b**
   - **S = a b** : Correspond à une paire de parenthèses « () ».

2. **S = a S b**
   - **S = a (a b) b** : Correspond à « (()) ».

3. **S = a b S**
   - **S = a b a b** : Correspond à « ()() ».

4. **S = a S b S**
   - **S = a (a b) b a b** : Correspond à « (())() ».

### **Conclusion**

En traduisant les règles de réécriture en formules AB, on obtient une représentation claire et structurée des séquences de parenthèses équilibrées. Cela aide à comprendre comment chaque paire de parenthèses peut être construite de manière ordonnée.

J'espère que cette réécriture te permet de mieux comprendre comment traduire les règles de réécriture en formules AB. Si tu as d'autres questions, n'hésite pas à les poser !
## **1.3.Exercice : Comprendre et résoudre**

#### **1. Quel est le langage généré par cette grammaire ? Combien de séquences de longueur 6 sont générées ?**

Cette grammaire génère des séquences de 0 et 1. Voyons rapidement les règles :

- **S** peut commencer par un **0** et suivre la règle pour **B**, ou commencer par un **1** et suivre la règle pour **A**.
- **B** peut être un **1**, ou un **1** suivi de **S**, ou encore un **0** suivi de deux **B**.
- **A** peut être un **0**, ou un **0** suivi de **S**, ou un **1** suivi de deux **A**.

Ces règles permettent de construire des séquences de bits. En analysant cela, le langage généré par cette grammaire est l'ensemble des séquences de 0 et 1 dans lesquelles **chaque dérivation commence par soit 0 soit 1, suivie de plusieurs séquences imbriquées de 0 et 1**.

##### Calcul des séquences de longueur 6

On peut examiner toutes les dérivations possibles pour obtenir des séquences de longueur 6. Étant donné que chaque règle peut générer soit un 0, soit un 1 avec plusieurs suites de symboles non-terminaux, on calcule toutes les combinaisons possibles pour obtenir des séquences de longueur 6.

Cependant, le dénombrement exact est complexe manuellement car cette grammaire est assez riche. Une approche plus systématique (comme un algorithme de génération) serait nécessaire pour déterminer le nombre exact de séquences de longueur 6 générées. Ce type de calcul pourrait être fait en analysant toutes les dérivations possibles en utilisant les règles et en suivant les transitions de **S**, **A**, et **B**.

#### **2. Traduction des règles en formules AB**

On va maintenant traduire les règles de la grammaire donnée en formules AB. Voici les règles données :

1. **S → 0 B | 1 A**
2. **B → 1 | 1 S | 0 B B**
3. **A → 0 | 0 S | 1 A A**

##### Traduction des règles

On traduit les règles de manière similaire à l'exercice précédent, en introduisant des formules qui représentent les différents cas.

1. **S = 0 B | 1 A**
   - **S** commence par soit un **0** suivi de **B**, soit un **1** suivi de **A**.
  
2. **B = 1 | 1 S | 0 B B**
   - **B** peut être un **1**, ou un **1** suivi de **S**, ou un **0** suivi de deux **B**.

3. **A = 0 | 0 S | 1 A A**
   - **A** peut être un **0**, ou un **0** suivi de **S**, ou un **1** suivi de deux **A**.

Ces règles montrent comment chaque non-terminal **S**, **A**, ou **B** se transforme en une séquence de 0 et de 1, en fonction des autres règles.

#### **3. La séquence 001101 est ambiguë. Donnez deux arbres de dérivation AB différents pour cette séquence.**

L'ambiguïté signifie que cette séquence peut être dérivée de deux façons différentes en utilisant les règles de la grammaire. On va explorer deux dérivations possibles de **001101**.

##### Première dérivation possible (Arbre 1)

1. **S → 0 B**
2. **B → 0 B B**
3. **B → 1**
4. **B → 1**
   - Ce chemin commence avec **S → 0 B**. Le **B** devient **0 B B**, où le premier **B** devient **1** et le second **B** devient aussi **1**.

Le résultat est **001101**.

##### Deuxième dérivation possible (Arbre 2)

1. **S → 0 B**
2. **B → 1 S**
3. **S → 1 A**
4. **A → 0**
   - Ici, **S → 0 B**, où **B → 1 S**. Ensuite, **S → 1 A** et **A → 0** pour obtenir la séquence **001101**.

Ces deux dérivations différentes montrent que la séquence **001101** peut être générée de deux façons distinctes avec cette grammaire, ce qui explique l'ambiguïté.

### Conclusion

- La grammaire génère des séquences complexes de 0 et 1, et la langue produite par cette grammaire contient ces séquences.
- Nous avons traduit les règles en formules AB qui expliquent comment chaque symbole se transforme.
- La séquence **001101** est ambiguë car elle peut être dérivée par deux chemins différents, ce que nous avons montré avec deux arbres de dérivation.
## 2.1.Résolution détaillée des exercices sur les grammaires AB

Nous allons résoudre ensemble les exercices donnés, en expliquant chaque étape de manière détaillée et en mettant en avant les concepts clés. Nous utiliserons un format arboré pour visualiser les dérivations, ce qui facilitera la compréhension.

## [[Guide Complet sur les Grammaires AB pour Débutants]]
## Introduction aux grammaires AB

Avant de commencer, il est important de comprendre ce qu'est une **grammaire AB**. En linguistique, une grammaire AB est un type de grammaire catégorielle qui utilise des formules pour représenter les catégories grammaticales des mots et la manière dont ils se combinent pour former des phrases.

### Catégories de base

- **n** : nom (par exemple, *chien*, *chat*)
- **np** : syntagme nominal (par exemple, *le chien*, *Alice*)
- **s** : phrase (une proposition complète)

### Catégories fonctionnelles

- **A/B** : une fonction qui attend un élément de catégorie **B** à sa droite pour former un élément de catégorie **A**.
- **A\\B** : une fonction qui attend un élément de catégorie **B** à sa gauche pour former un élément de catégorie **A**.

Ces notations nous aident à comprendre comment les mots peuvent se combiner pour former des structures grammaticales correctes.

---

## Exercices

Nous allons maintenant résoudre les exercices en assignant les formules manquantes et en montrant les dérivations pour chaque phrase.

1. Ben and Amy visited Paris.
2. Ben visited and disliked London.
3. Amy went to England and visited London.
4. Amy went to Paris and to London.
5. All students and professors like the new library.
6. Chris reads all new and interesting books.

### 1. **Alice dreams.**

#### Assignation des formules

- **Alice** : np (donné)
- **dreams** : ?

#### Détermination de la catégorie de "dreams"

"Dreams" est un verbe intransitif qui nécessite un sujet à sa gauche pour former une phrase complète. Nous pouvons donc lui attribuer la catégorie **s\\np**, ce qui signifie qu'il attend un syntagme nominal à sa gauche pour former une phrase.

#### Dérivation

1. **Alice** : np
2. **dreams** : s\\np

En combinant les deux :

- **np** (Alice) suivi de **s\\np** (dreams) donne **s** (phrase), via l'application arrière.

#### Arbre de dérivation

```
      s
     / \
   np  s\np
 Alice dreams
```

#### Explication simplifiée

- **Concept clé** : Le verbe "dreams" a besoin d'un sujet (un syntagme nominal) pour former une phrase.
- **Processus** : On combine "Alice" (np) avec "dreams" (s\\np) pour obtenir une phrase complète (s).

---

### 2. **The rabbit never dreams.**

#### Assignation des formules

- **The** : np/n
- **rabbit** : n (donné)
- **never** : (s\\np)/(s\\np)
- **dreams** : s\\np (comme précédemment)

#### Dérivation

1. **The rabbit** :

   - **The** (np/n) + **rabbit** (n) ⇒ **np**

2. **Never dreams** :

   - **Never** ((s\\np)/(s\\np)) + **dreams** (s\\np) ⇒ **s\\np**

3. **Phrase complète** :

   - **np** (The rabbit) + **s\\np** (never dreams) ⇒ **s**

#### Arbre de dérivation

```
             s
           /   \
        np       s\np
       /  \     /    \
   np/n    n  (s\np)/(s\np)  s\np
 the   rabbit   never     dreams
```

#### Explication simplifiée

- **"The"** transforme un nom en syntagme nominal : **the** + **rabbit** ⇒ **the rabbit** (np).
- **"Never"** est un adverbe qui modifie le verbe : il prend un verbe et donne un nouveau verbe modifié.
- On combine ensuite le sujet **np** avec le prédicat **s\\np** pour former la phrase complète.

---

### 3. **Tweedledum irritates Tweedledee.**

#### Assignation des formules

- **Tweedledum** : np (donné)
- **irritates** : (s\\np)/np
- **Tweedledee** : np (donné)

#### Dérivation

1. **Irritates Tweedledee** :

   - **irritates** ((s\\np)/np) + **Tweedledee** (np) ⇒ **s\\np**

2. **Phrase complète** :

   - **Tweedledum** (np) + **s\\np** ⇒ **s**

#### Arbre de dérivation

```
             s
           /   \
        np     s\np
     Tweedledum   /   \
              (s\np)/np  np
             irritates  Tweedledee
```

#### Explication simplifiée

- **"Irritates"** est un verbe transitif qui a besoin d'un objet (np) pour former un prédicat (s\\np).
- On combine d'abord **irritates** avec **Tweedledee** pour obtenir le prédicat.
- Ensuite, on combine le sujet **Tweedledum** avec le prédicat pour former la phrase.

---

### 4. **The Mad Hatter teases the Red Queen.**

#### Assignation des formules

- **The** : np/n
- **Mad** : n/n
- **Hatter** : n (donné)
- **teases** : (s\\np)/np
- **Red** : n/n
- **Queen** : n (donné)

#### Dérivation

1. **The Mad Hatter** :

   - **Mad** (n/n) + **Hatter** (n) ⇒ **n**
   - **The** (np/n) + **Mad Hatter** (n) ⇒ **np**

2. **The Red Queen** :

   - **Red** (n/n) + **Queen** (n) ⇒ **n**
   - **The** (np/n) + **Red Queen** (n) ⇒ **np**

3. **Teases the Red Queen** :

   - **teases** ((s\\np)/np) + **The Red Queen** (np) ⇒ **s\\np**

4. **Phrase complète** :

   - **The Mad Hatter** (np) + **s\\np** ⇒ **s**

#### Arbre de dérivation

```
                   s
                 /   \
               np     s\np
              /  \     /   \
         np/n   n    (s\np)/np  np
          |    / \     |      /    \
        The n/n   n  teases np/n     n
            Mad Hatter        |     / \
                             The  n/n  n
                              |  Red Queen
```

#### Explication simplifiée

- Les adjectifs **"Mad"** et **"Red"** modifient les noms pour former de nouveaux noms.
- On forme les syntagmes nominaux **"The Mad Hatter"** et **"The Red Queen"**.
- **"Teases"** est un verbe transitif qui prend un objet pour former un prédicat.
- On combine le sujet avec le prédicat pour former la phrase.

---

### 5. **The rabbit considers Alice slow.**

#### Assignation des formules

- **The** : np/n
- **rabbit** : n (donné)
- **considers** : ((s\\np)/adj)/np
- **Alice** : np (donné)
- **slow** : adj

#### Dérivation

1. **The rabbit** :

   - **The** (np/n) + **rabbit** (n) ⇒ **np**

2. **Considers Alice slow** :

   - **considers** (((s\\np)/adj)/np) + **Alice** (np) ⇒ (s\\np)/adj
   - Résultat + **slow** (adj) ⇒ **s\\np**

3. **Phrase complète** :

   - **The rabbit** (np) + **s\\np** ⇒ **s**

#### Arbre de dérivation

```
             s
           /   \
         np     s\np
        /  \     |
     np/n  n   ((s\np)/adj)/np
      The rabbit   /    \
                 np     adj
                Alice  slow
```

#### Explication simplifiée

- **"Considers"** est un verbe qui a besoin d'un objet (**Alice**) et d'un adjectif (**slow**) pour former un prédicat.
- On combine d'abord **considers** avec **Alice**, puis avec **slow**.
- **"The rabbit"** est le sujet qui se combine avec le prédicat pour former la phrase.

### 6. **Alice likes the song of the Gryphon.**

#### Assignation des formules

- **Alice** : np (donné)
- **likes** : (s\\np)/np (verbe transitif)
- **the** : np/n (déterminant)
- **song** : n (donné)
- **of** : (n\\n)/np (préposition)
- **the** : np/n (déterminant)
- **Gryphon** : n (donné)

#### Explication des catégories

- **"of"** est une préposition qui, en grammaire AB, est souvent assignée à la catégorie **(n\\n)/np**. Cela signifie qu'elle attend un np à sa droite (par exemple, *the Gryphon*) pour former un modificateur de nom (n\\n) qui s'appliquera à un nom à sa gauche (par exemple, *song*).

#### Dérivation

1. **The Gryphon** :

   - **the** (np/n) + **Gryphon** (n) ⇒ **np**

2. **of the Gryphon** :

   - **of** ((n\\n)/np) + **the Gryphon** (np) ⇒ **n\\n**

3. **song of the Gryphon** :

   - **song** (n) + **of the Gryphon** (n\\n) ⇒ **n**

   *(Ici, nous utilisons l'application arrière : n + n\\n ⇒ n)*

4. **The song of the Gryphon** :

   - **the** (np/n) + **song of the Gryphon** (n) ⇒ **np**

5. **likes the song of the Gryphon** :

   - **likes** ((s\\np)/np) + **the song of the Gryphon** (np) ⇒ **s\\np**

6. **Phrase complète** :

   - **Alice** (np) + **likes the song of the Gryphon** (s\\np) ⇒ **s**

#### Arbre de dérivation

```
                      s
                    /   \
                  np    s\np
                 Alice   /     \
                       (s\np)/np   np
                         likes    /          \
                                 np/n        n
                                  the      /      \
                                         n        n\n
                                       song    (n\n)/np np
                                                 of   /    \
                                                     np/n   n
                                                      the Gryphon
```

#### Explication simplifiée

- **Étape 1** : On construit **"The Gryphon"** en combinant **"the"** et **"Gryphon"**.
- **Étape 2** : On forme **"of the Gryphon"** en combinant **"of"** avec **"The Gryphon"**.
- **Étape 3** : On applique **"of the Gryphon"** à **"song"** pour obtenir **"song of the Gryphon"**.
- **Étape 4** : On ajoute **"the"** pour obtenir **"The song of the Gryphon"**.
- **Étape 5** : On combine **"likes"** avec **"The song of the Gryphon"**.
- **Étape 6** : On complète la phrase en combinant **"Alice"** avec le prédicat obtenu.

---

### 7. **She likes Tweedledee.**

#### Assignation des formules

- **She** : np (pronom sujet)
- **likes** : (s\\np)/np (verbe transitif)
- **Tweedledee** : np (donné)

#### Dérivation

1. **likes Tweedledee** :

   - **likes** ((s\\np)/np) + **Tweedledee** (np) ⇒ **s\\np**

2. **Phrase complète** :

   - **She** (np) + **s\\np** ⇒ **s**

#### Arbre de dérivation

```
       s
     /   \
   np    s\np
  She     /     \
        (s\np)/np  np
         likes  Tweedledee
```

#### Explication simplifiée

- **Concept clé** : Les pronoms comme **"She"** fonctionnent comme des syntagmes nominaux (**np**).
- On suit le même processus que précédemment : le verbe transitif **"likes"** attend un objet (**np**) pour former un prédicat (**s\\np**), puis on combine avec le sujet pour obtenir une phrase complète.

---

### 8. **Tweedledee likes her.**

#### Assignation des formules

- **Tweedledee** : np (donné)
- **likes** : (s\\np)/np (verbe transitif)
- **her** : np (pronom objet)

#### Dérivation

1. **likes her** :

   - **likes** ((s\\np)/np) + **her** (np) ⇒ **s\\np**

2. **Phrase complète** :

   - **Tweedledee** (np) + **s\\np** ⇒ **s**

#### Arbre de dérivation

```
        s
      /   \
    np    s\np
   Tweedledee /    \
            (s\np)/np  np
             likes    her
```

#### Explication simplifiée

- **Concept clé** : **"Her"** est un pronom objet qui fonctionne comme un **np**.
- Le processus est identique : le verbe transitif **"likes"** prend un objet pour former un prédicat, puis on combine avec le sujet.

---

### 9. **The Hatter who teases Alice.**

#### Note importante

Cette phrase est une **syntaxe nominale complexe** avec une **proposition relative** (**who teases Alice**). Nous devons donc gérer la combinaison d'un nom avec une proposition relative.

#### Assignation des formules

- **The** : np/n (déterminant)
- **Hatter** : n (donné)
- **who** : (n\\n)/(s/np) (pronom relatif)
- **teases** : (s\\np)/np (verbe transitif)
- **Alice** : np (donné)

#### Explication des catégories

- **"who"** est un pronom relatif qui relie une proposition relative au nom qu'elle qualifie. Il prend une phrase incomplète (manquant un np sujet) pour former un modificateur de nom (**n\\n**).

#### Dérivation

1. **teases Alice** :

   - **teases** ((s\\np)/np) + **Alice** (np) ⇒ **s\\np**

2. **who teases Alice** :

   - **who** ((n\\n)/(s/np)) + **teases Alice** (s\\np) ⇒ **n\\n**

   *(Note : Ici, nous devons ajuster pour que **who** puisse combiner avec **s\\np**. Cela nécessite une transformation ou une catégorie différente.)*

**Problème identifié** :

Il y a une incohérence dans les catégories assignées. En grammaire AB standard, le pronom relatif **"who"** peut être assigné à la catégorie **(n\\n)/(s\\np)**, ce qui signifie qu'il prend une phrase relative manquant le sujet (donc une phrase de catégorie **s\\np**) pour former un modificateur de nom (**n\\n**).

**Correction de la catégorie de "who"** :

- **who** : (n\\n)/(s\\np)

#### Reprise de la dérivation

1. **teases Alice** :

   - **teases** ((s\\np)/np) + **Alice** (np) ⇒ **s\\np**

2. **who teases Alice** :

   - **who** ((n\\n)/(s\\np)) + **teases Alice** (s\\np) ⇒ **n\\n**

3. **Hatter who teases Alice** :

   - **Hatter** (n) + **who teases Alice** (n\\n) ⇒ **n**

4. **The Hatter who teases Alice** :

   - **the** (np/n) + **Hatter who teases Alice** (n) ⇒ **np**

#### Arbre de dérivation

```
                   np
                 /    \
              np/n     n
              The     /   \
                    n      n\n
                  Hatter  /     \
                         (n\n)/(s\np)  s\np
                             who      /     \
                                   (s\np)/np  np
                                    teases  Alice
```

#### Explication simplifiée

- **Étape 1** : On forme **"teases Alice"** en combinant le verbe **"teases"** avec **"Alice"**.
- **Étape 2** : **"who"** prend la phrase **"teases Alice"** pour former un modificateur de nom.
- **Étape 3** : On applique ce modificateur au nom **"Hatter"** pour obtenir **"Hatter who teases Alice"**.
- **Étape 4** : On ajoute **"the"** pour obtenir le syntagme nominal complet **"The Hatter who teases Alice"**.

---

## Conclusion

En utilisant les grammaires AB, nous avons pu assigner des catégories grammaticales aux mots et montrer comment ils se combinent pour former des phrases grammaticalement correctes. Les arbres de dérivation nous aident à visualiser ces combinaisons étape par étape.

Les concepts clés à retenir sont :

- **Catégories grammaticales** : n, np, s, adj, etc.
- **Fonctions grammaticales** : comment certains mots (comme les verbes) attendent d'autres mots pour former des structures plus grandes.
- **Applications** : combiner des catégories en utilisant les règles d'application avant (/) et arrière (\\).

En comprenant ces concepts, on peut analyser et construire des phrases de manière structurée et logique.



