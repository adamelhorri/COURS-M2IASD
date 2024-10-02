---
tags:
  - TBD
  - cours
---

# Cours 1 : Bases

### 1. Rappels de logique du premier ordre

#### **Syntaxe : Formules construites sur un vocabulaire**
- **Vocabulaire** : \(V = (P, C)\) où :
  - **P** : Liste de relations ou prédicats (ex. : "aime", "ami").
  - **C** : Liste de constantes (ex. : "Pierre", "Marie").

Les formules sont faites à partir de ce vocabulaire.

- **Terme** : Un **terme** peut être une constante (comme "Pierre") ou une variable (comme \(x\)).
  
- **Atome** : Un **atome** est une phrase qui dit quelque chose de vrai ou faux, comme \(p(t_1, ..., t_k)\), où \(p\) est un prédicat et \(t_i\) est un terme.

- **Formule** : Une formule peut être :
  - Un atome : \(p(t_1, ..., t_k)\).
  - Construite avec des règles, comme :
    - **¬A** (pas \(A\)),
    - **A ∧ B** (et),
    - **A ∨ B** (ou),
    - **A → B** (si \(A\), alors \(B\)),
    - **A ↔ B** (si et seulement si),
    - **∃x A** (il existe un \(x\) tel que \(A\)),
    - **∀x A** (pour tout \(x\), \(A\)).

- **Variable libre** : Une variable qui n'est pas sous un quantificateur.  
- **Formule close (fermée)** : Une formule qui n'a pas de variables libres.

#### **Sémantique : Interprétations**

- Une **interprétation** d'un vocabulaire \(V = (P, C)\) est notée **I = (D_I, ._I)**, où :
  - **D_I ≠ ∅** : Le domaine d'interprétation (ex. : {d1, d2}).
  - Chaque constante \(c\) dans \(C\) a une valeur dans \(D_I\).
  - Pour chaque prédicat \(p\) de \(P\), \(p_I\) est un ensemble de tuples dans \(D_I\).

**Exemple :**

Si \(V = ({p/2, r/3}, {a, b})\) :
- \(D_I = {d1, d2, d3}\)
- \(a_I = d1\), \(b_I = d2\)
- \(p_I = { (d2, d1), (d2, d3), (d3, d2) }\)
- \(r_I = { (d3, d3, d1) }\)

L'arité des prédicats indique le nombre d'arguments qu'ils prennent.

- **Hypothèse du nom unique** : Deux constantes différentes représentent des objets différents.

**Exemple :**

Avec \(V = ({p/2, r/3}, {a, b})\) :
- \(D_I = {d1, d2, d3}\)
- \(a_I = d1\), \(b_I = d2\)
- \(p_I = { (b, a), (b, d3), (d3, b) }\)
- \(r_I = { (d3, d3, a) }\)

#### **Modèles**
- Une interprétation **I** est un **modèle** d'une formule close **f** si **f** est vraie dans **I**. On dit que **I satisfait f**.
- Une formule est **satisfiable** si elle a au moins un modèle, sinon elle est **insatisfiable**.

**Exemples :**
- \(f1 = ∃x ∃y (p(b, x) ∧ r(x, x, y))\) est satisfaite par \(I\).
- \(f2 = p(a, b) ∧ p(b, a)\) n'est pas satisfaite par \(I\).

#### **Conséquence logique**
Si **f** entraîne **g** (noté \(f ⊨ g\)), cela signifie que si **f** est vraie, alors **g** est aussi vraie.

**Exemples :**
- Si \(f1 : p(a) ∧ ∀x (p(x) → q(x))\), alors \(f2 : q(a)\) est une conséquence.

---

### 2. Vision logique des bases de données et requêtes

#### **Bases de données / Bases de connaissances**
- **Ontologie** : Ensemble de règles.
- **Atome instancié** : Un atome sans variables, un fait.
- **Base de faits** : Ensemble d'atomes instanciés.

On peut comparer les bases de données relationnelles et les bases de faits :
- Une base de données relationnelle peut être considérée comme une base de faits et vice-versa.

#### **BD relationnelle : Ensemble de tables**
Chaque table suit un schéma. Par exemple :

| Titre          | Directeur     | Acteur       |
|----------------|---------------|--------------|
| Pulp Fiction   | Q. Tarantino  | J. Travolta  |
| Pulp Fiction   | Q. Tarantino  | U. Thurman   |
| Grease         | R. Kleiser    | J. Travolta  |

Un autre exemple :

| Cinéma   | Titre        | Horaire               |
|----------|--------------|-----------------------|
| Diagonal | Pulp Fiction | 01/03/2022 à 20h      |

#### **BD relationnelle : Vue abstraite**
- **Schéma de relation** : **R[A1...Ak]** où **R** est le nom et **A1...Ak** sont les attributs.
  - Exemple : Film[titre, directeur, acteur].

- **Schéma de base de données** : Ensemble de schémas de relations, ex. :
  - Film[titre, directeur, acteur],
  - Programme[cinéma, titre, horaire].

- **Domaine** : Ensemble des valeurs possibles dans les tables.

#### **Requêtes dans le modèle relationnel**

L’**algèbre relationnelle** est un ensemble d'opérations comme :
- **Sélection** : Choisir certaines lignes d'une table.
- **Projection** : Choisir certaines colonnes d'une table.
- **Union** : Combiner les résultats de deux requêtes.
- **Différence** : Soustraire les résultats d’une requête à une autre.

**Exemple de requête SQL :**  
**"Trouver les films avec J. Travolta"**
```sql
SELECT DISTINCT Film.Titre  
FROM Film  
WHERE Film.Acteur = 'J. Travolta';
```
**Résultat :**
| Titre        |
|--------------|
| Pulp Fiction |
| Grease       |

#### **Requêtes en logique**
Une requête en algèbre relationnelle peut aussi être vue comme une **formule de logique du premier ordre**.

**Exemple de traduction en logique :**
```text
Q(x) = ∃y Film(x, y, jt)
```

#### **Requêtes conjonctives**
Les requêtes qui cherchent à **trouver un motif** (ensemble de conditions) sont appelées **requêtes conjonctives**.

**Exemple de requête SQL :**  
**"Trouver les cinémas avec des films de Tarantino"**
```sql
SELECT Programme.Cinéma, Programme.Titre  
FROM Film, Programme  
WHERE Film.Directeur = 'Q. Tarantino'  
AND Film.Titre = Programme.Titre;
```

En logique, cela devient :
```text
Q(z, x) = ∃y ∃t (Film(x, qt, y) ∧ Programme(z, x, t))
```

#### **Union de requêtes conjonctives (UCQ)**
Une **UCQ** combine plusieurs requêtes conjonctives.

**Exemple :**  
**"Trouver les cinémas avec des films de Tarantino, des films avec Travolta ou 'The Chef'"**
```text
Q(z, x) = (∃y ∃t (Film(x, qt, y) ∧ Programme(z, x, t))) ∨  
           (∃y ∃t (Film(x, y, jt) ∧ Programme(z, x, t))) ∨  
           (∃t (Programme(z, x, t) ∧ x = 'The Chef'))
```

#### **Sémantique des requêtes : Qu’est-ce qu’une réponse ?**
Pour les **requêtes booléennes** :
- Une base de faits **F** répond "oui" à une requête booléenne **Q** si **Q** est vraie dans **F**.

**Exemple :**  
Base de faits **F** :
```text
p(a, b), p(b, a), p(a, c), q(b, b), q(a, c), q(c, b)
```
On peut voir **F** comme une interprétation.

#### **Réponses à une requête conjonctive**
Un **k-uplet** de constantes est une **réponse** à **Q** dans **F** si **F** satisfait **Q**

.

**Exemple :**
```text
Q() = ∃x ∃y ∃z (p(x, y) ∧ p(y, z) ∧ q(z, x))
```
Pour la base **F** :
```text
p(a, b), p(b, a), p(a, c), q(b, b), q(a, c), q(c, b)
```
Si on trouve des valeurs pour \(x\), \(y\), et \(z\) qui satisfont **Q**, alors on a une réponse.

---

### 3. Différence entre les hypothèses de monde ouvert et monde clos

#### **Hypothèse du monde clos (CWA)**
Dans les bases de données, l'hypothèse du monde clos suppose que **tout ce qui n’est pas mentionné est faux**. Si un fait n'est pas présent, il est supposé ne pas exister.

**Exemple :**
Si on demande :  
**"Y a-t-il une relation **r** ?"**, et **r** n'est pas dans la base, la réponse est **non**.

#### **Hypothèse du monde ouvert (OWA)**
Dans cette hypothèse, il est possible que des faits existent même s'ils ne sont pas mentionnés. Donc, si un fait n'est pas présent, on ne peut pas dire s'il est vrai ou faux.

**Exemple :**
Reprenons la même base :  
Si on demande :  
**"Y a-t-il une relation **r** ?"**, la réponse est **peut-être**.

#### **Différence entre monde ouvert et monde clos**
- **Monde clos** : On vérifie directement si **Q** est vraie dans **F**.
- **Monde ouvert** : On doit considérer toutes les extensions possibles de **F** pour répondre à **Q**.

Pour les **requêtes conjonctives**, ces deux hypothèses n'affectent pas les réponses.

---

### 4. Homomorphisme et conséquence logique

Les **homomorphismes** sont essentiels pour relier les requêtes conjonctives et les bases de faits.

Une base **F** implique une requête **q** (notée **F ⊨ q**) si un **homomorphisme** existe de **q** dans **F**.

#### Pourquoi est-ce vrai ?
- Si **F ⊨ q**, alors tous les modèles de **F** sont aussi des modèles de **q**.
- Si un homomorphisme **h** de **q** dans **F** existe, alors **F** satisfait **q**.

---

### 5. Modèle canonique d’une base de faits

Le **modèle canonique** d'une base de faits **F** est un modèle qui contient exactement les relations décrites dans **F**.

Pour une base de faits **F** sur un vocabulaire **V = (P, C)** :
- Le modèle canonique a pour domaine **D_M = C**.
- Chaque prédicat est interprété par l'ensemble des tuples correspondant aux faits de **F**.

**Exemple :**  
Si **F** est :
```text
F = { p(a, b), p(b, c), q(c) }
```
Alors le modèle canonique de **F** est défini par :
- **D_M = {a, b, c}**,
- **p_M = {(a, b), (b, c)}**,
- **q_M = {c}**.

---

### Conclusion : Récapitulatif des points clés

- **Base de données** = ensemble d’atomes instanciés (faits).
- **Algèbre relationnelle** = base de SQL, même logique que les requêtes du premier ordre.
- **Requêtes conjonctives (CQ)** et **unions de requêtes conjonctives (UCQ)** sont essentielles.
- Les réponses à une requête conjonctive **Q(x1, ..., xk)** sur **F** proviennent des **homomorphismes** de **Q** dans **F**.
- Dans le **monde clos**, **F** satisfait **Q** si **F** est un modèle de **Q**.
- Dans le **monde ouvert**, **F** satisfait **Q** si toutes les extensions possibles de **F** satisfont **Q**. 

Ces idées sont cruciales pour comprendre comment fonctionnent les bases de données et la gestion des connaissances.
