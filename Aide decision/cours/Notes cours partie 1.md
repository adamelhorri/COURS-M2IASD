---
tags:
  - aideDecision
  - cours
---

# [[Partie 1 Cours]]

### Plan du cours
- **Projet de fin de semestre** : Rapport + Présentation (30 %)
- **Thèmes abordés** :
  - Modélisation
  - Représentation
  - Raisonnement
  - Décision

---

## 1) Modélisation

### Résultats (Outcomes dans les mondes possibles)
- **Ensemble des variables** : Les variables prennent des valeurs dans leurs **domaines**.
-   \Pi_{i=1}^n Dom(X_i)    : Ensemble des outcomes possibles.
-   \cap    : Ensemble des outcomes faisables.
-   \omega    : Un outcome spécifique.

### Priorités des outcomes
- Tous les outcomes n’ont pas la même priorité.
- Exemple :   c_1 \succ c_2   , où je préfère le créneau   c_1    au créneau   c_2   .

---

## 2) Représentation des préférences

### Probabilité vs Plausibilité

```
           Probabilité                     Plausibilité
               \                                /
               \___ Représentation des préférences __/
                        |                   |
               Ordinale                   Cardinale
```

---

### Représentation des préférences

1. **Ordinale** : Relations qualitatives de préférence.
2. 
   -   \omega_1 \leq \omega_2    :   \omega_1    est au moins aussi préféré que   \omega_2   .
   -   \omega_1 \sim \omega_2    :   \omega_1    et   \omega_2    sont incomparables.
   - **Relation de préférence** : 
     -   \geq    : Préordre (réflexif et transitif).
     -   >    : Ordre (irréflexif et transitif).

3. **Cardinale** : Utilisation d'une fonction numérique   u omega)    pour mesurer les préférences.
   - Impossible d'exprimer l'incomparabilité dans cette représentation (tout est comparable).

---

## 3) Relation de préférence

- **Préordre** : Réflexif et transitif.
- **Ordre** : Irréflexif et transitif.
- **Relation partielle** : Il existe des incomparabilités.
- **Relation complète** : Tous les outcomes sont comparables, ce qui permet une stratification des outcomes.

---

## 4) Stratification des outcomes

### Exemple de relation complète et stratification :

```plaintext
Strate 0:      ω3
Strate 1:      ω1, ω0
Strate 2:      ω2
```

-   \omega_1 \succ \omega_2 \succ \omega_3 \succ \omega_4 \succ \omega_5   .

### Exemple avec préordre partiel (incomparabilité présente) :

```plaintext
S0 : ω1, ω3
S1 : ω2
```

### Exemple avec un ordre complet (chaque outcome dans une strate unique) :

```plaintext
ω1 > ω0 > ω3 > ω2
```

---

## 5) Représentation cardinale

- **Fonction de préférence**   u omega)    : On associe une valeur numérique à chaque outcome.

### Table des préférences cardinales

| Outcome  ( \omega   ) |   u omega)    |
|------------------------|-----------------|
|   \omega_1            | 0.8             |
|   \omega_2            | 0.7             |
|   \omega_3            | 0.5             |
|   \omega_4            | 0.4             |

- **Relation de préférence basée sur la fonction   u   ** :
  -   \omega_1 > \omega_2    si   u omega_1) > u omega_2)   .
  -   \omega_1 \sim \omega_2    si   u omega_1) = u omega_2)   .

---

## 6) Élicitation des préférences

- **Préférences contextuelles** (ou conditionnelles) :
  - Exemple : "Je préfère   A    à   B   , **si**   C    est vrai".
  - Utilisation dans le **machine learning** ou le **data mining** lorsqu'on a des données massives.

- **Élicitation itérative** : Par questionnaire, pour obtenir les préférences des utilisateurs.

---

## 7) Langages de représentation compacte

- **Relations partielles** :
  - **Qualitatif** : On ne donne pas explicitement les poids des préférences.
  - **Quantitatif** : On peut exprimer des poids pour les préférences.

---

## 8) Logique possibiliste

- La **logique possibiliste** est utilisée pour gérer les préférences incertaines.
- Exemple d'un système de croyance avec des formules pondérées en plausibilité :
$$ 
  
\pi omega) = 
\begin{cases} 
1 & \text{si } \omega \text{ est pleinement plausible}, \\
0 & \text{si } \omega \text{ est impossible}.
\end{cases}
  
$$ 
---

## Exemple d'exercice d'application

### Situation

Nous avons une base de croyance :  
$$ 
  
\Sigma = \{ (p, 1), (¬p \lor b, 0.8), (¬p \lor ¬f, 0.8), (¬b \lor f, 0.4), (¬b \lor w, 0.4) \}
  
$$ 
### Table des plausibilités :

| Outcome  ( \omega   )       | Plausibilité |
|------------------------------|--------------|
|   \omega_0 = p, b, f, w     |   1 - \max(0.8, 0.4) = 0.1    |
|   \omega_1 = p, ¬b, f, ¬w    | 0.1          |
|   \omega_2 = ¬p, b, f, ¬w    | 0.7          |
|   \omega_3 = ¬p, ¬b, f, w    | 0.5          |

- **Conclusion** : On ne peut pas obtenir une plausibilité de 1 car la base est contradictoire.

Je comprends que tu souhaites une transcription exacte et complète des notes que tu as fournies. Je vais donc réécrire les notes dans leur intégralité, sans raccourcir ni simplifier le contenu. Voici les notes telles qu'elles apparaissent dans les images, avec tous les détails, les schémas et les tableaux inclus.

---

### CM2 : **Conditional Logics** (Logique Comparée) - 18/09

#### Modélisation : Écriture mathématique des relations
- **Cardinal** :   u omega) \Rightarrow p   
- **Ordinal** :   \geq    (ordre)
- **Ordre partiel** : Au moins un couple incomparable
- **Ordre complet**
- **Information partielle compacte** : information = **préférence** / **croyance**
  - **Représentation logique** (logique pondérée, possibiliste, pénalité)
  - **Représentation graphique**

---

### Ceteris Paribus Sémantiques
$$ 
  
\Omega = \{ p \land q, p \land \neg q, \neg p \land q, \neg p \land \neg q \}
  
$$ 
#### Exemple :
$$ 
  
p \land q \succ p \land \neg q \succ \neg p \land q \succ \neg p \land \neg q
  
$$ 
1. **Ordre partiel** :$$ 
   -   \omega_1 \sim \omega_2 \sim \omega_3 \sim \omega_4   
   - Il existe des incomparabilités entre certains outcomes.
   - $$ 
   
#### Sémantique Optimiste

- **Optimiste** :
  -   p \succ q    si au moins un outcome de   p    est préféré à tous les outcomes de   q   .
$$  
Exemple :
  
p \succ q
  
$$
#### Sémantique Pessimiste

- **Pessimiste** :
  -   p \succ q    si le pire outcome de   p    est préféré à tous les outcomes de   q   .
$$ 
Exemple :
  
p \succ q
  
$$ 
#### Sémantique Opportuniste

- **Opportuniste** :
  -   p \succ q    si au moins un outcome de   p    est préféré à au moins un outcome de   q   .
$$   
Exemple :
  
p \succ q
  
$$ 
---

### I) **Conditional Logics**

Si   C    est vrai, je préfère   A    à   B   .
$$ 
  
A \succ B
  
$$ 
- **Exemple** :
  - Je préfère les menus où il y a du poisson et du vin blanc aux menus où il y a de la viande et du vin blanc, mais je préfère aussi les menus sans poisson où il n'y a pas de vin blanc.

#### Strong Semantics
$$ 
  
\{ \omega_1, \omega_2 \} \succ \{ \omega_3, \omega_4 \}
  
$$ 
---

### II) **Algorithme**

- Un préordre est associé à un ensemble   P    s'il satisfait chacune des préférences.

Exemple avec des préférences incomparables :
$$ 
  
L(P) = \left( \left( \omega_1, \omega_2, \omega_3 \right) , \left( \omega_3, \omega_4 \right) \right)
  
$$ 
#### Strates et ensembles de préférences :

| Strate | Outcome  ( \omega   ) |
|--------|------------------------|
|   E_1    |   \omega_0              |
|   E_2    |   \omega_1, \omega_3    |
|   E_3    |   \omega_2              |

---

**Représentation Graphique** :
$$
  
p \Rightarrow q
   
$$



