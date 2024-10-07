---
tags:
  - AdminBDD
  - tps
---

## Exercice 1


### Exercice : Construction d’un arbre B+

Nous devons construire un arbre B+ avec les numéros de départements (`numD`) comme clés d’accès. L’ordre de l’arbre est **1** (ce qui signifie qu'un nœud interne peut contenir au plus **2 clés** et une feuille peut contenir au plus **2 valeurs**).

#### Valeurs à insérer dans l’arbre B+ :
```
17, 14, 20, 3, 8, 25, 30, 19
```

### Étape par étape de la construction :

---

### **Étape 1 : Insertion de `17`**

- **L'arbre est vide**, on crée une feuille et on insère `17`.

**Arbre** :
```
[17]
```

---

### **Étape 2 : Insertion de `14`**

- La feuille contenant `17` a encore de la place (maximum 2 valeurs dans un nœud feuille), donc on insère `14`.
- Après insertion, on trie les valeurs dans la feuille.

**Arbre** :
```
[14, 17]
```

---

### **Étape 3 : Insertion de `20`**

- Le nœud feuille `[14, 17]` est plein (2 valeurs), donc un **éclatement** est nécessaire.
- La **clé médiane** `17` est remontée pour former un nœud interne.
- Après éclatement, le nœud gauche contient `[14]` et le nœud droit contient `[20]`.

**Arbre** :
```
    [17]
   /    \
 [14]  [20]
```

---

### **Étape 4 : Insertion de `3`**

- On insère la valeur `3` dans le nœud gauche qui contient actuellement `[14]`.
- Le nœud a de la place, donc on insère `3` et on trie.

**Arbre** :
```
    [17]
   /    \
 [3, 14] [20]
```

---

### **Étape 5 : Insertion de `8`**

- Le nœud gauche `[3, 14]` est plein, un **éclatement** est nécessaire.
- La **clé médiane** `8` est remontée vers le nœud interne.
- Après l’éclatement, le nœud gauche devient `[3]`, et une nouvelle feuille avec `[14]` est créée.

**Arbre** :
```
     [8, 17]
    /   |   \
  [3] [14] [20]
```

---

### **Étape 6 : Insertion de `25`**

- La valeur `25` est insérée dans la feuille qui contient `20` car `25 > 17`.
- Il y a de la place dans la feuille, donc on insère `25` à droite du `20`.

**Arbre** :
```
     [8, 17]
    /   |   \
  [3] [14] [20, 25]
```

---

### **Étape 7 : Insertion de `30`**

- La feuille `[20, 25]` est pleine. Il faut la **diviser**.
- La **clé médiane** `25` est remontée dans le nœud interne.
- Cependant, le nœud interne `[8, 17]` a déjà 2 clés, donc il éclate également :
  - La clé médiane `17` est remontée pour former une **nouvelle racine**.
  - Le nœud gauche de la nouvelle racine contient `[8]` et le nœud droit contient `[25]`.

**Arbre** :
```
      [17]
     /    \
  [8]    [25]
 /   \   /   \
[3] [14] [20] [30]
```

---

### **Étape 8 : Insertion de `19`**

- On insère `19` dans la feuille qui contient `20`, car `19 < 25`.
- Le nœud `[20]` a de la place, donc on insère `19` et on trie.

**Arbre final** :
```
      [17]
     /    \
  [8]    [25]
 /   \   /   \
[3] [14] [19, 20] [30]
```

---

### Conclusion :
L'arbre B+ final a bien une **hauteur de 3**, avec les nœuds internes contenant **au maximum 2 clés** et les feuilles contenant **au maximum 2 valeurs**. Le processus d’éclatement a été correctement géré à chaque insertion, assurant que l’arbre reste équilibré.

## Exercice 2 

#### Données :
- La table contient **50 000 tuples** de **100 octets** chacun.
- La taille d'un bloc (ou page) de données est de **8192 octets**.
- **1192 octets** sont réservés pour l'en-tête et les insertions/modifications, donc l'espace disponible par bloc pour héberger les enregistrements de tuples est de **7000 octets**.

---

### 2.1 Question 1 : Calcul du facteur de blocage et du nombre de blocs nécessaires

#### 1. Calcul du facteur de blocage \( b_f \)

Le facteur de blocage est le nombre de tuples pouvant être stockés dans un bloc. La formule utilisée est :

$$
b_f = \frac{\text{Taille disponible par bloc}}{\text{Taille d'un tuple}}
$$

Substitution des valeurs :

$$
b_f = \frac{7000 \text{ octets}}{100 \text{ octets/tuple}} = 70 \text{ tuples par bloc}
$$

#### 2. Calcul du nombre total de blocs \( B \)

Le nombre de blocs nécessaires est obtenu en divisant le nombre total de tuples par le facteur de blocage. La formule est :

$$
B = \frac{\text{Nombre total de tuples}}{b_f}
$$

Substitution des valeurs :

$$
B = \frac{50 000 \text{ tuples}}{70 \text{ tuples/bloc}} = 714.29 \text{ blocs}
$$

Comme on ne peut pas avoir une fraction de bloc, on arrondit à l'entier supérieur :

$$
B = 715 \text{ blocs}
$$

#### 3. Calcul de la taille totale en octets de la table

La taille totale de la table est obtenue en multipliant le nombre total de blocs par la taille de chaque bloc. La formule est :

$$
\text{Taille totale} = B \times \text{Taille d'un bloc}
$$

Substitution des valeurs :

$$
\text{Taille totale} = 715 \text{ blocs} \times 8192 \text{ octets/bloc} = 5 859 680 \text{ octets}
$$

La taille totale de la table est donc **5 859 680 octets**, soit environ **5.86 Mo**.

---

### 2.2 Question 2 : Coût en nombre de blocs à parcourir lors d'une requête

#### Hypothèses :
- Aucun index n'est construit.
- Les blocs de données de la table sont en mémoire secondaire.
- Le temps d'accès à chaque bloc est de **0.01 seconde**.

#### 1. Coût moyen de la recherche d'un tuple existant

Pour une recherche sans index, nous devons effectuer une **recherche séquentielle**. En moyenne, il faudra lire la moitié des blocs pour trouver le tuple. La formule utilisée pour le nombre moyen de blocs à parcourir est :

$$
\text{Blocs à parcourir (moyenne)} = \frac{B}{2}
$$

Substitution des valeurs :

$$
\frac{715}{2} = 357.5 \text{ blocs}
$$

Le temps nécessaire est donné par le nombre de blocs parcourus multiplié par le temps d'accès à chaque bloc. La formule pour le **temps de recherche** est :

$$
\text{Temps (tuple existant)} = \text{Blocs à parcourir} \times \text{Temps par bloc}
$$

Substitution des valeurs :

$$
\text{Temps (tuple existant)} = 357.5 \times 0.01 \text{ sec/bloc} = 3.575 \text{ secondes}
$$

#### 2. Coût de la recherche d'un tuple inexistant

Si le tuple n'existe pas, il faudra parcourir **tous les blocs** pour vérifier qu'il n'est pas présent. Le nombre de blocs à parcourir est simplement \( B \). La formule utilisée est :

$$
\text{Blocs à parcourir (tuple inexistant)} = B
$$

Substitution des valeurs :

$$
\text{Blocs à parcourir (tuple inexistant)} = 715 \text{ blocs}
$$

Le temps nécessaire est donné par la formule :

$$
\text{Temps (tuple inexistant)} = B \times \text{Temps par bloc}
$$

Substitution des valeurs :

$$
\text{Temps (tuple inexistant)} = 715 \times 0.01 \text{ sec/bloc} = 7.15 \text{ secondes}
$$
### 2.3 Question 3 : Calcul du facteur de blocage, du nombre de blocs et de la hauteur de l'index B+

#### Données :
- Taille des enregistrements d'index : **20 octets**.
- Taille disponible dans un bloc : **7000 octets** (espace après en-tête).
- Clé unique et dense.
- La table contient **50 000 tuples**.

---

#### 1. Calcul du facteur de blocage \( b_f \) pour les enregistrements d'index

Le facteur de blocage est le nombre d'enregistrements d'index pouvant être stockés dans un bloc. La formule utilisée est :

$$
b_f = \frac{\text{Taille disponible par bloc}}{\text{Taille d'un enregistrement d'index}}
$$

Substitution des valeurs :

$$
b_f = \frac{7000 \text{ octets}}{20 \text{ octets/enregistrement}} = 350 \text{ enregistrements par bloc}
$$

---

#### 2. Calcul du nombre total de blocs nécessaires pour l'index

Le nombre total de blocs nécessaires pour stocker les enregistrements d'index est obtenu en divisant le nombre total de tuples par le facteur de blocage des enregistrements d'index. La formule est :

$$
B = \frac{\text{Nombre total de tuples}}{b_f}
$$

Substitution des valeurs :

$$
B = \frac{50 000 \text{ tuples}}{350 \text{ enregistrements/bloc}} = 142.86 \text{ blocs}
$$

On arrondit à l'entier supérieur, donc :

$$
B = 143 \text{ blocs}
$$

---

#### 3. Calcul de la taille totale en octets pour l'index

La taille totale de l'index est obtenue en multipliant le nombre de blocs par la taille d'un bloc. La formule est :

$$
\text{Taille totale} = B \times \text{Taille d'un bloc}
$$

Substitution des valeurs :

$$
\text{Taille totale} = 143 \times 7000 \text{ octets/bloc} = 1 001 000 \text{ octets}
$$

La taille totale pour l'index est donc **1 001 000 octets** (environ **1 Mo**).

---

#### 4. Calcul de la hauteur et de l'ordre de l'arbre B+

- **Ordre de l'arbre B+** : L'ordre est défini comme le nombre de tuples par bloc divisé par 2. La formule est :

$$
\text{Ordre} = \frac{b_f}{2}
$$

Substitution des valeurs :

$$
\text{Ordre} = \frac{350}{2} = 175
$$

- **Hauteur de l'arbre B+** : La hauteur est donnée par la formule logarithmique en base \(b_f\) du nombre total de blocs nécessaires pour l'index. La formule est :

$$
h = \lceil \log_{\text{Ordre}}{B} \rceil
$$

Substitution des valeurs :

$$
h = \lceil \log_{175}{143} \rceil \approx \lceil 1.03 \rceil = 2
$$

La hauteur de l'arbre B+ est donc **2**.

---

### 2.4 Question 4 : Coût en nombre de blocs pour obtenir un tuple avec sélection sur clé

#### Données :
- L'attribut clé est indexé par un B+ Tree de hauteur **2** (calculé dans la question précédente).
- Le B+ Tree a un ordre de **175**.
- La sélection est basée sur une comparaison avec un **opérateur d'égalité**.

---

#### 1. Coût lorsque le tuple **existe**

Lorsqu'un tuple existe et que la recherche se fait avec un index B+ Tree, le coût est lié au parcours de l'arbre, de la racine jusqu'à la feuille, plus un accès supplémentaire pour récupérer le bloc contenant le tuple.

La formule pour le **coût en nombre de blocs** est :

$$
\text{Coût (tuple existant)} = h + 1
$$

Où \( h \) est la hauteur de l'arbre, et \( 1 \) représente l'accès supplémentaire pour lire le bloc feuille contenant le tuple.

Substitution des valeurs :

$$
\text{Coût (tuple existant)} = 2 + 1 = 3 \text{ blocs}
$$

Donc, lorsqu'un tuple **existe**, le coût est de **3 blocs**.

---

#### 2. Coût lorsque le tuple **n'existe pas**

Lorsque le tuple n'existe pas, le coût est similaire à celui du tuple existant, car nous devons toujours parcourir l'arbre B+ jusqu'à la feuille pour vérifier l'absence du tuple.

La formule pour le **coût en nombre de blocs** est également :

$$
\text{Coût (tuple inexistant)} = h + 1
$$

Substitution des valeurs :

$$
\text{Coût (tuple inexistant)} = 2 + 1 = 3 \text{ blocs}
$$

Donc, lorsqu'un tuple **n'existe pas**, le coût est également de **3 blocs**.

---


### 2.5 Question 5 : Coût en nombre de blocs pour obtenir un tuple avec sélection sur un attribut non clé

#### Données :
- Aucun index n'est construit sur l'attribut non clé.
- La recherche se fait donc de manière **séquentielle** à travers tous les blocs de la table.
- La table contient **715 blocs** (calculé dans la **Question 1**).

---

#### 1. Coût lorsque le tuple **existe**

Quand le tuple recherché **existe**, la recherche se fait de manière séquentielle dans les blocs. En moyenne, on trouvera le tuple après avoir parcouru la moitié des blocs de la table.

La formule pour le **coût moyen en nombre de blocs** est :

$$
\text{Coût (tuple existant)} = \frac{B}{2}
$$

Où \( B \) est le nombre total de blocs dans la table.

Substitution des valeurs :

$$
\text{Coût (tuple existant)} = \frac{715}{2} = 357.5 \text{ blocs}
$$

On arrondit à l'entier supérieur, donc :

$$
\text{Coût (tuple existant)} = 358 \text{ blocs}
$$

---

#### 2. Coût lorsque le tuple **n'existe pas**

Lorsque le tuple **n'existe pas**, il est nécessaire de parcourir **tous les blocs** de la table pour vérifier qu'il n'est pas présent.

La formule pour le **coût en nombre de blocs** est simplement :

$$
\text{Coût (tuple inexistant)} = B
$$

Substitution des valeurs :

$$
\text{Coût (tuple inexistant)} = 715 \text{ blocs}
$$

---


### 2.6 Question 6 : Nombre de tuples supplémentaires pour un nouveau niveau dans le B+ Tree

#### Données :
- La table contient actuellement **50 000 tuples**.
- Le facteur de blocage de l'index est de **350 enregistrements par bloc** (calculé dans la **Question 3**).
- Le B+ Tree a une **hauteur de 2** (calculé dans la **Question 3**).
- L'ordre de l'arbre est de **175**.

---

#### Objectif :
Nous devons déterminer combien de tuples supplémentaires sont nécessaires pour que l'index B+ Tree atteigne un **niveau supplémentaire** (c'est-à-dire une hauteur de 3).

---

#### 1. Nombre de tuples que peut gérer un B+ Tree de hauteur \( h = 2 \)

Un B+ Tree de hauteur \( h \) peut gérer un certain nombre de tuples en fonction de son ordre \( m \). La formule pour calculer le **nombre maximum de tuples** qu'un arbre B+ de hauteur \( h \) peut gérer est :

$$
N_{\text{max}} = m^h \times b_f
$$

Où :
- \( m \) est l'ordre de l'arbre (ici \( 175 \)),
- \( h \) est la hauteur de l'arbre (ici \( 2 \)),
- \( b_f \) est le facteur de blocage (nombre de tuples par bloc, ici \( 350 \)).

Substitution des valeurs :

$$
N_{\text{max}} = 175^2 \times 350 = 30 625 \times 350 = 10 718 750 \text{ tuples}
$$

Un B+ Tree de hauteur \( 2 \) peut donc gérer jusqu'à **10 718 750 tuples**.

---

#### 2. Nombre de tuples que peut gérer un B+ Tree de hauteur \( h = 3 \)

De la même manière, nous pouvons calculer le nombre de tuples qu'un B+ Tree de **hauteur 3** peut gérer. La formule est :

$$
N_{\text{max,3}} = m^3 \times b_f
$$

Substitution des valeurs :

$$
N_{\text{max,3}} = 175^3 \times 350 = 5 359 375 \times 350 = 1 875 781 250 \text{ tuples}
$$

Un B+ Tree de hauteur **3** peut donc gérer jusqu'à **1 875 781 250 tuples**.

---

#### 3. Nombre de tuples supplémentaires nécessaires

Actuellement, l'arbre B+ Tree peut gérer **50 000 tuples**, et nous avons calculé qu'un B+ Tree de hauteur 2 peut gérer jusqu'à **10 718 750 tuples**. 

Le nombre de **tuples supplémentaires** nécessaires pour atteindre un nouveau niveau dans l'arbre est donc simplement le nombre de tuples supplémentaires nécessaires pour dépasser cette limite de **10 718 750** tuples.

La formule pour le **nombre de tuples supplémentaires** nécessaires est :

$$
\text{Tuples supplémentaires} = N_{\text{max,2}} - \text{Nombre actuel de tuples}
$$

Substitution des valeurs :

$$
\text{Tuples supplémentaires} = 10 718 750 - 50 000 = 10 668 750 \text{ tuples}
$$

---

### Résumé des résultats de l'Exercice 2 :

- **2.1** : Facteur de blocage, nombre de blocs, et taille de la table
  - **Facteur de blocage** \( b_f \) : 70 tuples par bloc.
  - **Nombre de blocs** \( B \) : 715 blocs.
  - **Taille totale** : 5 859 680 octets (soit environ 5.86 Mo).

- **2.2** : Coût en nombre de blocs lors d'une requête
  - **Coût moyen (tuple existant)** : 357.5 blocs (arrondi à 358 blocs), soit 3.575 secondes.
  - **Coût moyen (tuple inexistant)** : 715 blocs, soit 7.15 secondes.

- **2.3** : Facteur de blocage, nombre de blocs, et hauteur de l'index B+ Tree
  - **Facteur de blocage de l'index** \( b_f \) : 350 enregistrements par bloc.
  - **Nombre total de blocs pour l'index** : 143 blocs.
  - **Taille totale de l'index** : 1 001 000 octets (soit environ 1 Mo).
  - **Hauteur de l'arbre B+** : 2.

- **2.4** : Coût en nombre de blocs pour obtenir un tuple avec sélection sur clé
  - **Coût (tuple existant)** : 3 blocs.
  - **Coût (tuple inexistant)** : 3 blocs.

- **2.5** : Coût en nombre de blocs pour obtenir un tuple avec sélection sur un attribut non clé
  - **Coût (tuple existant)** : 358 blocs.
  - **Coût (tuple inexistant)** : 715 blocs.

- **2.6** : Nombre de tuples supplémentaires pour un nouveau niveau dans le B+ Tree
  - **Tuples supplémentaires nécessaires** : 10 668 750 tuples pour atteindre un nouveau niveau (hauteur 3).

## Exercice 3
