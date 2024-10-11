---
tags:
  - aideDecision
  - tps
---
[[Fiche d'exo 2]]

## Exercice 1

Soit la base stratifiée suivante :  
$$
\Sigma = \{(\neg a \lor c, b \lor c), (\neg b, \neg c), (\neg a \lor \neg c, a \lor c)\}
$$

### Résolution

La base est décomposée en trois strates :  
$$
S_1 = (\neg a \lor c, b \lor c), \quad S_2 = (\neg b, \neg c), \quad S_3 = (\neg a \lor \neg c, a \lor c)
$$

**Étape 1 : Calcul de \(Free(S_1)\)**  
\( S_1 \) ne contient aucun conflit, donc :
$$
Free(S_1) = \{\neg a \lor c, b \lor c\}
$$

**Étape 2 : Calcul de \(Free(S_1 \cup S_2)\)**  
\( S_1 \cup S_2 \) est incohérent (\(c\) et \(\neg c\)). Donc :
$$
Free(S_1 \cup S_2) = \varnothing
$$

**Étape 3 : Calcul de \(Free(S_1 \cup S_2 \cup S_3)\)**  
\( S_1 \cup S_2 \cup S_3 \) est également incohérent. Donc :
$$
Free(S_1 \cup S_2 \cup S_3) = \varnothing
$$

**Conclusion**  
L'ensemble des formules libres est :
$$
Free(\Sigma) = \{\neg a \lor c, b \lor c\}
$$

## Exercice 2

Soit
$$
\Sigma = \{ b,\ \neg a \lor \neg b \lor c,\ d \lor e,\ \neg d \lor e,\ \neg e \lor \neg c,\ f \lor a,\ \neg f \lor a \}
$$

### Objectif

1. **Calculer des arguments** (s'ils existent) pour chacune des formules \( a, b, c, d, e, f \).
2. **Donner l'ensemble des formules** qui sont une conséquence argumentée de \( \Sigma \).

### Définitions Clés

Avant de commencer, voici quelques définitions simples pour mieux comprendre le problème :

- **Base de Connaissances (\( \Sigma \))** : Un ensemble de phrases logiques que nous utilisons pour tirer des conclusions.
  
- **Argument** : Un argument pour une formule \( \phi \) est un ensemble de phrases dans \( \Sigma \) qui, ensemble, nous permettent de déduire \( \phi \) sans contradiction.

- **Conséquence Argumentée** : Une formule est une conséquence argumentée de \( \Sigma \) si on peut construire un argument pour elle à partir de \( \Sigma \).

### 1. Calcul des Arguments pour Chaque Formule

Nous allons examiner chaque formule \( a, b, c, d, e, f \) pour voir si nous pouvons construire un argument pour elle à partir des éléments de \( \Sigma \).

#### **Argument pour \( a \)**
$$
A = \langle \{ f \lor a,\ \neg f \lor a \},\ a \rangle
$$
**Explication :**
- **Étape 1** : Regardons les deux phrases \( f \lor a \) et \( \neg f \lor a \).
- **Étape 2** : Si on suppose que \( f \) est vrai, alors \( f \lor a \) est vrai, donc \( a \) doit aussi être vrai.
- **Étape 3** : Si on suppose que \( f \) est faux, alors \( \neg f \lor a \) devient vrai, ce qui implique encore que \( a \) doit être vrai.
- **Conclusion** : Quelle que soit la valeur de \( f \), \( a \) doit être vrai. Donc, \( a \) est déduit de ces deux phrases.

#### **Argument pour \( b \)**
$$
B = \langle \{ b \},\ b \rangle
$$
**Explication :**
- **Étape 1** : La base \( \Sigma \) contient la phrase \( b \).
- **Étape 2** : Cette phrase affirme directement que \( b \) est vrai.
- **Conclusion** : Aucun raisonnement supplémentaire n'est nécessaire. \( b \) est vrai grâce à cette phrase seule.

#### **Argument pour \( c \)**
$$
C = \langle \{ b,\ \neg a \lor \neg b \lor c \},\ c \rangle
$$
**Explication :**
- **Étape 1** : Nous savons déjà que \( b \) est vrai (grâce à l'argument \( B \)).
- **Étape 2** : Regardons la phrase \( \neg a \lor \neg b \lor c \).
- **Étape 3** : Comme \( b \) est vrai, \( \neg b \) est faux. Donc, la phrase devient \( \neg a \lor c \).
- **Étape 4** : Si \( a \) est vrai, alors \( \neg a \) est faux, ce qui implique que \( c \) doit être vrai.
- **Étape 5** : Si \( a \) est faux, alors \( \neg a \) est vrai, et la phrase \( \neg a \lor c \) est vraie quelle que soit la valeur de \( c \).
- **Étape 6** : Cependant, nous avons déjà un argument pour \( a \) (argument \( A \)), qui montre que \( a \) est vrai.
- **Conclusion** : Puisque \( a \) est vrai, \( c \) doit également être vrai pour que \( \neg a \lor c \) soit vrai.

#### **Argument pour \( d \)**
**Aucun argument n'existe** pour \( d \) à partir de \( \Sigma \).

**Explication :**
- **Étape 1** : Les phrases impliquant \( d \) sont \( d \lor e \) et \( \neg d \lor e \).
- **Étape 2** : Si nous essayons de déduire \( d \), nous constatons que ces phrases nous mènent à \( e \), mais pas directement à \( d \).
- **Étape 3** : Il n'y a pas de phrase dans \( \Sigma \) qui affirme ou nie directement \( d \) sans impliquer d'autres variables.
- **Conclusion** : Nous ne pouvons pas construire un argument clair pour \( d \) sans contradiction ou information supplémentaire.

#### **Argument pour \( e \)**
$$
E = \langle \{ d \lor e,\ \neg d \lor e \},\ e \rangle
$$
**Explication :**
- **Étape 1** : Considérons les phrases \( d \lor e \) et \( \neg d \lor e \).
- **Étape 2** : Si \( d \) est vrai, alors \( d \lor e \) est vrai, mais cela ne nous dit rien sur \( e \).
- **Étape 3** : Si \( d \) est faux, alors \( \neg d \lor e \) devient vrai, ce qui implique que \( e \) doit être vrai.
- **Étape 4** : Cependant, pour que les deux phrases soient vraies en même temps, \( e \) doit nécessairement être vrai.
- **Conclusion** : Donc, \( e \) est vrai.

#### **Argument pour \( f \)**
**Aucun argument n'existe** pour \( f \) à partir de \( \Sigma \).

**Explication :**
- **Étape 1** : Les seules phrases impliquant \( f \) sont \( f \lor a \) et \( \neg f \lor a \).
- **Étape 2** : Ces phrases nous permettent de déduire \( a \), mais elles ne nous donnent aucune information directe sur \( f \).
- **Conclusion** : Nous ne pouvons pas construire un argument clair pour \( f \) sans information supplémentaire.

### 2. Conséquences Argumentées de \( \Sigma \)

Les formules pour lesquelles nous avons pu construire des arguments sont :

$$
\{ a,\ b,\ c,\ e \}
$$

**Justification :**

- **\( a \)** : Déduit grâce à l'argument \( A \).
- **\( b \)** : Affirmé directement par l'argument \( B \).
- **\( c \)** : Déduit grâce à l'argument \( C \).
- **\( e \)** : Déduit grâce à l'argument \( E \).

Les formules **\( d \)** et **\( f \)** ne possèdent pas d'arguments valides dans \( \Sigma \) et ne sont donc **pas** des conséquences argumentées de \( \Sigma \).

### Résumé des Arguments

| Formule | Argument                                 | Conclusion |
| ------- | ---------------------------------------- | ---------- |
| \( a \) | $$ \{ f \lor a,\ \neg f \lor a \} $$     | \( a \)    |
| \( b \) | \( \{ b \} \)                            | \( b \)    |
| \( c \) | $$\{ b,\ \neg a \lor \neg b \lor c \} $$ | \( c \)    |
| \( d \) | Aucun                                    | -          |
| \( e \) | $$ \{ d \lor e,\ \neg d \lor e \} $$     | \( e \)    |
| \( f \) | Aucun                                    | -          |

### Conclusion Finale

En analysant chaque formule, nous avons identifié les arguments possibles pour \( a, b, c, e \) et constaté qu'il n'existe pas d'arguments directs pour \( d \) et \( f \) dans \( \Sigma \).

Ainsi, **les conséquences argumentées de \( \Sigma \)** sont :

$$
\{ a,\ b,\ c,\ e \}
$$

Ces formules peuvent être déduites de manière cohérente à partir de \( \Sigma \) sans créer de contradictions
