---
tags:
  - TBD
---
1. **Base de faits et variables quantifiées**
    
    - [ ]  Étant donné une base de faits `F` composée des atomes `{movie(m1), movie(m2), actor(a), actor(b), actor(c), play(a,m1), play(a,m2), play(c,x)}`, représentez `F` sous forme logique en utilisant des variables quantifiées existentiellement.
    - [ ]  Décrivez comment `F` peut être interprété comme une conjonction d’atomes avec des variables quantifiées existentiellement.
2. **Requêtes conjonctives**
    
    - [ ]  Considérez une requête conjonctive `Q(x) = ∃y (movie(y) ∧ play(x, y))`. Simplifiez `Q` en notation `{movie(y), play(x, y)}` et expliquez la structure logique de cette requête.
    - [ ]  Reformulez une requête booléenne en utilisant des atomes simples. Par exemple, définissez une requête qui vérifie simplement la présence d’un acteur.
3. **Construction d'un graphe orienté à partir d'un ensemble d'atomes**
    
    - [ ]  Construisez un graphe orienté `G = (V, E)` à partir de l’ensemble d'atomes `{p(x, y), q(y, z), r(z)}`, où chaque atome représente un arc. Indiquez l’ensemble des sommets `V` et l’ensemble des arcs `E`.
    - [ ]  Ajoutez des étiquettes distinctes aux arcs si les prédicats sont multiples, et aux sommets si certains termes sont des constantes.
4. **Transformation en hypergraphe**
    
    - [ ]  Étant donné un ensemble d'atomes `{p(x, y, a), q(y, z)}` avec un prédicat d'arité supérieure à 2, représentez cet ensemble sous forme d’hypergraphe. Associez chaque atome à un hyperarc et chaque terme à un sommet.
    - [ ]  Montrez comment des étiquettes peuvent être ajoutées aux hyperarcs pour représenter les prédicats.
5. **Graphe biparti d’incidence**
    
    - [ ]  Pour l'ensemble d'atomes `{movie(m1), actor(a), play(a, m1)}`, construisez le graphe biparti d’incidence associé, en distinguant les sommets termes et les sommets atomes.
    - [ ]  Assurez-vous que chaque sommet atome est connecté aux sommets termes correspondant à ses arguments. Utilisez des étiquettes pour chaque arête reliant un atome à ses arguments.
6. **Graphe de Gaifman (Primal)**
    
    - [ ]  Étant donné l’ensemble d’atomes `{p(x, y), q(y, z), r(z, x)}`, construisez le graphe de Gaifman (ou primal) associé en connectant deux sommets s’ils appartiennent au même atome.
    - [ ]  Proposez un autre ensemble d’atomes ayant le même graphe primal pour illustrer la structure indépendante de l’arité des prédicats.
7. **Homomorphismes entre graphes**
    
    - [ ]  Donnez deux ensembles d’atomes `F1` et `F2` sous forme de graphes. Vérifiez s’il existe un homomorphisme de `F1` dans `F2` en testant la correspondance des sommets et des arêtes.
    - [ ]  Expliquez comment la présence d’étiquettes dans les graphes peut affecter la possibilité d’un homomorphisme.
8. **Isomorphisme d'ensembles d'atomes**
    
    - [ ]  Pour les ensembles d’atomes `{p(x, y), q(y, z)}` et `{p(a, b), q(b, c)}`, définissez un isomorphisme entre eux et expliquez la notion d’« égalité jusqu'à un renommage bijectif de variables ».
    - [ ]  Donnez un exemple d’ensembles d'atomes qui sont logiquement équivalents mais non isomorphes. Discutez de la signification de cette différence.
9. **Calcul et optimisation du core d'un ensemble d'atomes**
    
    - [ ]  Prenez un ensemble d'atomes `{p(x, y), q(y, z), r(x)}` et identifiez une redondance. Calculez le core de cet ensemble en supprimant les atomes redondants pour obtenir une version minimale.
    - [ ]  Proposez un algorithme pour calculer le core d’un ensemble d’atomes. Indiquez la borne supérieure du nombre de tests d’homomorphismes nécessaires pour réduire au core.
10. **Optimisation de requêtes conjonctives**
    
    - [ ]  Soit `Q1` et `Q2` deux requêtes conjonctives définies par des ensembles d’atomes. Déterminez si `Q1` est plus spécifique que `Q2` en analysant la possibilité de répondre à `Q1` en connaissant les réponses de `Q2`.
    - [ ]  Minimisez une requête conjonctive `{p(x, y), p(y, z), r(z), p(x, z)}` en identifiant les atomes redondants. Justifiez comment la requête optimisée améliore l'efficacité d'évaluation.
11. **Exercices avancés : isomorphismes et cores multiples**
    
    - [ ]  Donnez un exemple d'un ensemble d'atomes binaires `A` tel que :
        - [ ]  `A` admet un isomorphisme dans lui-même (un « automorphisme ») qui n'est pas l'identité.
        - [ ]  `A` admet deux cores distincts, montrant ainsi des structures minimales équivalentes mais différentes.
12. **Construction d'un ensemble d'atomes avec homomorphismes universels**
    
    - [ ]  Soit un vocabulaire logique `V = (P, C)` où `P` et `C` sont des ensembles finis. Construisez un ensemble d'atomes `F` tel que tout ensemble d'atomes `F'` puisse admettre un homomorphisme vers `F`.
13. **Algorithme de calcul du core**
    
    - [ ]  Élaborez un algorithme pour calculer le core d'un ensemble d'atomes donné, en détaillant chaque étape pour identifier et supprimer les redondances.
    - [ ]  Évaluez la complexité de cet algorithme en termes du nombre de tests d'homomorphismes requis et justifiez pourquoi cet algorithme est efficace pour réduire les atomes à leur structure minimale.