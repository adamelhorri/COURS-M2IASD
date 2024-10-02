---
tags:
  - TBD
  - cours
---
Voici une version développée et détaillée du cours sur la logique du premier ordre, adaptée pour un adolescent de 17 ans, avec des explications claires et des exemples pour illustrer les concepts.

---

# Introduction à la Logique du Premier Ordre (FOL)

La logique du premier ordre est un système qui nous aide à raisonner sur des objets, leurs relations et leurs propriétés. C'est un outil puissant utilisé en mathématiques, en informatique et même en philosophie. Dans cette introduction, nous allons explorer les bases de ce système, y compris sa syntaxe, sa sémantique et un sous-ensemble particulier connu sous le nom de FOL(∧, ∃).

---

## 1. Syntaxe

### Qu'est-ce qu'un vocabulaire logique ?

Un **vocabulaire logique** est comme un ensemble de mots et de règles que nous utilisons pour créer des phrases logiques. Dans ce vocabulaire, nous avons :

- **Prédicats** : Ce sont des mots qui décrivent des relations ou des propriétés. Par exemple, "est un élève" pourrait être un prédicat utilisé pour parler d'une personne.
- **Constantes** : Ce sont des mots qui désignent des choses précises, comme "Marie", "Paul", ou "30". Elles représentent des entités spécifiques dans notre monde.

### Termes

Un **terme** est une unité fondamentale en logique qui peut être :

- Une **variable** : Un symbole qui représente quelque chose d'inconnu ou qui peut changer. Par exemple, on peut utiliser "x" pour représenter n'importe quel élève dans une classe.
- Une **constante** : Un symbole qui représente quelque chose de précis. Par exemple, "Marie" est une constante qui désigne une personne spécifique.

### Atomes

Un **atome** est la plus petite unité que l'on peut créer en logique. Il se compose d'un prédicat et de termes. Par exemple :

- **est un élève(Marie)** est un atome qui dit que Marie est un élève.

C'est une affirmation simple mais qui peut être utilisée comme base pour construire des phrases plus complexes.

### Formules

Les **formules** sont des phrases logiques qui peuvent être vraies ou fausses. Voici comment nous les construisons :

1. Un **atome** est une formule.
2. Si A et B sont des formules et x est une variable, nous pouvons faire les formules suivantes :
   - **¬A** : "non A", ce qui signifie que A est faux. Par exemple, si A est "Marie est un élève", alors ¬A signifie "Marie n'est pas un élève".
   - **A ∧ B** : "A et B", ce qui signifie que les deux sont vrais. Par exemple, "Marie est un élève et elle a 17 ans".
   - **A ∨ B** : "A ou B", ce qui signifie que l'un ou l'autre est vrai. Par exemple, "Marie est un élève ou elle est professeur".
   - **A → B** : "Si A alors B", cela indique que si A est vrai, alors B doit aussi être vrai. Par exemple, "Si Marie est un élève, alors elle va à l'école".
   - **A ↔ B** : "A si et seulement si B", cela signifie que A et B doivent avoir la même valeur de vérité. Par exemple, "Marie est un élève si et seulement si elle va à l'école".
   - **∀x A** : "Pour tout x, A est vrai", ce qui signifie que ce qui est dit pour x est vrai pour tous les x. Par exemple, "Pour tout élève, il va à l'école".
   - **∃x A** : "Il existe un x tel que A est vrai", cela signifie qu'il y a au moins un x pour lequel A est vrai. Par exemple, "Il existe un élève qui aime les maths".

### Portée des quantificateurs

Les **quantificateurs** (comme ∀ et ∃) sont des mots qui nous permettent de parler de tous les objets ou d'au moins un objet dans notre domaine. Voici quelques exemples pour clarifier :

- **Exemple 1** : Considérons la formule **f1 = ∀x (p(x) → r(x, a))**, où a est une constante. Cette formule signifie que pour chaque x, si p(x) est vrai, alors r(x, a) doit aussi être vrai. En d'autres termes, pour chaque élève, s'il est un élève, alors il va à l'école.

- **Exemple 2** : Une autre formule pourrait être **f2 = (∀x p(x)) → r(x, a)**. Ici, le quantificateur ∀x ne s'applique qu'à p(x). Cela signifie que si tous les élèves sont des élèves, alors r(x, a) est vrai. Il est important de noter que la position des parenthèses change le sens de la phrase.

### Simplification des parenthèses

Pour simplifier les formules et éviter des phrases trop chargées, il est courant de supprimer les parenthèses non nécessaires. Par exemple :

- f2 peut être réécrit sans parenthèses comme **f2 = ∀x p(x) → r(x, a)**. Cela aide à rendre la formule plus lisible.

### Notation pointée

Une autre notation couramment utilisée en logique, notamment dans le cadre de la preuve de programmes, est la **notation pointée**. Cette notation diffère dans sa manière de gérer les parenthèses et la portée des quantificateurs. Par exemple, une formule comme **∀x. p(x) → r(x, a)** signifie que ∀x porte sur toute la formule qui suit, à savoir p(x) → r(x, a). Cela permet de réduire le nombre de parenthèses.

Si nous souhaitons exprimer f2 avec la notation pointée, nous utiliserions les parenthèses pour limiter la portée du quantificateur, ce qui donnerait **(∀x. p(x)) → r(x, a)**.

### Variables libres et liées

Les **variables** peuvent être classées comme **libres** ou **liées**. Voici ce que cela signifie :

- Une **variable libre** est une variable qui apparaît dans la formule sans être sous le contrôle d'un quantificateur. Par exemple, dans la formule **r(x, a)**, x est libre si aucun quantificateur ne le contrôle.
- Une **variable liée** est une variable qui est sous le contrôle d'un quantificateur. Par exemple, dans la formule **∀x p(x)**, x est liée parce qu'elle est sous le contrôle du quantificateur ∀.

Une formule est **fermée** si toutes les variables sont liées. Cela signifie qu'il n'y a pas de variables libres qui peuvent prendre des valeurs dans le domaine.

---

## 2. Sémantique

La **sémantique** est la manière dont nous interprétons nos formules logiques dans le monde réel. Voici comment cela fonctionne :

### Interprétation

Une **interprétation** d'un vocabulaire logique comprend :

1. Un **domaine** : L'ensemble des objets que nous considérons. Par exemple, tous les élèves d'une classe ou tous les animaux dans un zoo.
2. Une fonction qui attribue une signification aux constantes et aux prédicats. Par exemple, la constante "Marie" pourrait être associée à une personne spécifique dans notre domaine.

### Vérité

Une formule peut être **vraie** ou **fausse** dans une interprétation. Voici comment déterminer cela :

- Une **formule fermée** (c’est-à-dire sans variable libre) est dite **vraie** dans une interprétation I si elle est satisfaite par tous les éléments du domaine. Par exemple, si nous savons que Marie est effectivement un élève, alors "est un élève(Marie)" est vrai.

- Si une formule contient des variables libres, il est nécessaire de fournir une **assignation** de ces variables à des éléments spécifiques du domaine pour évaluer la vérité de la formule. Par exemple, pour vérifier si "x est un élève" est vrai, nous devons dire quel élève x représente.

### Modèles

Quand une formule est vraie dans une interprétation, on dit que cette interprétation est un **modèle** de la formule. Si une formule est vraie dans toutes les interprétations possibles, on dit qu'elle est **valide**. Par exemple, la formule "Tout élève est une personne" est valide car cela est vrai dans n'importe quel contexte.

### Conséquence logique

Si une formule A est toujours vraie quand une autre formule B est vraie, alors nous disons que A est une **conséquence logique** de B. Par exemple, si "Marie est un élève" implique "Marie va à l'école", alors nous avons une conséquence logique. Cela signifie que chaque fois que B est vrai, A doit aussi être vrai.

---

## 3. Fragment FOL(∧, ∃)

Nous allons maintenant examiner un sous-ensemble spécifique de la logique du premier ordre, appelé **FOL(∧, ∃)**. Ce fragment se concentre sur :

- **Conjonctions** (A ∧ B) : Nous n'utilisons que le "et" pour combiner des phrases.
- **Quantificateurs existentiels** (∃x) : Nous parlons d'au moins un objet.

### Formules dans FOL(∧, ∃)

Les formules de ce fragment ont la forme :

**∃x1, x2, ..., xn (A1 ∧ A2 ∧ ... ∧

 Ap)**

Cela signifie qu'il existe au moins un objet qui satisfait toutes les conditions des atomes A1, A2, etc. Par exemple :

- **Il existe un élève qui aime les maths et qui va à l'école** pourrait être une formule dans ce fragment.

### Notation ensembliste

Dans ce fragment, nous pouvons aussi utiliser une **notation ensembliste** pour simplifier l'expression des formules. Par exemple, la formule **∃x (p(x) ∧ q(x))** peut être représentée par l’ensemble **{p(x), q(x)}**, où chaque élément de l’ensemble représente un atome. Cela facilite la compréhension des relations entre les objets.

### Homomorphismes

Un **homomorphisme** est une façon de relier des formules entre elles. Si vous pouvez trouver un homomorphisme d'une formule à une autre, cela signifie que la première formule est une conséquence de la seconde. Cela nous aide à établir des relations logiques entre différentes formules.

---

Cette présentation vise à rendre le contenu plus accessible et compréhensible, tout en conservant les concepts essentiels de la logique du premier ordre. Des exemples et des explications claires sont fournis pour aider à comprendre comment fonctionne ce système logique.