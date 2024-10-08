# Résolution détaillée des exercices sur les grammaires AB

Nous allons résoudre ensemble les exercices donnés, en expliquant chaque étape de manière détaillée et en mettant en avant les concepts clés. Nous utiliserons un format arboré pour visualiser les dérivations, ce qui facilitera la compréhension.

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



