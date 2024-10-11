---
tags:
  - tps
  - aideDecision
---


# Exercices

## **Exercice 1**  
Soient les préférences suivantes exprimées par un utilisateur :  
Je préfère une soupe de poisson à une soupe végétarienne. Cependant, cette préférence est inversée lorsque l’entrée est copieuse. La préférence n’est pas vraie lorsque l’entrée est à base de fruits de mer, qu’elle soit copieuse ou non. Ma préférence entre le vin rouge et le vin blanc est conditionnée par la soupe servie. Je préfère accompagner une soupe végétarienne de vin rouge et une soupe de poisson de vin blanc.

**Questions :**
1. Donner l’ensemble des variables et leurs domaines.
2. Formaliser les préférences ci-dessus exprimées en langage naturel.
D'accord, reprenons l'exercice 1 en entier, en suivant précisément le format et les étapes que tu m'as indiquées.

### Resolution **Exercice 1 - Préférences exprimées en langage naturel :**


#### 1. Donner l’ensemble des variables et leurs domaines.

- **\( S \)** (soupe) : {**P** (Poisson), **V** (Végétarienne)}
- **\( E \)** (entrée) : {**C** (Copieuse), **N** (Non copieuse), **F** (Fruits de mer)}
- **\( V \)** (vin) : {**R** (Rouge), **B** (Blanc)}

#### 2. Formaliser les préférences exprimées en langage naturel.

### Préférences formalisées :

1. **\( P > V \)**  
   Traduction : "Je préfère la soupe de poisson à la soupe végétarienne."
   
   **Formalisme** :  
   - \( P \) pour la soupe de poisson.  
   - \( V \) pour la soupe végétarienne.  
   - \( P > V \) signifie que la soupe de poisson est préférée à la soupe végétarienne.

2. **\( C∨V > C \∧ P \)**  
   Traduction : "Lorsque l’entrée est copieuse, je préfère la soupe végétarienne à la soupe de poisson."
   
   **Formalisme** :  
   - \( C \) pour entrée copieuse.  
   - \( V > P \) si \( C \), c'est-à-dire que la soupe végétarienne est préférée à la soupe de poisson quand l’entrée est copieuse.

3. **On ne peut pas formuler la préférence**  
   Traduction : "Lorsque l’entrée est à base de fruits de mer, qu’elle soit copieuse ou non, aucune préférence n’est exprimée."

   **Explication** :  
   - On ne peut pas exprimer cette préférence car il n'y a pas de relation stricte entre les choix dans ce contexte. Il y a incomparabilité dans ce cas, donc il est impossible de formaliser cette préférence dans un cadre strict.

4. **Préférences sur le vin conditionnées par la soupe :**

   - **\( V \∧ R > V \∧ B \)**  
     Traduction : "Je préfère accompagner une soupe végétarienne avec du vin rouge."
     
     **Formalisme** :  
     - \( V \∧ R > V \∧ B \) signifie que le vin rouge est préféré au vin blanc lorsqu'il accompagne une soupe végétarienne.

   - **\( P \∧ B > P \∧ R \)**  
     Traduction : "Je préfère accompagner une soupe de poisson avec du vin blanc."
     
     **Formalisme** :  
     - \( P \∧ B > P \∧ R \) signifie que le vin blanc est préféré au vin rouge lorsqu'il accompagne une soupe de poisson.

---

### Résumé final dans le format structuré

1. **\( P > V \)** : "Je préfère la soupe de poisson à la soupe végétarienne."
2. **\( C \∧ V > C \∧ P \)** : "Lorsque l’entrée est copieuse, je préfère la soupe végétarienne à la soupe de poisson."
3. **On ne peut pas formaliser** : "Lorsque l’entrée est à base de fruits de mer, aucune préférence ne peut être formalisée."
4. **\( V \∧ R > V \∧ B \)** et **\( P \∧ B > P \∧ R \)** : "Je préfère accompagner une soupe végétarienne avec du vin rouge et une soupe de poisson avec du vin blanc."

## **Exercice 2**  
Soient les préférences conditionnelles suivantes (**α** et **β** sont des variables propositionnelles) :

- α ∧ β B ¬α
- α ∧ ¬β B α ∧ β
- ¬α ∧ ¬β B α ∧ β

**Question :**  
Calculer la relation de préférences associée à cet ensemble de préférences suivant la sémantique optimiste.

D'accord, je vais réécrire la solution sans utiliser de mathématiques complexes ou de LaTeX, uniquement avec des caractères lisibles en markdown.

### **Resolution exercice 2 :**

#### Préférences conditionnelles données :
1. α ∧ β > ¬α
2. α ∧ ¬β > α ∧ β
3. ¬α ∧ ¬β > α ∧ β

---

### **Étape 1 : Ensemble des résultats possibles (Ω)**

Les combinaisons possibles de α et β sont :

- ω0 = α ∧ β
- ω1 = α ∧ ¬β
- ω2 = ¬α ∧ β
- ω3 = ¬α ∧ ¬β

Donc, l'ensemble des résultats possibles est :  
**Ω = { ω0, ω1, ω2, ω3 }**

---

### **Étape 2 : Ensemble des préférences (P)**

Les préférences conditionnelles se traduisent par les relations suivantes :

1. α ∧ β > ¬α : Cela implique que ω0 > ω2 et ω0 > ω3
2. α ∧ ¬β > α ∧ β : Cela implique que ω1 > ω0
3. ¬α ∧ ¬β > α ∧ β : Cela implique que ω3 > ω0

Ainsi, **P = { ω0 > ω2, ω0 > ω3, ω1 > ω0, ω3 > ω0 }**

---

### **Étape 3 : Utilisation de α(S) et R(S)**

Nous allons partitionner les sous-ensembles S1, S2 et S3 pour chaque préférence, puis identifier les résultats préférés (α(S)) et les résultats rejetés (R(S)).

#### Sous-ensemble S1 : α ∧ β > ¬α
- **Résultat préféré :** α(S1) = {ω0}
- **Résultats non préférés (R(S1)) :** R(S1) = {ω2, ω3}

#### Sous-ensemble S2 : α ∧ ¬β > α ∧ β
- **Résultat préféré :** α(S2) = {ω1}
- **Résultats non préférés (R(S2)) :** R(S2) = {ω0}

#### Sous-ensemble S3 : ¬α ∧ ¬β > α ∧ β
- **Résultat préféré :** α(S3) = {ω3}
- **Résultats non préférés (R(S3)) :** R(S3) = {ω0}

---

### **Étape 4 : Analyse des résultats et ensemble α(P)**

L'ensemble des résultats préférés est obtenu par l'analyse des résultats préférés α(S1), α(S2), et α(S3), ainsi que les résultats rejetés R(Si).

**α(P) = { (α(S1), R(S1)), (α(S2), R(S2)), (α(S3), R(S3)) }**  
Ce qui donne :  
**α(P) = { ({ω0}, {ω2, ω3}), ({ω1}, {ω0}), ({ω3}, {ω0}) }**

---

### **Étape 5 : Résolution finale en utilisant α(P)**

#### Analyse des préférences conflictuelles :

- Pour S1, ω0 est le résultat préféré avec ω2 et ω3 comme résultats non préférés.
- Pour S2, ω1 est préféré à ω0.
- Pour S3, ω3 est préféré à ω0.

#### Conclusion sur les ensembles E1 et E2 :

- **E1 = {ω2}** : Résultat préféré sans conflit.
- **E2 = {}** : Aucun autre résultat sans conflit.

---

## **Exercice 3**  
Soient les préférences conditionnelles suivantes (**α**, **β** et **γ** sont des variables propositionnelles) :

- α B ¬α ∧ β
- α ∧ β B α ∧ ¬β
- ¬α ∧ ¬β B α ∧ β

**Question :**  
Calculer la relation de préférences associée à cet ensemble de préférences suivant la sémantique optimiste.

---
### Resolution
### **Préférences conditionnelles données :**

1. α > ¬α ∧ β
2. α ∧ β > α ∧ ¬β
3. ¬α ∧ ¬β > α ∧ β

---

### **Étape 1 : Ensemble des résultats possibles (Ω)**

Les combinaisons possibles de **α** et **β** sont :

- **ω0** = α ∧ β
- **ω1** = α ∧ ¬β
- **ω2** = ¬α ∧ β
- **ω3** = ¬α ∧ ¬β

Donc, l'ensemble des résultats possibles est :  
**Ω = { ω0, ω1, ω2, ω3 }**

---

### **Étape 2 : Construction de l'ensemble des préférences (P)**

Les préférences conditionnelles données dans l'énoncé se traduisent par les relations suivantes :

1. **S1 : α > ¬α ∧ β**  
    Cela signifie que **ω0 > ω2**.
    
2. **S2 : α ∧ β > α ∧ ¬β**  
    Cela signifie que **ω0 > ω1**.
    
3. **S3 : ¬α ∧ ¬β > α ∧ β**  
    Cela signifie que **ω3 > ω0**.
    

Nous avons donc :  
**P = { ω0 > ω2, ω0 > ω1, ω3 > ω0 }**

---

### **Étape 3 : Analyse des sous-ensembles S1, S2, S3**

Pour chaque sous-ensemble, nous allons identifier les résultats préférés (L(S)) et les résultats non préférés (R(S)).

#### Sous-ensemble S1 : α > ¬α ∧ β

- **Résultat préféré (L(S1)) :** {ω0, ω1}
- **Résultat non préféré (R(S1)) :** {ω2}

#### Sous-ensemble S2 : α ∧ β > α ∧ ¬β

- **Résultat préféré (L(S2)) :** {ω0}
- **Résultat non préféré (R(S2)) :** {ω1}

#### Sous-ensemble S3 : ¬α ∧ ¬β > α ∧ β

- **Résultat préféré (L(S3)) :** {ω3}
- **Résultat non préféré (R(S3)) :** {ω0}

---

### **Étape 4 : Conclusion avec les ensembles E1, E2 et E3**

#### E1 : {ω3}

- Ce résultat est préféré sans conflit avec les autres résultats.

#### E2 : {ω0}

- Ce résultat est préféré mais en conflit avec ω3.

#### E3 : {ω1, ω2}

- Ces résultats sont moins préférés.

## **Exercice 4**  
Soient **A**, **B** et **C** trois variables binaires telles que **Dom(A) = {a, ¬a}**, **Dom(B) = {b, ¬b}** et **Dom(C) = {c, ¬c}**. Soit un CP-net sur **A**, **B** et **C** dont les préférences locales sont les suivantes :

- ¬a est préféré inconditionnellement à a,
- b est préféré inconditionnellement à ¬b,
- ¬c est préféré à c si a et b ont la même valeur de vérité. La préférence est inversée dans le cas contraire.

**Questions :**
1. Construire le CP-net associé à ces préférences.
2. Donner la relation de préférence associée.

---

## **Exercice 5**  
Soient **A**, **B** et **C** trois variables binaires telles que **Dom(A) = {a, ¬a}**, **Dom(B) = {b, ¬b}** et **Dom(C) = {c, ¬c}**. Soit un CP-net sur **A**, **B** et **C** dont les préférences locales sont les suivantes :

- ¬a est préféré inconditionnellement à a,
- b est préféré à ¬b si a est vrai. La préférence est inversée dans le cas contraire,
- ¬c est préféré à c si a est vrai. La préférence est inversée dans le cas contraire.

**Questions :**
1. Construire le CP-net associé à ces préférences.
2. Donner la relation de préférence associée.

---