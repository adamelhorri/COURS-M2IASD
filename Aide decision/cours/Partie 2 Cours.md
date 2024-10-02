---
tags:
  - aideDecision
  - cours
---

### Aide à la décision - Souhila KACI

### Gestion des incohérences

---

Dans cette première partie du deuxième cours sur l'aide à la décision, nous allons explorer la manière dont les incohérences dans une base de connaissances peuvent être gérées. Contrairement aux systèmes classiques qui supposent que les bases de connaissances sont cohérentes, ce cours présente des méthodes pour **gérer les incohérences** et **effectuer des inférences** même lorsque des contradictions sont présentes dans les informations disponibles.

---

#### **Qu'est-ce que la connaissance ?**

La **connaissance** est définie comme les informations que nous avons sur les croyances ou les préférences des agents. Cela inclut des informations sur des faits, des règles, ou des relations entre objets ou événements qui sont utilisées pour prendre des décisions ou faire des inférences logiques.

##### **Exemple de connaissance** :
- « Les pingouins sont des oiseaux. »
- « Les oiseaux volent. »
- « Un pingouin ne vole pas. »

Ce type de connaissance peut provenir de plusieurs sources, et il est essentiel de pouvoir raisonner à partir de ces informations, même lorsqu'elles sont contradictoires.

---

#### **Contexte de la logique classique**

La **logique propositionnelle classique** est un cadre utilisé pour raisonner sur des propositions logiques. Elle repose sur un ensemble de **variables propositionnelles** (comme **a, b, c, etc.**) et des **connecteurs logiques** usuels (**∧** pour "et", **∨** pour "ou", **¬** pour la négation, **→** pour l'implication, et **↔** pour l'équivalence).

Dans cette logique, les **formules logiques** sont des combinaisons de ces variables et connecteurs, et permettent de représenter des informations telles que :
- **a ∧ b** : « a et b sont tous deux vrais ».
- **a → b** : « si a est vrai, alors b est vrai ».

L'objectif de la logique classique est de **déduire** de nouvelles informations à partir d'un ensemble donné de formules, appelé **base de connaissances**.

---

#### **Définitions complémentaires**

Dans la logique classique, une **base de connaissances Σ** est un ensemble de formules. Cependant, il est possible que cette base contienne des **incohérences**. Quelques concepts essentiels liés à l'incohérence sont :

1. **Consistance maximale** : Un sous-ensemble **A** de **Σ** est dit **maximalement consistant** s'il est cohérent et qu'ajouter une nouvelle formule de **Σ \ A** entraînerait une incohérence.
   
2. **Inconsistance minimale** : Un sous-ensemble **A** de **Σ** est **minimalement incohérent** s'il est incohérent et que retirer une formule de **A** restaurerait la cohérence.

Ces concepts sont cruciaux pour la gestion des incohérences dans une base de connaissances, car ils permettent de **localiser les conflits** et de raisonner à partir d'informations partielles qui sont encore cohérentes.

---

#### **Sources d'incohérence**

Les incohérences dans une base de connaissances peuvent provenir de plusieurs facteurs. Les **exceptions** et la **multiplicité des sources** d'information sont deux causes fréquentes.

##### **Exemple d'incohérence due aux exceptions** :
- « Les oiseaux volent. »
- « Les pingouins sont des oiseaux. »
- « Les pingouins ne volent pas. »

Ici, une contradiction apparaît car le fait qu'un pingouin ne vole pas contredit la règle générale selon laquelle les oiseaux volent.

##### **Exemple d'incohérence due à des sources multiples** :
- Agent 1 : « Nous avons cours demain. »
- Agent 2 : « S'il pleut, nous n'avons pas cours. »
- Agent 3 : « Il pleuvra demain. »

Ces informations provenant de différentes sources introduisent une incohérence lorsqu'elles sont combinées. Pour résoudre ce type de conflit, il est nécessaire de réviser ou d'ajuster la base de connaissances.

---

#### **Méthodes de gestion des incohérences**

Lorsqu'une incohérence est détectée dans une base de connaissances, deux grandes approches sont envisageables :

1. **Révision de la base de connaissances** : Il s'agit de modifier la base pour éliminer les contradictions. Cela peut impliquer de retirer certaines informations ou de reformuler des règles pour rétablir la cohérence.

2. **Tolérance aux incohérences** : Au lieu d'éliminer les incohérences, cette approche consiste à définir des méthodes de raisonnement non classique qui permettent de **faire des inférences même en présence d'incohérences**. Ces méthodes sont essentielles pour exploiter une base de connaissances sans perdre d'informations utiles.

---

#### **Inférence non classique et gestion des incohérences : Le cas plat**

Dans le cas plat (où toutes les informations ont le même statut), une méthode efficace pour gérer les incohérences est l'utilisation de la **relation argumentative de conséquence**.

---

##### **Relation argumentative de conséquence**

Une **relation argumentative de conséquence** est définie lorsqu'un sous-ensemble **A** de la base de connaissances **Σ** satisfait trois conditions :
1. **Consistance** : **A** est cohérent, c'est-à-dire qu'il ne mène pas à une contradiction.
2. **Validité** : **A** permet de déduire la formule **φ** (ce que l'on cherche à prouver).
3. **Minimalité** : **A** est minimal, c'est-à-dire qu'aucune formule supplémentaire ne peut être retirée sans perdre la validité de **φ**.

On dit alors que **A** est un **argument pour φ**. Cette approche permet de raisonner à partir de sous-ensembles cohérents d'une base de connaissances incohérente.

##### **Exemple**

Supposons que nous ayons les informations suivantes :
- « Il pleut. »
- « Si la pluie tombe, je dois utiliser un parapluie. »
- « J'aime le paprika. »

Ici, seul le sous-ensemble composé de « il pleut » et « si la pluie tombe, je dois utiliser un parapluie » est pertinent pour déduire que je dois utiliser un parapluie. L'information concernant le paprika est superflue pour cette conclusion et n'a pas d'impact sur la validité de l'argument.

---

##### **Relation argumentative de conséquence : Définition formelle**

La formule **φ** est une **conséquence argumentative de Σ**, notée **Σ `A φ**, s'il existe un argument pour **φ** dans **Σ**, et qu'il n'existe aucun argument pour **¬φ** dans **Σ**. Cela signifie que l'on peut déduire **φ** de **Σ** sans contradiction.

##### **Exemple d'application**

Imaginons une discussion au sein de la rédaction d'un journal sur la publication ou non d'informations compromettantes concernant un politicien. Les informations suivantes sont disponibles :
1. Simon Jones est membre du Parlement.
2. Si Simon Jones est membre du Parlement, nous n'avons pas besoin de taire des détails sur sa vie privée.
3. Simon Jones vient de démissionner du Parlement.
4. Si Simon Jones a démissionné, il n'est plus membre du Parlement.
5. Si Simon Jones n'est plus membre du Parlement, nous devons taire des détails sur sa vie privée.

Cet exemple illustre la manière dont des arguments peuvent être formés à partir de sous-ensembles cohérents de la base de connaissances, en déduisant des conclusions malgré des contradictions potentielles.

---

### Gestion des incohérences - Inférences non classiques et conséquences argumentatives

---

Dans cette seconde partie du cours sur la gestion des incohérences, nous allons explorer plus en profondeur les différentes méthodes permettant de traiter des bases de connaissances contradictoires. Nous avons vu dans la première partie comment des contradictions peuvent apparaître, et nous allons maintenant examiner des techniques d'**inférence non classique** et de **gestion des incohérences** pour maintenir des raisonnements valides, même en présence de conflits dans les informations.

---

### **Inférence argumentative vs inférence classique**

L'inférence **classique** repose sur la logique déductive standard, où une conclusion est déduite à partir de prémisses valides sans qu'il y ait de contradictions dans la base de connaissances. En revanche, l'**inférence argumentative** est utilisée pour traiter les incohérences et permet de déduire des conclusions même lorsque certaines informations sont en conflit.

L'objectif est de déterminer dans quelles conditions une conséquence argumentative peut coïncider avec une conséquence classique. Autrement dit, nous cherchons à savoir sous quelles conditions la relation suivante est vraie :  
**Σ `φ si et seulement si Σ`A φ**.

Dans de nombreux cas, la **conséquence argumentative** fournit une manière plus flexible de traiter des bases de connaissances incohérentes, en tolérant certains conflits tout en permettant des inférences partielles.

---

### **Conséquence libre (Free Consequence)**

La notion de **conséquence libre** repose sur l'idée d'exclure les formules impliquées dans des incohérences et de ne raisonner qu'avec celles qui sont **cohérentes**. Ainsi, une **formule libre** est définie comme une formule qui ne fait pas partie d'un sous-ensemble inconsistant de la base de connaissances.

#### **Définitions** :

- **Formule libre** : Une formule **ϕ** est dite libre dans **Σ** si **ϕ** ne fait pas partie d'un sous-ensemble minimalement inconsistant de **Σ**.
    
    - **Free(Σ)** est l'ensemble des formules libres dans **Σ**, c'est-à-dire l'ensemble des formules qui ne sont pas impliquées dans des incohérences.
- **Conséquence libre** : Une formule **φ** est une conséquence libre de **Σ**, notée **Σ `Free φ**, si et seulement si **Free(Σ)` φ**.
    

##### **Exemple** :

- Soit **Σ = {φ, ¬φ ∨ ¬ψ, ψ, ¬φ ∨ ϕ, ¬ψ ∨ ϕ}**.
    - Dans ce cas, nous identifions les formules qui sont impliquées dans des incohérences et nous ne raisonnons qu'avec celles qui ne le sont pas.

---

### **Conséquence universelle (MC-consequence)**

La **conséquence universelle**, ou **MC-consequence**, repose sur le concept de **sous-bases maximales cohérentes**. L'idée est de ne raisonner qu'avec les sous-ensembles maximaux de la base de connaissances qui sont cohérents. Ces sous-ensembles sont appelés **MC(Σ)**.

#### **Définitions** :

- **MC(Σ)** est l'ensemble des sous-bases maximales cohérentes de **Σ**.
- Une formule **φ** est une **conséquence universelle** de **Σ**, notée **Σ `MC φ**, si pour chaque sous-base cohérente **A** dans **MC(Σ)**, **A` φ**.

##### **Exemple** :

- Soit **Σ = {φ, ¬φ ∨ ¬ψ, ψ, ¬φ ∨ ϕ, ¬ψ ∨ ϕ}**.
    - Nous cherchons les sous-ensembles maximaux de **Σ** qui sont cohérents, et nous raisonnons avec ces sous-ensembles.

---

### **Relation entre les conséquences MC et argumentatives**

L'une des questions importantes est de comprendre comment les conséquences **MC-consequence** et **argumentatives** sont liées. Dans certains cas, les conclusions obtenues à partir d'arguments (conséquences argumentatives) peuvent coïncider avec les conclusions obtenues à partir des sous-bases maximales cohérentes (conséquences universelles), mais ce n'est pas toujours le cas.

L'objectif est donc d'étudier ces relations pour comprendre dans quelles situations il est préférable d'utiliser l'une ou l'autre de ces méthodes d'inférence.

---

### **Conséquence lexicographique (L-consequence)**

La **conséquence lexicographique** est une approche basée sur un critère de cardinalité. Elle prend en compte les sous-ensembles maximaux cohérents, mais au lieu de les considérer tous, elle se concentre sur ceux qui ont le plus grand nombre d'éléments cohérents.

#### **Définitions** :

- Un sous-ensemble **A** appartient à **L(Σ)** s'il fait partie de **MC(Σ)** et s'il a une cardinalité supérieure ou égale à celle de tous les autres sous-ensembles maximaux cohérents.
    - **|A|** représente la cardinalité de l'ensemble **A**.
- Une formule **φ** est une **L-consequence** de **Σ**, notée **Σ `L φ**, si pour chaque sous-ensemble **A** dans **L(Σ)**, **A` φ**.

##### **Exemple** :

- Soit **Σ = {ψ → φ, ψ, ¬ψ, ¬φ ∧ ¬ψ, ¬φ}**.
    - Nous identifions les sous-ensembles maximaux cohérents et nous sélectionnons ceux ayant la plus grande cardinalité pour raisonner avec eux.

---

### **Conséquence existentielle (Existential Consequence)**

La **conséquence existentielle** est une méthode plus flexible pour effectuer des inférences. Elle repose sur l'idée qu'il suffit qu'un sous-ensemble cohérent permette de déduire une conclusion pour que cette conclusion soit acceptée.

#### **Définitions** :

- Une formule **φ** est une **conséquence existentielle** de **Σ**, notée **Σ `∃ φ**, s'il existe au moins un sous-ensemble cohérent **A** dans **MC(Σ)** tel que **A` φ**.

##### **Exemple** :

- Soit **Σ = {ψ → φ, ψ, ¬ψ, ¬φ ∧ ¬ψ, ¬φ}**.
    - Il suffit qu'un seul sous-ensemble maximal cohérent permette de déduire **φ** pour que **φ** soit considéré comme une conséquence existentielle.

---

### **Lien entre les différentes relations de conséquence**

À ce stade, nous avons présenté plusieurs méthodes pour gérer les incohérences et effectuer des inférences dans des bases de connaissances contradictoires. Il est important de comprendre les relations entre ces différentes méthodes :

- **Conséquences argumentatives**,
- **Conséquences libres**,
- **Conséquences universelles (MC-consequence)**,
- **Conséquences lexicographiques (L-consequence)**,
- **Conséquences existentielles**.

Chaque méthode a ses avantages et ses inconvénients en fonction du contexte et du niveau de tolérance aux incohérences. Le choix de la méthode dépendra de la complexité de la base de connaissances, du type d'incohérences présentes, et des objectifs de l'inférence.

---
#### Gestion des incohérences - Bases de connaissances priorisées et inférence

---

Dans cette troisième partie, nous aborderons la gestion des incohérences dans les **bases de connaissances priorisées**, où les informations sont classées en fonction de leur importance. Nous explorerons les méthodes d’inférence associées, telles que l’inférence possibiliste, l’inférence linéaire et l’inférence argumentative. L'objectif est de comprendre comment les priorités peuvent simplifier la résolution des conflits et améliorer la gestion des incohérences dans une base de connaissances.

---

### **Qu'est-ce qu'une priorité et d'où vient-elle ?**

Une **priorité** représente un degré d'importance, d'incertitude ou de préférence associé à une connaissance. Dans les systèmes à base de connaissances, l’introduction de priorités permet de simplifier la gestion des conflits en accordant plus de poids aux informations jugées plus fiables ou plus importantes. Ainsi, lorsqu'une contradiction survient, les informations ayant une plus grande priorité sont conservées, tandis que celles de moindre importance peuvent être ignorées ou révisées.

##### **Exemple de priorité** :

- **Fiabilité des sources** : Un rapport d'expert aura probablement plus de poids qu'une rumeur ou une information non vérifiée.
- **Préférence dans les connaissances** : Certaines règles de décision peuvent être jugées plus importantes que d'autres.

Les priorités jouent donc un rôle essentiel dans la gestion des incohérences, car elles permettent de résoudre des conflits plus facilement en définissant des niveaux d'importance parmi les formules.

---

### **Bases de connaissances priorisées**

Une **base de connaissances priorisée** est une base structurée en **couches**, où chaque couche contient des formules ayant le même niveau de priorité. Les formules dans une couche donnée sont considérées plus importantes que celles des couches inférieures.

#### **Définition** :

- Soit **Σ = S1 ∪ S2 ∪ · · · ∪ Sn**, une base de connaissances stratifiée où les formules dans **S1** ont la plus haute priorité, et celles dans **Sn** la plus basse.
- Chaque **Si** est un multiensemble, ce qui signifie qu'une même formule peut apparaître plusieurs fois dans la même couche ou dans différentes couches.

La notation **φ ∈i Σ** indique que la formule **φ** appartient à la couche **Si**. Cette structure permet de définir des **sous-bases** cohérentes, c’est-à-dire des ensembles de formules n'entraînant pas d'incohérence.

---

### **Inférence possibiliste**

L'**inférence possibiliste** est une méthode qui exploite la structure en couches pour tirer des conclusions à partir des couches les plus prioritaires. L'idée est de raisonner d'abord avec les informations les plus fiables, en n'intégrant des informations moins prioritaires que si elles ne contredisent pas les formules des couches supérieures.

#### **Définition : i-consequence**

Une formule **φ** est une **i-consequence** de **Σ**, notée **Σ `i φ**, si :

1. **S1 ∪ · · · ∪ Si** est cohérent,
2. **S1 ∪ · · · ∪ Si ` φ**,
3. Pour tout **j < i**, **S1 ∪ · · · ∪ Sj** ne permet pas de déduire **φ**.

#### **Définition : π-consequence**

Une formule **φ** est une **π-consequence** de **Σ** si elle est une **i-consequence** pour un certain **i**, notée **Σ `π φ**.

##### **Exemple** :

Soit **Σ = {{p}, {¬p ∨ b, ¬p ∨ ¬f}, {¬b ∨ f}}**.

- On calcule l’ensemble des formules cohérentes à partir des couches les plus prioritaires pour déterminer les conséquences possibilistes.

---

### **Problèmes liés à l'inférence possibiliste**

L'inférence possibiliste, bien qu'efficace dans certains cas, souffre du problème du **drowning effect** (effet de noyade). Ce phénomène survient lorsque des informations importantes sont noyées par des conflits dans des couches inférieures, empêchant ainsi l’héritage de propriétés.

##### **Exemple du drowning effect** :

Supposons que nous ayons les informations suivantes :

- **p** : pingouin,
- **b** : oiseau,
- **f** : voler,
- **w** : ailes.

Nous pouvons exprimer que les oiseaux volent, mais que les pingouins, bien que des oiseaux, ne volent pas. Le problème du drowning effect peut empêcher l’héritage de certaines propriétés, comme le fait que les pingouins ont des ailes, même si la seule propriété que nous souhaitons inhiber est le vol.

---

### **Conséquence libre dans les bases priorisées**

Pour résoudre le problème du drowning effect, une approche consiste à définir la notion de **conséquence libre**. Cette méthode identifie les sous-bases cohérentes et maximise l'utilisation des formules non impliquées dans des incohérences.

#### **Définition : Sous-base dominante**

La **sous-base dominante** de **Σ** est définie comme : **Σ∗ = Free(S1) ∪ Free(S1 ∪ S2) ∪ · · · ∪ Free(S1 ∪ · · · ∪ Sn)**, où **Free(S1 ∪ · · · ∪ Si)** représente l'ensemble des formules libres dans les couches jusqu'à **Si**.

#### **Définition : Conséquence libre**

Une formule **φ** est une **conséquence libre** de **Σ**, notée **Σ `ND φ**, si **Σ∗` φ**.

---

### **Conséquence linéaire**

L'**inférence linéaire** est une autre méthode pour gérer les incohérences en éliminant les couches incohérentes. Contrairement à l'inférence possibiliste, cette méthode élimine directement les couches qui entraînent des conflits.

#### **Définition : Sous-base linéaire**

La sous-base **l(Σ)** est obtenue en supprimant les couches incohérentes. Elle est construite de la manière suivante :

- **l(S1) = S1** si **S1** est cohérent, sinon **l(S1) = ∅**,
- Pour **i > 1**, **l(S1 ∪ · · · ∪ Si) = l(S1 ∪ · · · ∪ Si−1) ∪ Si** si l'ensemble est cohérent, sinon **l(S1 ∪ · · · ∪ Si) = l(S1 ∪ · · · ∪ Si−1)**.

#### **Définition : Conséquence linéaire**

Une formule **φ** est une **conséquence linéaire** de **Σ**, notée **Σ `l φ**, si **l(Σ)` φ**.

##### **Exemple** :

Soit **Σ = {{p}, {¬p ∨ b, ¬p ∨ ¬f}, {¬b ∨ f, ¬b ∨ w}}**. L'inférence linéaire permet de déduire des conséquences en éliminant les couches incohérentes, mais cette méthode ne résout pas toujours complètement le problème du drowning effect.

---

### **Inférence argumentative dans les bases priorisées**

L'**inférence argumentative** est adaptée aux bases priorisées en tenant compte des rangs de priorité des formules pour résoudre les contradictions. Un **argument de rang i** pour une formule **φ** doit être construit à partir de sous-ensembles cohérents des couches jusqu’à **Si**.

#### **Définition : Argument de rang i**

Un sous-ensemble **A** de **Σ** est un argument de rang **i** pour **φ** si :

1. **A** est cohérent,
2. **A ` φ**,
3. Pour toute formule **ψ** dans **A**, **A \ {ψ}** ne permet plus de déduire **φ**,
4. **R(A) = i**, c'est-à-dire que la couche la plus basse contenant des formules de **A** est **Si**.

#### **Définition : Conséquence argumentative**

Une formule **φ** est une **conséquence argumentative** de **Σ**, notée **Σ `A φ**, s’il existe un argument de rang **i** pour **φ** dans **Σ**, et que les arguments pour **¬φ**, s'ils existent, sont d'un rang supérieur.

##### **Exemple** :

Soit **Σ = {{¬c}, {¬a ∨ ¬b ∨ c, ¬d ∨ c, ¬e ∨ c}, {d, e, f, ¬f ∨ ¬g ∨ c}, {a, b, g, h}}**.

- Ici, l'inférence argumentative permet de déduire des conclusions tout en tenant compte des priorités et des contradictions.