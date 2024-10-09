---
tags:
  - aideDecision
  - tps
---
[[Fiche Exos partie 1]]
## **Méthodologie de résolution pour l'Exercice 1**

#### Objectif :
Cet exercice consiste à **formuler et structurer** les préférences exprimées en langage naturel par un utilisateur, en utilisant les notations adéquates. Il faut identifier les **variables** et leurs **domaines** (les options possibles), puis **formaliser** les préférences sous forme de relations.

---

### **Étapes de la résolution :**

1. **Identifier les variables et leurs domaines**
2. **Traduire les préférences en relations formelles**
3. **Gérer les préférences incomparables**
4. **Formaliser les préférences conditionnelles**
5. **Vérification et résumé des préférences**

---

### **Étape 1 : Identifier les variables et leurs domaines**

Dans cette étape, il faut repérer les éléments clés des préférences de l'utilisateur. Chaque élément important (soupe, entrée, vin) devient une **variable** avec plusieurs **domaines possibles** (valeurs que cette variable peut prendre).

**Variables identifiées :**
- **\( S \)** (soupe) : {**P** (Poisson), **V** (Végétarienne)}
  - \( P \) signifie que l'utilisateur choisit une soupe de poisson.
  - \( V \) signifie que l'utilisateur choisit une soupe végétarienne.

- **\( E \)** (entrée) : {**C** (Copieuse), **N** (Non copieuse), **F** (Fruits de mer)}
  - \( C \) signifie que l'entrée est copieuse.
  - \( N \) signifie que l'entrée n'est pas copieuse.
  - \( F \) signifie que l'entrée est à base de fruits de mer (qu'elle soit copieuse ou non).

- **\( V \)** (vin) : {**R** (Rouge), **B** (Blanc)}
  - \( R \) signifie vin rouge.
  - \( B \) signifie vin blanc.

**Résumé de cette étape :**
- **\( S \)** : {**P**, **V**}
- **\( E \)** : {**C**, **N**, **F**}
- **\( V \)** : {**R**, **B**}

---

### **Étape 2 : Traduire les préférences en relations formelles**

Cette étape consiste à formuler les préférences sous forme de relations mathématiques entre les différentes variables. Chaque préférence de l'utilisateur se traduit par une **relation stricte de préférence**.

#### Exemple de traduction d'une préférence :
- **Langage naturel** : "Je préfère la soupe de poisson à la soupe végétarienne."
- **Formulation formelle** : \( P > V \), ce qui signifie que la soupe de poisson est préférée à la soupe végétarienne.

---

### **Étape 3 : Gérer les préférences incomparables**

Dans certains cas, l'utilisateur ne peut pas exprimer de préférence claire entre certaines options, ce qui signifie qu'il est **impossible** de formaliser une relation de préférence stricte.

- **Langage naturel** : "La préférence n’est pas vraie lorsque l’entrée est à base de fruits de mer, qu’elle soit copieuse ou non."
- **Explication** : Ici, l'utilisateur n'a pas de préférence entre les soupes de poisson et végétarienne lorsqu'il y a des fruits de mer. Cette situation est **incomparable**, donc il est **impossible de la formaliser**.

---

### **Étape 4 : Formaliser les préférences conditionnelles**

Dans certains cas, les préférences dépendent d'une condition spécifique. Ces préférences sont appelées **préférences conditionnelles**. Elles se formalisent à l'aide du **connecteur logique conditionnel** (impliquant "si... alors...").

#### Exemple de préférence conditionnelle :
- **Langage naturel** : "Lorsque l’entrée est copieuse, je préfère la soupe végétarienne à la soupe de poisson."
- **Formulation formelle** : \( C \land V > C \land P \)
  - Cela signifie que **si l'entrée est copieuse**, alors la soupe végétarienne est préférée à la soupe de poisson.

#### Préférences conditionnelles sur le vin :
- **Langage naturel** : "Je préfère accompagner une soupe végétarienne avec du vin rouge."
- **Formulation formelle** : \( V \land R > V \land B \), ce qui signifie que le vin rouge est préféré au vin blanc lorsqu'il accompagne une soupe végétarienne.

---

### **Étape 5 : Vérification et résumé des préférences**

Une fois que toutes les préférences ont été traduites en relations formelles, il est important de **vérifier** que toutes les préférences sont bien représentées. Ensuite, on peut les résumer dans une liste organisée.

#### Résumé des préférences formalisées :
1. **\( P > V \)** : "Je préfère la soupe de poisson à la soupe végétarienne."
2. **\( C \land V > C \land P \)** : "Lorsque l’entrée est copieuse, je préfère la soupe végétarienne à la soupe de poisson."
3. **On ne peut pas formaliser** : "Lorsque l’entrée est à base de fruits de mer, aucune préférence ne peut être formalisée."
4. **\( V \land R > V \land B \)** : "Je préfère accompagner une soupe végétarienne avec du vin rouge."
5. **\( P \land B > P \land R \)** : "Je préfère accompagner une soupe de poisson avec du vin blanc."

---

### **Conclusion**

Cette méthode vous permet de formaliser des préférences exprimées en langage naturel en utilisant des notations logiques et mathématiques. Chaque étape est importante pour structurer les choix de l'utilisateur de manière claire, et elle peut être réutilisée pour d'autres types de préférences conditionnelles.

En résumé :

1. **Identifiez les variables** (ce sont les options à choisir) et leurs **domaines** (les valeurs possibles).
2. **Traduisez les préférences** en relations strictes.
3. **Gérez les incomparabilités** (situations où il est impossible de formuler une préférence).
4. **Formulez les préférences conditionnelles** en utilisant "si... alors...".
5. **Vérifiez et résumez** toutes les préférences sous une forme claire et logique.
---
## **Méthodologie de Résolution des Exercices de Préférences Conditionnelles** (EXO 2 et 3)

La résolution des exercices de **préférences conditionnelles** consiste à formuler et analyser des relations entre des options ou des combinaisons d’options, dans le but de déterminer lesquelles sont préférées, en fonction de certaines conditions. Voici une explication détaillée et simplifiée, étape par étape, pour aider à comprendre comment résoudre ce type d'exercices, même si vous êtes débutant et que vous ne connaissez pas le cours.

---

### **Définitions de base**

1. **Préférence conditionnelle** :
    
    - Une **préférence** est une relation entre deux options ou combinaisons d’options où l'une est **préférée** à l'autre.
    - Exemple : dire **"α > β"** signifie que l'option **α** est préférée à l'option **β**.
2. **Combinaison d’options (outcome)** :
    
    - Les **options** ou **combinaisons d’options** sont des situations définies par la valeur des variables propositionnelles.
    - Par exemple, si **α** et **β** sont deux choix possibles, alors **α ∧ β** signifie que **α** et **β** sont tous deux vrais (vérifiés). De la même façon, **¬α ∧ β** signifie que **α** est faux mais **β** est vrai.
3. **Sémantique optimiste** :
    
    - Dans la **sémantique optimiste**, une option est préférée si **au moins une de ses variantes** est meilleure que toutes les variantes d'une autre option.

---

### **Étapes de résolution d’un exercice de préférences conditionnelles**

Nous allons détailler ici la méthode en cinq étapes principales :

1. **Identifier les options et leurs combinaisons possibles**
2. **Traduire les préférences en relations formelles**
3. **Analyser les préférences pour chaque sous-ensemble (S1, S2, S3, etc.)**
4. **Calculer les résultats préférés et non préférés**
5. **Conclure sur les résultats préférés sans conflit**

---

### **Étape 1 : Identifier les options et leurs combinaisons possibles**

Chaque exercice commence par identifier **les variables propositionnelles** (comme **α**, **β**, etc.). À partir de ces variables, on détermine toutes les **combinaisons possibles** de leurs valeurs (vrai ou faux). Ces combinaisons sont appelées des **outcomes** ou résultats.

- Si on a deux variables (comme **α** et **β**), alors les combinaisons possibles sont :
    - **ω0** = α ∧ β (les deux variables sont vraies)
    - **ω1** = α ∧ ¬β (seule **α** est vraie)
    - **ω2** = ¬α ∧ β (seule **β** est vraie)
    - **ω3** = ¬α ∧ ¬β (aucune des deux n'est vraie)

Ainsi, l'ensemble des combinaisons possibles est souvent noté **Ω**.

**Exemple pour deux variables** :  
Ω = { ω0 = α ∧ β, ω1 = α ∧ ¬β, ω2 = ¬α ∧ β, ω3 = ¬α ∧ ¬β }

---

### **Étape 2 : Traduire les préférences en relations formelles**

L’énoncé vous donne des **préférences conditionnelles** à traduire sous forme de relations. Cela indique **quelle combinaison** (outcome) est préférée à une autre.

- Si l'énoncé vous dit que **"α > ¬α ∧ β"**, cela signifie que **α** est préféré à la situation où **α** est faux mais **β** est vrai.  
    Forme traduite : **ω0 > ω2** (car **α ∧ β** est préféré à **¬α ∧ β**).

Répétez cette traduction pour chaque préférence donnée dans l'énoncé.

---

### **Étape 3 : Analyser les préférences pour chaque sous-ensemble**

Nous allons maintenant organiser les préférences en **sous-ensembles** en fonction des règles de préférence. Ces sous-ensembles sont notés **S1, S2, S3**, etc. Chaque sous-ensemble contient une préférence et les combinaisons associées.

- Par exemple, si **S1** contient la préférence **"α > ¬α ∧ β"**, alors on analyse **S1** en trouvant quel résultat (outcome) est préféré et quel résultat ne l'est pas.

Cela se fait pour chaque sous-ensemble de préférence :

- **S1** : α > ¬α ∧ β ⇒ ω0 > ω2
- **S2** : α ∧ β > α ∧ ¬β ⇒ ω0 > ω1
- **S3** : ¬α ∧ ¬β > α ∧ β ⇒ ω3 > ω0

---

### **Étape 4 : Calculer les résultats préférés et non préférés**

Pour chaque sous-ensemble, on calcule :

- **L(S)** : l'ensemble des **résultats préférés**
- **R(S)** : l'ensemble des **résultats non préférés**

**Exemple pour un sous-ensemble S1** :  
Si **S1** a la préférence **"α > ¬α ∧ β"**, alors on écrit :

- **L(S1) = { ω0 }** (ω0 est préféré)
- **R(S1) = { ω2 }** (ω2 est moins préféré)

Faites cela pour chaque sous-ensemble.

---

### **Étape 5 : Conclure sur les résultats préférés sans conflit**

Une fois que tous les sous-ensembles ont été analysés, nous regroupons les résultats. L’objectif est de trouver les **résultats préférés** qui **ne sont pas en conflit**.

- **E1** : ensemble des résultats **sans conflit** (les résultats préférés par rapport à toutes les autres options). Ce sont souvent les résultats finaux les plus importants.
- **E2** : ensemble des résultats **en conflit** avec d'autres préférences.

**Exemple final** :

- **E1 = {ω3}** (c'est le résultat préféré sans conflit)
- **E2 = {ω0}** (en conflit avec ω3)

---

### **Résumé de la méthode**

1. **Identifier les combinaisons possibles** des options (les outcomes).
2. **Traduire les préférences** en relations formelles.
3. **Analyser chaque sous-ensemble** de préférence (S1, S2, S3).
4. **Calculer les résultats préférés** et non préférés (L(S) et R(S)).
5. **Conclure** sur les résultats préférés sans conflit (E1).


