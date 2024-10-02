---
tags:
  - TBD
  - cours
---
si trop complexe ;) [[Banalisation cours 1]]
### 1. Rappels de logique du premier ordre

#### **Syntaxe : Formules construites sur un vocabulaire**
- **Vocabulaire** : V = (P, C), où :
  - **P** : Ensemble fini de prédicats (ou relations), chacun ayant une **arité** (nombre d'arguments).
  - **C** : Ensemble de constantes, qui peut être infini.

Les formules sont construites sur un vocabulaire \(V\).

- **Terme** sur V : une **constante** c ∈ C ou une **variable**.
  - Les symboles de fonction d'arité supérieure à 0 ne sont pas pris en compte.
  
- **Atome** sur V : une expression de la forme **p(t1, ..., tk)**, où **p** appartient à P et chaque **ti** est un terme sur V.

- **Formule** sur V : définie par induction, elle peut être :
  - Un **atome** : p(t1, ..., tk).
  - Construite à partir de règles telles que :
    - **¬A** (négation),
    - **A ∧ B** (conjonction),
    - **A ∨ B** (disjonction),
    - **A → B** (implication),
    - **A ↔ B** (équivalence),
    - **∃x A** (quantification existentielle),
    - **∀x A** (quantification universelle),
    où **A** et **B** sont des formules sur V.

- **Variable libre** : une occurrence de variable qui n'est pas sous la portée d'un quantificateur.
- **Formule close (fermée)** : formule sans variable libre.

#### **Sémantique : Interprétations**

- Une **interprétation** d'un vocabulaire V = (P, C) est donnée par **I = (D_I, ._I)**, où :
  - **D_I ≠ ∅** : Le domaine de l'interprétation.
  - Pour toute constante **c** dans C, **c_I** appartient à **D_I**.
  - Pour tout prédicat **p** dans P d'arité **k**, **p_I** est un sous-ensemble de **D_I^k** (produit cartésien de D_I sur k termes).

Exemple :

V = ({p/2, r/3}, {a, b})

I = 
- D_I = {d1, d2, d3}
- a_I = d1, b_I = d2
- p_I = { (d2, d1), (d2, d3), (d3, d2) }
- r_I = { (d3, d3, d1) }

L'arité des prédicats est indiquée en indice.

- **Hypothèse simplificatrice** : on adopte l'hypothèse du **nom unique** (Unique Name Assumption), selon laquelle deux constantes différentes désignent forcément des objets différents. Cela signifie que dans toute interprétation, deux constantes différentes s'interprètent par deux éléments distincts du domaine.

Exemple (avec l'hypothèse du nom unique) :

V = ({p/2, r/3}, {a, b})

I =
- D_I = {d1, d2, d3}
- a_I = d1, b_I = d2
- p_I = { (b, a), (b, d3), (d3, b) }
- r_I = { (d3, d3, a) }

#### **Modèles**
- Une interprétation **I** est un **modèle** d'une formule close **f** si **f** est vraie dans **I**. On dit alors que **I satisfait f**.
- Une formule est dite **satisfiable** si elle possède un modèle. Sinon, elle est **insatisfiable**.

Exemples :
- f1 = ∃x ∃y (p(b, x) ∧ r(x, x, y))
  - Satisfaite par I.
- f2 = p(a, b) ∧ p(b, a)
  - Non satisfaite par I.
- f3 = ∀x ¬p(x, x)
  - Satisfaite par I.
- f4 = p(a, a) ∧ ∀x ¬p(x, x)
  - Insatisfiable.

#### **Conséquence logique (entailment)**
Étant donné deux formules closes **f** et **g**, on dit que **f ⊨ g** (f entraîne g, g est conséquence de f) si tout modèle de **f** est également un modèle de **g**. Autrement dit, dans tout monde où **f** est vraie, **g** doit l’être aussi.

Exemples :
- f1 : p(a) ∧ ∀x (p(x) → q(x))
- f2 : q(a)
- f3 : p(a) ∧ ∃x q(x)
- f4 : p(a) ∧ ¬q(a) ∧ ∀x (p(x) → q(x))

On a :
- f1 ⊨ f2, f3
- f4 ⊨ f1, f2, f3
- f4 est insatisfiable, donc toute formule est conséquence de f4.

---

### 2. Vision logique des bases de données et requêtes

#### **Bases de données / Bases de connaissances**
- **Ontologie** : ensemble fini de règles.
- **Atome instancié (ground)** : atome sans variables.
- **Fait** : atome instancié.
- **Base de faits** : ensemble fini de faits.

On peut établir une correspondance entre bases de données relationnelles et bases de faits :
- Toute base de données relationnelle peut être vue comme une base de faits.
- Toute base de faits peut être vue comme une base de données relationnelle.

#### **BD relationnelle : Ensemble de tables**
Chaque table obéit à un schéma. Par exemple :
| Titre          | Directeur     | Acteur       |
|----------------|---------------|--------------|
| Pulp Fiction   | Q. Tarantino  | J. Travolta  |
| Pulp Fiction   | Q. Tarantino  | U. Thurman   |
| Grease         | R. Kleiser    | J. Travolta  |

Un autre exemple avec des séances de cinéma :
| Cinéma   | Titre        | Horaire               |
|----------|--------------|-----------------------|
| Diagonal | Pulp Fiction | 01/03/2022 à 20h      |

#### **BD relationnelle : Vue abstraite**
- **Schéma de relation** : **R[A1...Ak]** où **R** est le nom de la relation d’arité **k**, et **A1...Ak** sont les attributs.
  - Exemple : Film[titre, directeur, acteur].

- **Schéma de base de données (S)** : ensemble de schémas de relations.
  - Exemple : 
    - Film[titre, directeur, acteur],
    - Programme[cinéma, titre, horaire],
    - Lieu[cinéma, adresse, site web].

- **Domaine (dom)** : ensemble des valeurs possibles dans les tables.
  
- **Table (ou instance de schéma de relation)** : ensemble fini de k-uplets sur **dom** pour un schéma de relation donné.
  
- **Base de données** sur (S, dom) : ensemble fini de tables sur dom, avec une table pour chaque schéma de relation.

#### **BD relationnelle : Vue logique**
On peut encore abstraire les bases de données en remplaçant les attributs par une numérotation : 1, 2, 3, etc. Ainsi, un schéma de relation devient un **prédicat** de même arité.

Exemple :
- Film[titre, directeur, acteur] devient Film/3.
- Programme[cinéma, titre, horaire] devient Programme/3.
- Lieu[cinéma, adresse, téléphone] devient Lieu/3.

Ainsi, un schéma de base de données **S** devient un ensemble de prédicats, et le domaine **dom** devient un ensemble de constantes.


#### **Requêtes dans le modèle relationnel**

L’**algèbre relationnelle** est un langage de requête formel basé sur un ensemble d'opérations telles que :
- **Sélection** : permet de choisir certaines lignes dans une table selon un critère.
- **Projection** : permet de choisir certaines colonnes dans une table.
- **Union** : combine les résultats de deux requêtes.
- **Différence** : soustrait les résultats d’une requête à une autre.
- **Produit cartésien** : combine toutes les lignes de deux tables.
- D'autres opérations dérivées existent, comme la **jointure**, qui combine des tables selon un critère commun.

Le langage **SQL** (Structured Query Language) repose sur ces principes de l’algèbre relationnelle. Une **requête SQL** associe une ou plusieurs tables à une nouvelle table, qui contient les résultats de la requête.

Exemple :  
Requêtes en SQL :
**"Trouver les films dans lesquels joue J. Travolta"**
```sql
SELECT DISTINCT Film.Titre  
FROM Film  
WHERE Film.Acteur = 'J. Travolta';
```

Ce qui donnerait le résultat suivant :
| Titre        |
|--------------|
| Pulp Fiction |
| Grease       |

#### **Requêtes en logique**

Toute requête exprimée en algèbre relationnelle peut être vue comme une **formule de la logique du premier ordre**, également appelée **requête de premier ordre**.

Traduction de la requête précédente en logique du premier ordre :
```text
Q(x) = ∃y Film(x, y, jt)
```
Dans cette formule :
- **x** et **y** sont des variables,
- **jt** est une constante (représentant J. Travolta).

Une **requête de premier ordre** est notée **Q(x1, ..., xk)**, où **x1, ..., xk** sont les **variables libres**, également appelées **variables réponses**. Ce sont les variables dont on cherche les valeurs. Si une requête n’a aucune variable réponse (c'est-à-dire si **k = 0**), on parle alors de **requête booléenne**.

Exemple de requête booléenne :  
**"J. Travolta joue-t-il dans un film ?"**
Cette question peut être formulée par la requête booléenne suivante :
```text
Q() = ∃x ∃y Film(x, y, jt)
```
Dans ce cas, il suffit de vérifier si cette requête est vraie ou fausse dans la base de données.

#### **Requêtes conjonctives**

Les requêtes qui consistent à **trouver un motif** (un ensemble de conditions qui doivent être vraies simultanément) sont appelées **requêtes conjonctives**. Une requête conjonctive prend la forme suivante :
```text
Q(x1, ..., xk) = ∃xk+1,...,xm (A1 ∧ A2 ∧ ... ∧ Ap)
```
Où **A1, ..., Ap** sont des **atomes** (c’est-à-dire des prédicats appliqués à des variables ou des constantes). La conjonction **(A1 ∧ A2 ∧ ... ∧ Ap)** signifie que toutes ces conditions doivent être vraies en même temps.

Autrement dit, une **requête conjonctive** est une **conjonction d'atomes** quantifiée existentiellement (ce qui signifie qu'il existe des valeurs possibles pour certaines variables, mais pas forcément toutes). Cependant, une requête conjonctive n'est pas nécessairement une formule **close** (fermée), c'est-à-dire qu'elle peut avoir des variables libres.

Exemple de traduction logique d'une requête SQL :  
**"Trouver les cinémas dans lesquels on passe un film de Tarantino et les titres de ces films"**
```sql
SELECT Programme.Cinéma, Programme.Titre  
FROM Film, Programme  
WHERE Film.Directeur = 'Q. Tarantino'  
AND Film.Titre = Programme.Titre;
```
Cette requête peut être traduite en une **requête conjonctive** dans la logique du premier ordre comme suit :
```text
Q(z, x) = ∃y ∃t (Film(x, qt, y) ∧ Programme(z, x, t))
```
Ici :
- **z** représente les cinémas,
- **x** représente les titres des films,
- **y** et **t** sont des variables quantifiées existentiellement, c'est-à-dire que leurs valeurs existent mais ne sont pas directement fournies.

#### **Union de requêtes conjonctives (UCQ)**

Une **union de requêtes conjonctives (UCQ)** est une **disjonction** (ou un "OU") de requêtes conjonctives ayant toutes les mêmes variables réponses. Autrement dit, une UCQ combine plusieurs requêtes conjonctives et retourne les résultats de toutes ces requêtes, à condition que les résultats partagent les mêmes variables réponses.

Exemple :  
**"Trouver les cinémas et les titres de films au programme de ces cinémas, tels que ce soient des films de Tarantino, des films avec Travolta, ou le film 'The Chef'"**
Cela peut être représenté sous forme d’une union de requêtes conjonctives comme suit :
```text
Q(z, x) = (∃y ∃t (Film(x, qt, y) ∧ Programme(z, x, t))) ∨  
           (∃y ∃t (Film(x, y, jt) ∧ Programme(z, x, t))) ∨  
           (∃t (Programme(z, x, t) ∧ x = 'The Chef'))
```
Cette requête est traduite en SQL de la manière suivante :
```sql
SELECT Programme.Cinéma, Programme.Titre  
FROM Programme, Film  
WHERE Film.Directeur = 'Q. Tarantino'  
AND Film.Titre = Programme.Titre  
UNION  
SELECT Programme.Cinéma, Programme.Titre  
FROM Programme, Film  
WHERE Film.Acteur = 'J. Travolta'  
AND Film.Titre = Programme.Titre  
UNION  
SELECT Programme.Cinéma, Programme.Titre  
FROM Programme  
WHERE Programme.Titre = 'The Chef';
```

#### **Sémantique des requêtes : Qu’est-ce qu’une réponse ?**

Commençons par les **requêtes booléennes**. L'idée est que si une requête booléenne est **vraie** dans une base de faits **F**, alors **F** fournit une réponse positive à la requête. En d'autres termes, une base de faits **F** "répond oui" à une requête booléenne **Q** si **Q** est **vraie** dans **F**.

Formellement, une **base de faits F** peut être vue comme une **interprétation logique** :
- Le **domaine** de l’interprétation est constitué des **constantes** de **F**.
- Chaque **constante** s’interprète par elle-même (c’est-à-dire que si une constante est présente dans la base de faits, elle représente exactement elle-même dans l’interprétation).
- Chaque **prédicat p** s’interprète comme l’ensemble des **uplets** (listes ordonnées d’éléments) qui satisfont ce prédicat dans **F**.

Exemple :  
Soit une base de faits **F** constituée de :
```text
p(a, b), p(b, a), p(a, c), q(b, b), q(a, c), q(c, b)
```
On peut voir **F** comme une interprétation **I** de la manière suivante :
- **D_I = {a, b, c}** (le domaine est constitué des constantes **a**, **b**, **c**).
- **p_I = {(a, b), (b, a), (a, c)}** (le prédicat **p** est vrai pour ces couples de constantes).
- **q_I = {(b, b), (a, c), (c, b)}** (le prédicat **q** est vrai pour ces couples).

On dit que **I** est le **modèle canonique** de **F** car c'est l’interprétation qui satisfait exactement les faits énoncés dans **F**.

#### **Réponses à une requête conjonctive**

Revenons aux **requêtes conjonctives**. Soit **Q(x1, ..., xk)** une requête conjonctive. Un **k-uplet** de constantes **(c1, ..., ck)** est une **réponse** à **Q** dans une base de faits **F** si **F** est un **modèle** de la requête **Q(x1/c1, ..., xk/ck)**, c’est-à-dire que si en remplaçant chaque variable **xi** par la constante **ci**, la requête **Q** est satisfaite par **F**.

Plus formellement, il existe un **homomorphisme** (une application qui préserve les relations) de **Q** dans **F** qui envoie chaque variable **xi** sur la constante **ci**.

Exemple d’une requête conjonctive :  
```text
Q() = ∃x ∃y ∃z (p(x, y) ∧ p(y, z)

 ∧ q(z, x))
```
Pour une base de faits **F** donnée par :
```text
p(a, b), p(b, a), p(a, c), q(b, b), q(a, c), q(c, b)
```
Cette requête peut être satisfaite si on peut trouver des **valeurs** pour **x**, **y** et **z** telles que **p(x, y)**, **p(y, z)** et **q(z, x)** soient vrais dans **F**. Deux façons d'y parvenir seraient :
1. **x = b**, **y = a**, **z = c**
2. **x = b**, **y = a**, **z = b**

Ces deux instanciations des variables de **Q** prouvent que **F** satisfait la requête **Q()**.

Ainsi, la base de faits **F** fournit deux réponses à cette requête conjonctive.
### 3. Différence entre les hypothèses de monde ouvert et monde clos

Dans les systèmes de bases de données et les systèmes de connaissances, il existe deux grandes hypothèses de modélisation du monde : **le monde clos** et **le monde ouvert**. Ces deux hypothèses influencent la manière dont les faits et les réponses aux requêtes sont interprétés dans une base de faits.

#### **Hypothèse du monde clos (Closed World Assumption - CWA)**

L'**hypothèse du monde clos** est couramment utilisée dans les bases de données relationnelles. Elle suppose que **tout ce qui n’est pas explicitement mentionné dans la base de faits est faux**. En d'autres termes, la base de faits décrit un monde **complètement connu**. Si un fait n'est pas présent dans la base, il est supposé ne pas exister.

Dans cette hypothèse, pour répondre à une requête booléenne **Q**, on ne considère que les faits présents dans **F** (la base de faits). Si **F** satisfait **Q**, la réponse est "oui". Sinon, la réponse est "non".

Exemple :
Supposons que nous ayons la base de faits suivante :
```text
p(a, b), p(b, a), p(a, c), q(b, b), q(a, c), q(c, b)
```
Si nous posons la question :  
**"Y a-t-il une relation **r** entre les éléments de cette base ?"**, et que **r** n'est pas présente dans la base, nous concluons que la réponse est **non**, car la base est supposée complète et ne contient aucune information sur **r**.

#### **Hypothèse du monde ouvert (Open World Assumption - OWA)**

L'**hypothèse du monde ouvert**, quant à elle, est couramment utilisée dans le **web sémantique** et les **bases de connaissances**. Elle suppose que la base de faits ne contient qu'une partie des informations disponibles, et que des faits peuvent exister même s'ils ne sont pas explicitement mentionnés dans la base. Ainsi, un fait qui n’est pas présent dans la base n’est ni vrai ni faux, il est simplement **inconnu**.

Dans cette hypothèse, pour répondre à une requête booléenne **Q**, on doit considérer **toutes les extensions possibles** de **F** (c’est-à-dire tous les ensembles de faits qui incluent **F**). Si **Q** est vraie dans **toutes** ces extensions, alors la réponse est "oui". Autrement dit, la base **F** satisfait **Q** (on note cela **F ⊨ Q**).

Exemple :
Reprenons la même base de faits :
```text
p(a, b), p(b, a), p(a, c), q(b, b), q(a, c), q(c, b)
```
Si nous posons la question :  
**"Y a-t-il une relation **r** entre certains éléments de cette base ?"**, alors sous l'hypothèse du monde ouvert, la réponse est "peut-être". Il se peut que des informations sur **r** existent mais ne soient pas encore connues ou ajoutées à la base.

#### **Différence entre monde ouvert et monde clos**

Sous l'**hypothèse du monde clos**, la réponse à une requête booléenne **Q** sur une base de faits **F** est **oui** si **F** satisfait **Q** (c'est-à-dire que **Q** est vraie dans **F**). Cela revient à faire une vérification de modèle : on examine directement si **Q** est vraie dans **F**.

Sous l'**hypothèse du monde ouvert**, la réponse à une requête booléenne **Q** sur **F** est **oui** si **toutes les extensions possibles** de **F** satisfont **Q**. Cela correspond à une **conséquence sémantique** : **F ⊨ Q**, c'est-à-dire que **F** implique nécessairement **Q**.

Pour les **requêtes conjonctives (CQ)** et les **unions de requêtes conjonctives (UCQ)**, ces deux hypothèses ne font **aucune différence**. Cela signifie que, dans ces cas, que l'on adopte l'hypothèse du monde ouvert ou clos, les réponses à une requête seront les mêmes.

Exemple concret :
Soit la base de faits suivante :
```text
F = { aPourEnfant(Jules, Chloé), Fille(Chloé) }
```
Et la question :  
**"Jules n’a-t-il que des filles ?"**  
Cette question se traduit par la requête booléenne suivante :
```text
Q() = ∀x (aPourEnfant(Jules, x) → Fille(x))
```
Cette formule peut également s’écrire :
```text
Q() = ¬ ∃x (aPourEnfant(Jules, x) ∧ ¬ Fille(x))
```
Dans ce cas, sous l'hypothèse du **monde clos**, **F** satisfait **Q** (puisque tous les enfants de Jules mentionnés dans la base sont des filles). Par contre, sous l'hypothèse du **monde ouvert**, **Q** n'est pas conséquence de **F**, car il pourrait exister des enfants de Jules qui ne sont pas mentionnés dans **F** et qui ne sont pas des filles.

Inversement, la question :  
**"Jules a-t-il un enfant qui n’est pas une fille ?"**  
Cette question se traduit par la requête booléenne suivante :
```text
Q() = ∃x (aPourEnfant(Jules, x) ∧ ¬ Fille(x))
```
Dans ce cas, sous les deux hypothèses (monde ouvert et monde clos), **F** ne satisfait pas **Q**, car il n’existe aucun fait dans **F** affirmant qu’un enfant de Jules n’est pas une fille.

---

### 4. Homomorphisme et conséquence logique

Les homomorphismes jouent un rôle fondamental dans la relation entre les **requêtes conjonctives** et les bases de faits. Un **homomorphisme** est une application qui associe les variables d'une requête conjonctive à des constantes d'une base de faits, tout en préservant les relations définies par les prédicats.

Soit **F** une base de faits et **q** une requête conjonctive booléenne. La base **F** **implique** la requête **q** (notée **F ⊨ q**) si et seulement si il existe un **homomorphisme** de **q** dans **F**.

#### Pourquoi est-ce vrai ?

- **(⇒)** Supposons que **F ⊨ q**, c'est-à-dire que **tout modèle de F** est également un **modèle de q**. En particulier, cela est vrai pour le **modèle canonique** de **F** (noté **M**). Par hypothèse, **M** est un modèle de **q**, ce qui signifie qu'il existe une application **f** qui associe les termes de **q** aux constantes du domaine de **M** (c'est-à-dire les constantes présentes dans **F**), et qui satisfait la requête **q**. Cette application **f** est exactement un homomorphisme de **q** dans **F**.
  
- **(⇐)** Supposons maintenant qu'il existe un **homomorphisme h** de **q** dans **F**. Cela signifie que **h** associe les variables de **q** à des constantes de **F** de manière à préserver les relations définies par les prédicats. En d'autres termes, pour chaque atome de **q**, les constantes associées par **h** satisfont les prédicats correspondants dans **F**. Comme le **modèle canonique** de **F** est l'**intersection de tous les modèles** de **F**, cela signifie que **tout modèle de F** est également un modèle de **q**. Ainsi, **F ⊨ q**.

---

### 5.Modèle canonique d’une base de faits

Le **modèle canonique** d'une base de faits **F** (sans variables) est un modèle particulier qui contient exactement les constantes et relations décrites dans **F**. Il s'agit du modèle le plus simple qui satisfait **F** et qui n'inclut aucune information supplémentaire.

Pour une base de faits **F** définie sur un vocabulaire **V = (P, C)** (où **P** est un ensemble de prédicats et **C** un ensemble de constantes), le modèle canonique de **F** a pour domaine **D_M = C**. Chaque prédicat **p** dans **P** est interprété par l'ensemble des uplets correspondant aux faits de **F** qui satisfont **p**.

Exemple :  
Soit la base de faits suivante :
```text
F = { p(a, b), p(b, c), q(c) }
```
Le **modèle canonique** de **F** est défini par :
- **D_M = {a, b, c, d, e}** (le domaine contient les constantes présentes dans **F** ainsi que d'autres éléments éventuels),
- **p_M = {(a, b), (b, c)}** (l'interprétation du prédicat **p** dans **M** correspond aux faits de **F**),


- **q_M = {c}** (l'interprétation de **q** correspond au fait **q(c)**),
- **r_M = ∅** (le prédicat **r**, qui n’apparaît pas dans **F**, est interprété comme vide).

---

### Conclusion : Récapitulatif des points clés

- **Toute base de données relationnelle** peut être vue comme un ensemble d’atomes instanciés (des faits), et **inversement**.
- L’**algèbre relationnelle** (sur laquelle repose SQL) et les **requêtes du premier ordre** ont **la même expressivité**.
- Les **requêtes conjonctives (CQ)** et les **unions de requêtes conjonctives (UCQ)** sont des classes fondamentales de requêtes dans les bases de données.
- Les réponses à une requête conjonctive **Q(x1, ..., xk)** sur une base de faits **F** sont données par des **homomorphismes** de **Q** dans **F**.
- Sous l'**hypothèse du monde clos**, une base de faits **F** satisfait une requête **Q** si **F** est un modèle de **Q**.
- Sous l'**hypothèse du monde ouvert**, une base de faits **F** satisfait une requête **Q** si **toutes les extensions** possibles de **F** satisfont **Q**. Pour les requêtes conjonctives, cependant, cela ne fait aucune différence entre monde ouvert et monde clos.

Ces concepts sont essentiels pour comprendre les bases de données relationnelles et les systèmes de gestion des connaissances, car ils définissent les fondements de l’interrogation et de la manipulation des données.


