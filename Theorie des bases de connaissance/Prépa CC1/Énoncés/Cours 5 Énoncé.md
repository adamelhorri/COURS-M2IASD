---
tags:
  - TBD
---

1. **Compréhension des Conjunctive Queries (CQs)**
   - [ ] Définissez une requête conjonctive (CQ) et expliquez sa structure générale (conjonction d'atomes et quantificateurs existentiels externes).
   - [ ] Donnez un exemple de CQ simple pour trouver toutes les lignes de bus accessibles à partir d'un arrêt spécifique : `∃yID, yStop, yTo.Stops(yID, yStop,"true") ∧ Connect(yID, yTo, xLine)`.

2. **Formulation des CQs**
   - [ ] En utilisant la table `Films`, traduisez les requêtes suivantes en CQs :
      - Listez les films diffusés à "Diagon".
      - Trouvez les heures de diffusion du film "Dogma" à "Diagon".
      - Obtenez le numéro de téléphone de "Diagon".

3. **Résolution de Sudoku avec les BCQs**
   - [ ] Exprimez un Sudoku 4x4 comme une BCQ en spécifiant un modèle logique où chaque cellule de la grille correspond à une permutation unique des valeurs `{1, 2, 3, 4}` et les règles de remplissage respectent les contraintes du Sudoku.
   - [ ] Décrivez le schéma de la base de données (relation `r`) et la forme de la requête `q` qui valide la solution.

4. **Compréhension des Boolean Conjunctive Queries (BCQs)**
   - [ ] Définissez une BCQ et donnez un exemple de requête booléenne demandant si un arrêt accessible a des lignes qui y partent.
   - [ ] Expliquez comment interpréter les réponses booléennes : vide (faux) ou ensemble unitaire `{⟨⟩}` (vrai).

5. **Évaluation des requêtes de premier ordre : Algorithme FO**
   - [ ] Étudiez l'algorithme d'évaluation des FO queries et expliquez chaque étape, en particulier le traitement des cas atomiques, négatifs (`¬ψ`), conjonctifs (`ψ1 ∧ ψ2`) et des quantificateurs existentiels (`∃x.ψ`).
   - [ ] Discutez des limites de cet algorithme en termes de complexité, en analysant la profondeur maximale de récursivité et le nombre d'itérations du `for`.

6. **Exemples de CQs cycliques et acycliques**
   - [ ] Écrivez deux exemples de requêtes conjonctives : une acyclique (`∃yc, ym, yf, ymm, ymf .mother(yc , ym) ∧ father(yc , yf ) ∧ married(ym, ymm) ∧ married(ymf , yf )`) et une cyclique (`∃yc, ym, yf.mother(yc , ym) ∧ father(yc , yf ) ∧ married(ym, yf )`).
   - [ ] Expliquez la différence entre ces requêtes en termes de complexité de traitement et structure du graphe.

7. **Évaluation de l'acyclicité par la réduction de GYO**
   - [ ] Appliquez l'algorithme de réduction de GYO pour déterminer l'acyclicité des CQs suivantes et donnez l'ordre d'élimination ou la réduction de GYO finale :
      - `∃x, y, z, w. r(x, w) ∧ r(y, w) ∧ r(z, w)`
      - `∃x, y, z. r(x, y) ∧ r(y, z) ∧ r(z, x)`.

8. **Définition des arbres de jointure (Join Trees)**
   - [ ] Définissez un arbre de jointure (join tree) pour une CQ et expliquez comment il est utilisé pour caractériser une CQ acyclique.
   - [ ] Pour la requête `∃x, y, z, v. r(x, y) ∧ r(y, z) ∧ r(z, v) ∧ s(x, y, z) ∧ s(y, z, v)`, construisez un arbre de jointure correspondant.

9. **Utilisation de l’algorithme de Yannakakis pour les CQs acycliques**
   - [ ] Exécutez l'algorithme de Yannakakis pour une CQ acyclique en suivant les étapes de `leaf-to-root` et `root-to-leaf` en remplaçant chaque relation par son semijoin avec son parent.
   - [ ] Expliquez comment cet algorithme permet d'optimiser l'évaluation de CQs en éliminant les faits non pertinents pour les réponses finales.

10. **Évaluation des CQs avec des constantes**
    - [ ] Expliquez comment gérer les CQs booléennes avec des constantes, comme `∃x, y, z. mother(x, y) ∧ father(x, z) ∧ bornIn(y, "Montpellier") ∧ bornIn(z, "Montpellier")`.
    - [ ] Décrivez les étapes pour vérifier l'acyclicité en ignorant les constantes et en appliquant des filtres et projections pour les éléments constants.

11. **Utilisation de la réduction GYO' avec les oreilles (ears)**
    - [ ] Définissez une oreille dans un hypergraphe et appliquez la procédure de réduction GYO' (oreille par oreille) pour les hypergraphes de la requête `∃x, y, z, v. r(x, y) ∧ r(y, z) ∧ r(z, v) ∧ s(x, y, z) ∧ s(y, z, v)`.

12. **Comparaison des techniques d'optimisation pour les CQs cycliques et acycliques**
    - [ ] Comparez la complexité de l’algorithme de Yannakakis pour les CQs acycliques avec d’autres méthodes d'évaluation des requêtes cycliques. Donnez un exemple de CQ cyclique et discutez des limites d'efficacité lorsque la CQ est transformée en une requête acyclique via l’ajout d’atomes.
