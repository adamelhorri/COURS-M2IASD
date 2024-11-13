---
tags:
  - TBD
---

### Exercices d'application (Database Theory and Knowledge Representation - 2nd Lecture)

1. **Compréhension des requêtes relationnelles**
   - [ ] Définissez ce qu’est une requête relationnelle en termes de syntaxe et de sémantique, en expliquant comment une expression de requête (`q`) est appliquée à une instance de base de données (`I`) pour produire une table de résultats.

2. **Exemples de Requêtes de Premier Ordre (FO)**
   - [ ] Utilisez les formules de premier ordre pour exprimer les requêtes suivantes :
      - Trouvez toutes les lignes de bus : `Lines(x, bus)`.
      - Listez tous les types possibles de lignes : `∃y.Lines(y, x)`.
      - Identifiez les lignes partant d’un arrêt accessible : `∃ySID, yStop, yTo. Stops(ySID, yStop, true) ∧ Connect(ySID, yTo, xLine)`.

3. **Syntaxe et Simplification en Logique du Premier Ordre**
   - [ ] Écrivez des formules en logique du premier ordre en utilisant les simplifications standards :
      - Conjonctions et disjonctions plates, quantificateurs multiples (`∃x, y, z.φ`), implications (`φ → ψ`) et biconditionnels (`φ ↔ ψ`).
      - Simplifiez l’expression suivante : `(φ1 ∧ (φ2 ∧ φ3))` en `φ1 ∧ φ2 ∧ φ3`.

4. **Sémantique des Formules de Premier Ordre**
   - [ ] Évaluez la satisfaction d'une formule logique `φ` par un ensemble de faits `I` et une assignation de variables `Z`.
   - [ ] Pour chaque sous-formule atomique de `φ`, expliquez comment vérifier la satisfaction ou non de cette formule sous les opérations logiques (`∧`, `∨`, `¬`, etc.) et les quantificateurs (`∃`, `∀`).

5. **Exercices sur les Requêtes de Premier Ordre**
   - [ ] Traduisez les requêtes suivantes en expressions de logique du premier ordre :
      - Qui est le directeur de "The Imitation Game" ?
      - Quels cinémas diffusent "The Imitation Game" ?

6. **Requêtes Booléennes**
   - [ ] Décrivez ce qu'est une requête booléenne et expliquez comment interpréter les résultats possibles `{⟨⟩}` ou `∅`.
   - [ ] Créez une requête booléenne qui vérifie si un film dirigé par "Smith" est actuellement en programme dans un cinéma.

7. **Traduction des Requêtes FO vers le Calcul Relationnel (RA)**
   - [ ] Traduisez les requêtes FO suivantes en expressions équivalentes dans le calcul relationnel (RA) :
      - `∃y.R(x, a, y)`
      - `R(x, y, w) ∧ x ≈ y`
      - `∃y, z. R(x, y, z) ∧ S(y, z)`
      - `A(x) ∧ ∀y.¬S(x, y)`

8. **Calcul de la Complexité des Requêtes FO**
   - [ ] Utilisez la complexité combinée et la complexité en fonction des données pour estimer le temps d'exécution des requêtes FO en fonction de la taille `m` de la formule et de la taille `n` de la base de données (`I`).
   - [ ] Analysez la complexité de mémoire des requêtes FO et justifiez pourquoi elles sont classées en `PSpace-complet` pour la complexité combinée.

9. **Comparaison de Langages de Requêtes**
   - [ ] Montrez l’équivalence entre le calcul relationnel (RA) et les requêtes de premier ordre (FO) en expliquant comment une requête RA donnée peut être traduite en une requête FO équivalente, et inversement.
   - [ ] Donnez un exemple de sous-langage du RA (`RA\∩`) et montrez qu’il est équivalent au RA complet en utilisant une opération jointe comme `q ∩ s ≡ q ▷◁ s`.

10. **Exercices avancés sur les Requêtes**
    - [ ] Exprimez les requêtes suivantes en logique du premier ordre, en utilisant les tables `Films`, `Venues`, et `Program` :
      - Listez les directeurs ayant également joué dans un film qu'ils ont réalisé : `∃yT. Films(yT, xD, xD)[xD]`.
      - Trouvez les acteurs n'ayant pas travaillé avec "Smith" : `∀zT , zD. (Films(zT, zD, xA) → zD ̸≈ "Smith")`.
      - Identifiez tous les acteurs ayant travaillé avec exactement deux directeurs différents.
    
11. **Résolution des Problèmes de Décision des Requêtes**
    - [ ] Formulez le problème de l’entailment des requêtes booléennes (`I |= φ`) et expliquez pourquoi il s’agit d’un problème de décision.
    - [ ] Formulez et résolvez le problème de réponse à une requête FO `q` pour une instance de base de données `I` et un tuple donné `⟨c1, ..., cn⟩`, en vérifiant si ce tuple appartient au résultat de `q` sur `I`.
