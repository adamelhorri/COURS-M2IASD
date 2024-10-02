---
tags:
  - TBD
  - cours
---
pour une version plus simple ;) [[Banalisation cours 0 FOL]]
# Logique du Premier Ordre (FOL)

La logique du premier ordre, souvent abrégée en FOL (First Order Logic), est un système formel permettant de raisonner sur des objets, des relations entre ces objets, et des propriétés de ces objets. Contrairement à la logique propositionnelle qui ne traite que des propositions vraies ou fausses, la logique du premier ordre introduit des concepts tels que les **variables**, les **prédicats**, et les **quantificateurs**. Nous allons aborder de manière détaillée la **syntaxe**, la **sémantique**, et un fragment spécifique de cette logique : le fragment FOL(∧,∃).

---

## 1. Syntaxe

La syntaxe de la logique du premier ordre repose sur la définition d'un **vocabulaire logique** V = (P, C), où :

- **P** représente un ensemble de **prédicats** ou **relations**, chaque prédicat ayant une arité k, c’est-à-dire le nombre d’arguments qu'il peut prendre. L’arité k est un entier supérieur ou égal à 0. Par exemple, un prédicat unitaire prend un seul argument, tandis qu'un prédicat binaire en prend deux.
- **C** représente un ensemble de **constantes**, des entités spécifiques comme des objets ou des éléments précis du domaine. Par exemple, une constante pourrait représenter un entier, un individu, ou un objet défini dans le monde de l’interprétation.

Il est important de noter que P est un ensemble fini, mais que C peut être infini. Par exemple, si le domaine d’interprétation inclut les entiers naturels, alors C peut inclure tous les entiers naturels, ce qui en fait un ensemble infini.

### Termes

Dans la logique du premier ordre, un **terme** est défini comme :

- Une **variable**, qui représente une entité inconnue.
- Une **constante** c ∈ C, qui représente une entité précise ou un élément particulier du domaine d’interprétation.

Il est également possible d’utiliser des **symboles fonctionnels** qui permettent de "calculer" ou de générer des individus à partir d’autres individus. Par exemple, une fonction pourrait prendre un objet et retourner son parent. Cependant, dans cette présentation, nous ne considérerons pas ces symboles fonctionnels pour simplifier les explications.

### Atomes

Un **atome** est la plus petite formule bien formée en logique du premier ordre. Il est constitué d’un prédicat p ∈ P et d’une liste de k termes, où k est l’arité du prédicat p. Un atome prend donc la forme suivante :

**p(e1, e2, ..., ek)**

où p est un prédicat de P et les ei sont des termes définis sur V (ceux-ci pouvant être des variables ou des constantes).

### Formules

Les **formules** en logique du premier ordre sont construites à partir des atomes et des connecteurs logiques. La définition des formules suit une structure inductive :

1. Un **atome** est une formule.
2. Si A et B sont des formules et x est une variable, alors les expressions suivantes sont aussi des formules :
   - ¬A (la négation de A)
   - A ∧ B (la conjonction de A et B, c’est-à-dire A et B sont toutes deux vraies)
   - A ∨ B (la disjonction de A et B, c’est-à-dire A ou B est vrai)
   - A → B (l’implication de A vers B)
   - A ↔ B (l’équivalence entre A et B, c’est-à-dire que A est vrai si et seulement si B est vrai)
   - ∀x A (le quantificateur universel : "pour tout x, A est vrai")
   - ∃x A (le quantificateur existentiel : "il existe un x tel que A est vrai")

### Portée des quantificateurs

Il est crucial de comprendre la notion de **portée** des quantificateurs en logique du premier ordre. Un **quantificateur** affecte la sous-formule qui le suit immédiatement. Prenons les exemples suivants pour illustrer cela.

- **Exemple 1** :
  Considérons la formule **f1 = ∀x (p(x) → r(x, a))**, où a est une constante. La formule est composée de deux atomes p(x) et r(x, a), reliés par une implication →. Le quantificateur universel ∀x s'applique à toute la formule p(x) → r(x, a), ce qui signifie que pour tout x, si p(x) est vrai, alors r(x, a) doit être vrai.

- **Exemple 2** :
  Une autre formule possible est **f2 = (∀x p(x)) → r(x, a)**, où le placement des parenthèses modifie la portée du quantificateur. Dans ce cas, le quantificateur universel ∀x s'applique uniquement à l'atome p(x), et non à l’implication entière. Le quantificateur ne porte que sur p(x), ce qui signifie que p(x) est vrai pour tout x, et cette vérité de p(x) doit impliquer que r(x, a) est vrai.

### Simplification des parenthèses

En pratique, pour éviter des formules trop complexes ou trop chargées en parenthèses, il est courant de simplifier les formules en supprimant les parenthèses qui ne sont pas nécessaires. Par exemple :

- f2 peut être réécrit sans parenthèses comme suit : **f2 = ∀x p(x) → r(x, a)**.

Cette simplification repose sur des propriétés associatives des connecteurs ∧ et ∨. Par exemple, une formule du type **(A(x) ∧ (B(x) ∧ C(x)))** peut être simplifiée en **A(x) ∧ B(x) ∧ C(x)**, ce qui clarifie la lecture sans altérer le sens.

### Notation pointée

Une autre notation couramment utilisée en logique, notamment dans le cadre de la preuve de programmes, est la **notation pointée**. Cette notation diffère dans sa manière de gérer les parenthèses et la portée des quantificateurs. Par exemple, une formule comme **∀x. p(x) → r(x, a)** signifie que ∀x porte sur toute la formule qui suit, à savoir p(x) → r(x, a). Cela permet de réduire le nombre de parenthèses.

Si nous souhaitons exprimer f2 avec la notation pointée, nous utiliserions les parenthèses pour limiter la portée du quantificateur, ce qui donnerait **(∀x. p(x)) → r(x, a)**.

### Variables libres et liées

Les **variables** dans une formule peuvent être classées comme **libres** ou **liées**. Une **variable libre** est une variable qui apparaît dans la formule sans être sous la portée d'un quantificateur. À l’inverse, une **variable liée** est une variable qui est capturée par un quantificateur (comme ∀x ou ∃x).

Une formule est dite **fermée** si elle ne contient aucune variable libre. Autrement dit, toutes les variables présentes dans la formule sont liées par des quantificateurs. Par exemple :

- La formule **f1 = ∀x (p(x) → r(x, a))** est **fermée** si a est une constante, car la seule variable x est liée par le quantificateur ∀x.
- En revanche, la formule **f2 = ∀x p(x) → r(x, a)** n'est pas fermée, car la variable x dans r(x, a) est libre.

---

## 2. Sémantique

En logique du premier ordre, la **sémantique** d'une formule est définie par son interprétation dans un "monde possible". Une **interprétation** d’un vocabulaire V = (P, C) est composée de :

1. Un **domaine non vide** D, représentant l'ensemble des objets ou entités dans ce monde.
2. Une fonction d'interprétation qui attribue une signification à chaque constante et prédicat de V.

### Interprétation des constantes et des prédicats

Une interprétation des prédicats repose sur l'idée que chaque prédicat correspond à un ensemble d'objets ou de combinaisons d'objets du domaine D. Prenons un exemple simple : si P contient un prédicat binaire r(x, y), et que l'interprétation I(r) est donnée par l'ensemble des couples (a, b), cela signifie que la relation r(x, y) est vraie pour les paires d'éléments (a, b) et (b, c) dans le domaine D.

Il est important de noter que certaines interprétations reposent sur l'**hypothèse du nom unique** (Unique Name Assumption, ou UNA). Cette hypothèse stipule que chaque constante c1 et c2 représente des individus différents dans le domaine. Autrement dit, si c1 ≠ c2, alors I(c1) ≠ I(c2). Par conséquent, les constantes distinctes doivent être associées à des éléments distincts du domaine.

Cependant, il est parfois utile de simplifier encore plus les interprétations en supposant que toutes les constantes s'interprètent elles-mêmes, c'est-à-dire I(c) = c. Dans ce cas, chaque constante

 est directement assimilée à un objet particulier dans le domaine, ce qui permet d'alléger les calculs et d'éviter des ambiguïtés dans la représentation.

### Vérité et modèles

L’une des questions centrales en logique du premier ordre est de savoir si une formule est **vraie** ou **fausse** dans une interprétation donnée. Cela se fait en attribuant une valeur de vérité aux formules dans une interprétation.

- Une **formule fermée** (c’est-à-dire sans variable libre) est dite **vraie** dans une interprétation I si elle est satisfaite par tous les éléments du domaine.
- Si une formule contient des variables libres, il est nécessaire de fournir une **assignation** de ces variables à des éléments spécifiques du domaine pour évaluer la vérité de la formule.

Prenons deux exemples pour illustrer cela :

1. Une formule fermée **∀x A(x)** est vraie dans une interprétation I si, pour tout élément d ∈ D, la formule A(x) est vraie lorsque x est assigné à d. Si cela est vérifié pour tous les d ∈ D, alors la formule entière **∀x A(x)** est vraie dans I.
2. Une formule existentielle **∃x A(x)** est vraie s'il existe un élément d ∈ D tel que A(x) est vraie lorsque x est assigné à d.

### Connecteurs logiques et vérité

Les connecteurs logiques ∧, ∨, ¬, etc., se comportent de manière similaire à ceux utilisés en logique propositionnelle. Voici un résumé de la manière dont ces connecteurs fonctionnent en termes de vérité :

- ¬A est vrai si A est faux.
- A ∧ B est vrai si A et B sont tous deux vrais.
- A ∨ B est vrai si au moins l’un des deux, A ou B, est vrai.
- A → B est vrai si A est faux ou si B est vrai (implication logique).
- A ↔ B est vrai si A et B ont la même valeur de vérité, c’est-à-dire qu’ils sont soit tous deux vrais, soit tous deux faux.

Un **atome** p(e1, e2, ..., ek) est vrai dans une interprétation I si les k-uplets (d1, d2, ..., dk), obtenus en assignant les termes ei aux éléments du domaine D, appartiennent à l’ensemble I(p). En d’autres termes, p(e1, ..., ek) est vrai si la combinaison des éléments correspondants dans le domaine satisfait la relation p.

Lorsqu'une formule est vraie dans une interprétation I, on dit que I est un **modèle** de la formule. Si une formule f est vraie dans toutes les interprétations possibles, elle est dite **valide**.

### Conséquence logique et équivalence

Deux formules f et g sont dites **équivalentes** si elles ont la même valeur de vérité dans toutes les interprétations possibles. Cela est noté **f ≡ g**.

Si une formule f est une **conséquence logique** d’une formule g, cela signifie que dans toute interprétation où g est vraie, f est également vraie. Cela se note **g |= f**, c’est-à-dire que chaque modèle de g est également un modèle de f.

Voici quelques propriétés importantes liées à la conséquence logique :

- Une formule A est **insatisfiable** si elle n'a pas de modèle, c’est-à-dire qu'il n'existe aucune interprétation où A est vraie.
- A |= B ssi la formule (A → B) est valide, ssi la formule (A ∧ ¬B) est insatisfaisable.
- Deux formules A et B sont **équivalentes** ssi la formule (A ↔ B) est valide.

---

## 3. Fragment FOL(∧,∃)

Un **fragment logique** est une restriction de la logique complète à un sous-ensemble spécifique de formules ou de connecteurs. Dans le cas de la logique du premier ordre, nous allons nous concentrer sur un fragment particulier appelé **FOL(∧, ∃)**, qui signifie **logique du premier ordre conjonctive et existentielle**.

Dans ce fragment :

- Les formules sont **conjonctives**, c’est-à-dire qu’elles n’utilisent que le connecteur ∧ pour combiner les atomes.
- Les formules sont **existentielles**, c’est-à-dire qu’elles n’utilisent que le quantificateur existentiel ∃ pour introduire des variables.

Le fragment **FOL(∧, ∃)** exclut les symboles fonctionnels, simplifiant ainsi la représentation des relations entre les objets.

### Structure des formules dans FOL(∧, ∃)

Une formule dans ce fragment a la forme générale :

**∃x1, x2, ..., xn (A1 ∧ A2 ∧ ... ∧ Ap)**

où les Aj sont des atomes (des prédicats appliqués à des termes) et les xi sont des variables qui apparaissent dans ces atomes.

Cela signifie que les formules dans **FOL(∧, ∃)** représentent des **faits existentiels** concernant un ensemble d’objets dans un domaine donné. Par exemple :

- **∃x (p(x) ∧ q(x))** signifie qu’il existe un élément x dans le domaine tel que les deux prédicats p(x) et q(x) sont vrais pour cet élément.

### Notation ensembliste

Pour les formules fermées dans ce fragment, il est possible d’utiliser une **notation ensembliste** pour simplifier l’expression des formules. Par exemple, la formule **∃x (p(x) ∧ q(x))** peut être représentée par l’ensemble **{p(x), q(x)}**, où chaque élément de l’ensemble représente un atome.

Il convient de noter qu’il peut exister des **formules équivalentes** qui, bien qu’exprimées différemment, ont la même signification logique. Par exemple, les formules **∃x (p(x) ∧ p(x))** et **∃x p(x)** sont équivalentes car elles expriment toutes deux le même fait : il existe un x tel que p(x) est vrai.

### Homomorphismes dans FOL(∧, ∃)

Le concept central pour raisonner dans ce fragment est celui de l’**homomorphisme**. Un **homomorphisme** entre deux ensembles d’atomes f et g est une application qui associe les variables de f aux termes de g de telle manière que chaque atome de f est satisfait dans g. Autrement dit, il existe une correspondance entre les objets et les relations de f et ceux de g.

L’importance des homomorphismes réside dans le fait qu’ils permettent de **démontrer** qu’une formule f est une conséquence logique d’une formule g. En effet, pour prouver que g |= f (c’est-à-dire que f est vraie chaque fois que g est vraie), il suffit de montrer qu’il existe un homomorphisme de f dans g.

---

Cette présentation en Markdown offre une structure claire et accessible pour le cours sur la logique du premier ordre, tout en conservant tous les détails essentiels.