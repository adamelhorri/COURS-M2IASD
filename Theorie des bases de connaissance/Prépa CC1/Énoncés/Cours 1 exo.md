---
tags:
  - TBD
---
### Exercices (HAI933I)

#### Exercice 1 (Fragment existentiel conjonctif)

- [ ] On considère les formules fermées suivantes (où `a` est une constante et les autres termes sont des variables) :
  - **F1** : ∃x1∃y1∃z1 (p(x1, y1) ∧ q(y1, z1) ∧ r(z1))
  - **F2** : ∃x2∃y2∃z2∃v2 (r(x2) ∧ p(x2, y2) ∧ q(y2, x2) ∧ q(y2, z2) ∧ p(z2, v2) ∧ q(v2, z2))
  - **F3** : ∃x3∃y3 (p(a, x3) ∧ q(x3, y3))
  - **F4** : ∃x4 (p(a, x4) ∧ q(x4, a))
  - **F5** : ∃x5∃y5 (p(y5, x5) ∧ q(x5, y5))
  - **F6** : ∃x6∃y6∃z6 (p(x6, y6) ∧ q(y6, x6) ∧ q(y6, z6) ∧ r(x6))
  - **F7** : ∃x7∃y7∃z7 (p(x7, y7) ∧ q(y7, x7) ∧ q(y7, z7) ∧ r(z7))

**Questions :**
1. [ ] Classifiez ces formules selon la relation “s’envoie par homomorphisme dans”.
2. [ ] À partir de votre réponse à la question précédente, déterminez les relations de conséquence logique entre ces formules.
3. [ ] Y a-t-il des formules équivalentes ? Deux formules `f` et `g` sont équivalentes si `f |= g` et `g |= f`, c’est-à-dire si `f` et `g` s’envoient l’une dans l’autre par homomorphisme.
4. [ ] Y a-t-il des formules redondantes (ou non minimales) ? Une formule vue comme un ensemble d’atomes est redondante si elle est équivalente à un sous-ensemble strict de ses atomes, c’est-à-dire si on peut lui enlever au moins un atome en gardant une formule équivalente.

#### Exercice 2 (Inclusion de requêtes conjonctives)

- [ ] Étant données deux requêtes conjonctives `Q1` et `Q2`, on dit que `Q1` est incluse dans `Q2` (notation `Q1 ⊆ Q2`) si, pour toute base de faits `F`, on a `Q1(F) ⊆ Q2(F)`, c’est-à-dire si l’ensemble des réponses à `Q1` dans `F` est inclus dans l’ensemble des réponses à `Q2` dans `F`.

**Questions :**
1. [ ] Reformulez la définition d’inclusion pour des requêtes booléennes en termes de réponse oui ou non. Quelles sont les relations d’inclusion (`⊆`) entre les formules de l’exercice 1 vues comme des requêtes ?
2. [ ] Montrez la propriété suivante : étant données des requêtes booléennes `Q1` et `Q2`, on a `Q1 ⊆ Q2` si et seulement s’il existe un homomorphisme de `Q2` dans `Q1`.
3. [ ] Définissez l’homomorphisme de requêtes conjonctives quelconques (c’est-à-dire avec un nombre quelconque de variables réponses), puis étendez la propriété aux requêtes conjonctives quelconques en utilisant cette notion.

#### Exercice 3 (Complexité du test d’homomorphisme)

- [ ] On appelle `HOM` le problème suivant : étant donnés deux ensembles d’atomes `Q` et `F`, déterminer s’il existe un homomorphisme de `Q` dans `F`. On considère un schéma d’algorithme naïf résolvant `HOM` :
  - **Pour toute application `h` de variables(`Q`) dans termes(`F`)**
  - **Si `h(Q) ⊆ F`, retourner vrai**
  - **Sinon, retourner faux**

**Questions :**
1. [ ] Combien y a-t-il d’applications (substitutions) `h` à tester ?
2. [ ] Montrez que le problème `HOM` est NP-complet :
   - [ ] Montrez que `HOM` est dans `NP`, c’est-à-dire qu’on peut déterminer en temps polynomial si une prétendue solution en est bien une (notion de “certificat polynomial”).
   - [ ] Montrez qu’il existe une réduction polynomiale d’un problème NP-complet à `HOM`. Considérez par exemple le problème NP-complet 3-coloration d’un graphe : étant donné un graphe `G = (V, E)`, existe-t-il une coloration des sommets de `G` avec 3 couleurs, c’est-à-dire une application `c : V → {1, 2, 3}` telle que pour tout `xy ∈ E`, `c(x) ≠ c(y)` ?
3. [ ] En théorie des bases de données, on considère deux mesures de complexité pour les problèmes faisant intervenir des requêtes et des bases de données : la complexité combinée (toutes les données sont prises en compte) et la complexité de données (on ne considère que la base de données, `Q` étant fixée). `HOM` est-il polynomial en complexité de données ?
