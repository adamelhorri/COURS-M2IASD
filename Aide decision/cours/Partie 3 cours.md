---
tags:
  - aideDecision
  - cours
---

# Gestion des connaissances pour l'aide à la décision : Fusion d'informations

## Introduction à la fusion d’informations
La **fusion d’informations** est un processus crucial dans la gestion des connaissances pour l’aide à la décision. Elle permet de combiner plusieurs sources d’informations afin de produire un résultat consolidé, tout en gérant les incohérences et les conflits potentiels entre les données. Dans ce contexte, les bases de données peuvent être exprimées sous forme propositionnelle, et différentes méthodes sont appliquées en fonction de la priorité explicite ou implicite des bases.

---

## 1. Fusion de bases propositionnelles sans priorité

### 1.1 Union des bases
Dans ce cas, la fusion consiste simplement à **unir** toutes les formules issues des différentes bases propositionnelles. Le résultat de l’union est souvent un ensemble incohérent car les différentes bases peuvent contenir des formules contradictoires.

**Exemple** : Si une base \( K_1 \) contient la proposition \( p \) et une autre base \( K_2 \) contient \( \neg p \), l’union des deux bases contiendra à la fois \( p \) et \( \neg p \), ce qui est incohérent.

#### Gestion de l’incohérence
Face à cette incohérence, des méthodes de gestion des conflits doivent être appliquées pour obtenir un ensemble cohérent d’informations. Les méthodes couramment utilisées incluent :
- **L’argumentation** : Consiste à analyser les conflits et à défendre les propositions les plus plausibles.
- **La Sémantique des Modèles Coalisés (SMC)** : Permet de sélectionner les modèles préférés en fonction des préférences sous-jacentes.

### 1.2 Application des méthodes de gestion d’incohérence
Les bases sont d’abord unies pour former un ensemble global, et les méthodes de gestion d’incohérence sont appliquées pour résoudre les contradictions et trouver un compromis acceptable entre les différentes propositions. Ce processus assure que les informations finales sont cohérentes et exploitables pour la prise de décision.

---

## 2. Fusion de bases propositionnelles avec priorité implicite

Dans ce type de fusion, aucune **relation d’ordre explicite** n'est définie a priori entre les bases. Cependant, un ordre implicite est calculé pour chaque base sur la base de la proximité des interprétations possibles. Cette approche est utile lorsque toutes les bases ne sont pas égales en importance ou qu’il n’existe pas d’ordre de priorité explicite.

### 2.1 Principe de calcul des distances
L'idée centrale est de mesurer la **proximité des interprétations** (outcomes) par rapport aux informations contenues dans les bases. Cette proximité est évaluée à l'aide de **distances**, telles que la **distance de Hamming**, qui permet de quantifier le nombre de différences entre deux interprétations logiques.

### 2.2 Processus de fusion en trois étapes
Le processus de fusion se décompose en trois étapes clés :

1. **Étape 1 : Calcul des distances locales**
   Chaque interprétation \( \omega \) est comparée à chaque base propositionnelle \( K_i \) en calculant la distance locale \( d(\omega, K_i) \). La distance de Hamming est souvent utilisée à cet effet :
   \[
   d(\omega, K_i) = \min_{\omega' \models K_i} H(\omega, \omega')
   \]
   où \( H(\omega, \omega') \) représente le nombre de littéraux différents entre \( \omega \) et \( \omega' \).

2. **Étape 2 : Agrégation des distances locales**
   Une distance globale est ensuite calculée en agrégeant les distances locales avec un opérateur d’agrégation noté \( \oplus \), comme :
   \[
   d_\oplus(\omega, E) = \oplus_{i=1}^n d(\omega, K_i)
   \]
   Les distances globales permettent de définir un pré-ordre total sur l’ensemble des interprétations.

3. **Étape 3 : Résultat de la fusion**
   Le résultat de la fusion, noté \( \text{Merge}_\oplus(E) \), est constitué des interprétations les plus proches de l’ensemble des bases :
   \[
   \text{Merge}_\oplus(E) = \max(\Omega, \preceq_\oplus)
   \]
   Les interprétations préférées sont celles qui minimisent la distance globale.

### 2.3 Exemple de fusion implicite
**Scénario** : Un professeur demande à ses trois étudiants quels langages ils souhaitent étudier parmi SQL (\( s \)), O2 (\( o \)), et Datalog (\( d \)).

- **Étudiant 1** : Il souhaite étudier SQL ou O2, mais pas Datalog. Sa base est notée \( K_1 = (s \lor o) \land \neg d \).
- **Étudiant 2** : Il refuse SQL et préfère étudier soit O2, soit Datalog, mais pas les deux en même temps : \( K_2 = (\neg s \land d \land \neg o) \lor (\neg s \land \neg d \land o) \).
- **Étudiant 3** : Il veut étudier les trois langages : \( K_3 = s \land d \land o \).

#### 2.3.1 Calcul des distances locales
On calcule d'abord les distances locales pour chaque interprétation \( \omega \) par rapport aux bases \( K_1 \), \( K_2 \), et \( K_3 \). Ces distances sont présentées dans le tableau suivant :

| Interprétation \( \omega \) | Distance \( d(\omega, K_1) \) | Distance \( d(\omega, K_2) \) | Distance \( d(\omega, K_3) \) |
|:--------------------------:|:-----------------------------:|:-----------------------------:|:-----------------------------:|
| \( \omega_0 \) : \( \neg s \neg d \neg o \) | 1 | 1 | 3 |
| \( \omega_1 \) : \( \neg s \neg d o \)  | 0 | 0 | 2 |
| \( \omega_2 \) : \( \neg s d \neg o \)  | 2 | 0 | 2 |
| \( \omega_3 \) : \( \neg s d o \)       | 1 | 1 | 1 |
| \( \omega_4 \) : \( s \neg d \neg o \)  | 0 | 2 | 2 |
| \( \omega_5 \) : \( s \neg d o \)       | 0 | 1 | 1 |
| \( \omega_6 \) : \( s d \neg o \)       | 1 | 1 | 1 |
| \( \omega_7 \) : \( s d o \)            | 1 | 2 | 0 |

#### 2.3.2 Calcul des distances globales
Différents opérateurs d’agrégation peuvent être utilisés pour combiner les distances locales :

- **Opérateur WS** (Weighted Sum) : Somme pondérée des distances locales.
- **Opérateur majorité** : Favorise la satisfaction de la majorité des bases.
- **Opérateur max** : Prend en compte la base qui impose la plus grande distance.
- **Opérateur min** : Favorise les interprétations minimisant la distance à toutes les bases.

Exemple d'agrégation avec \( k_1 = 1 \), \( k_2 = 3 \), et \( k_3 = 1 \) pour l'opérateur WS :

|  \( \omega \)  | \( d_1 \) | \( d_2 \) | \( d_3 \) | \( \sum \) | \( WS \) | \( MAX \) | \( MIN \) | \( GMAX \) |
| :------------: | :-------: | :-------: | :-------: | :--------: | :------: | :-------: | :-------: | :--------: |
| \( \omega_0 \) |     1     |     1     |     3     |     5      |    7     |     3     |     1     |  (3,1,1)   |
| \( \omega_1 \) |     0     |     0     |     2     |     2      |    2     |     2     |     0     |  (2,0,0)   |
| \( \omega_2 \) |     2     |     0     |     2     |     4      |    4     |     2     |     0     |  (2,2,0)   |
| \( \omega_3 \) |     1     |     1     |     1     |     3      |    5     |     1     |     1     |  (1,1,1)   |
| \( \omega_4 \) |     0     |     2     |     2     |     4      |    8     |     2     |     0     |  (2,2,0)   |
| \( \omega_5 \) |     0     |     1     |     1     |     2      |    4     |     1     |     0     |  (1,1,0)   |
| \( \omega_6 \) |     1     |     1     |     1     |     3      |    5     |     1     |     1     |  (1,1,1)   |
### 3.1 Introduction à la logique possibiliste

La **logique possibiliste** est une approche qui permet de modéliser l’incertitude associée aux informations. Elle est particulièrement adaptée lorsque les données fournies par différentes sources ne sont pas toujours certaines. Chaque formule propositionnelle est associée à un **degré de possibilité**, qui reflète le niveau de confiance ou de certitude que l’on accorde à cette information.

Dans le cadre de la fusion d’informations, la logique possibiliste permet de combiner des bases possibilistes provenant de différentes sources tout en prenant en compte ces degrés de confiance.

### 3.2 Principe de la fusion possibiliste

Soient B1,B2,…,BnB_1, B_2, \dots, B_nB1​,B2​,…,Bn​ des bases possibilistes, où chaque base BiB_iBi​ contient des formules associées à des degrés de possibilité. Le processus de fusion permet de produire une nouvelle base possibiliste B⊕B_\oplusB⊕​, qui combine les informations de toutes les bases en fonction d'un opérateur d’agrégation ⊕\oplus⊕. Cet opérateur détermine la manière dont les degrés de possibilité sont combinés.

#### Fusion de deux bases possibilistes

Pour deux bases possibilistes B1B_1B1​ et B2B_2B2​, le résultat de la fusion peut être exprimé comme suit :

1. **Formules présentes dans les deux bases** : Les formules communes à B1B_1B1​ et B2B_2B2​ sont combinées à l’aide de l’opérateur ⊕\oplus⊕ appliqué à leurs degrés de possibilité. Par exemple, si une formule ϕ\phiϕ est présente dans B1B_1B1​ avec un degré a1a_1a1​ et dans B2B_2B2​ avec un degré a2a_2a2​, le degré résultant après fusion sera 1−⊕(1−a1,1−a2)1 - \oplus(1 - a_1, 1 - a_2)1−⊕(1−a1​,1−a2​).
    
2. **Formules propres à chaque base** : Les formules qui n’apparaissent que dans une seule des deux bases sont conservées avec un nouveau degré de possibilité, qui est ajusté par l’opérateur d’agrégation.
    

### 3.3 Exemples d’opérateurs de fusion possibiliste

1. **Opérateur min** :
    
    - Cet opérateur prend le minimum des degrés de possibilité des formules communes entre deux bases :
    
    Bmin={(ϕ∨ψ,min⁡(ai,bj))∣(ϕ,ai)∈B1,(ψ,bj)∈B2}B_{\text{min}} = \{(\phi \lor \psi, \min(a_i, b_j)) \mid (\phi, a_i) \in B_1, (\psi, b_j) \in B_2\}Bmin​={(ϕ∨ψ,min(ai​,bj​))∣(ϕ,ai​)∈B1​,(ψ,bj​)∈B2​}
    - **Avantage** : Cet opérateur garantit que le résultat est cohérent si au moins une des bases est cohérente.
    - **Inconvénient** : Il ne renforce pas les informations redondantes.
2. **Opérateur max** :
    
    - Il prend le maximum des degrés de possibilité pour chaque formule :
    
    Bmax={(ϕ∨ψ,max⁡(ai,bj))∣(ϕ,ai)∈B1,(ψ,bj)∈B2}B_{\text{max}} = \{(\phi \lor \psi, \max(a_i, b_j)) \mid (\phi, a_i) \in B_1, (\psi, b_j) \in B_2\}Bmax​={(ϕ∨ψ,max(ai​,bj​))∣(ϕ,ai​)∈B1​,(ψ,bj​)∈B2​}
    - **Avantage** : Il est particulièrement utile pour maintenir la cohérence des informations, même lorsque certaines formules sont contradictoires.
    - **Inconvénient** : Il ne permet pas de renforcer les informations partagées par plusieurs bases.
3. **Opérateur produit** :
    
    - Cet opérateur combine les degrés de possibilité en multipliant les valeurs des formules communes :
    
    B∗=B1∪B2∪{(ϕ∨ψ,ai×bj)∣(ϕ,ai)∈B1,(ψ,bj)∈B2}B_* = B_1 \cup B_2 \cup \{(\phi \lor \psi, a_i \times b_j) \mid (\phi, a_i) \in B_1, (\psi, b_j) \in B_2\}B∗​=B1​∪B2​∪{(ϕ∨ψ,ai​×bj​)∣(ϕ,ai​)∈B1​,(ψ,bj​)∈B2​}
    - **Avantage** : Il permet de renforcer les informations redondantes, c'est-à-dire les formules présentes dans plusieurs bases avec des degrés de possibilité élevés.

### 3.4 Propriétés des opérateurs de fusion possibiliste

- **Associativité** : Un opérateur est dit associatif si le résultat de la fusion ne dépend pas de l’ordre dans lequel les bases sont fusionnées. Par exemple, Merge(B1,B2,B3)\text{Merge}(B_1, B_2, B_3)Merge(B1​,B2​,B3​) donne le même résultat que Merge(Merge(B1,B2),B3)\text{Merge}(\text{Merge}(B_1, B_2), B_3)Merge(Merge(B1​,B2​),B3​). L’opérateur produit est un exemple d’opérateur associatif.
    
- **Idempotence** : Un opérateur est idempotent si fusionner plusieurs fois la même base ne change pas le résultat. Cela garantit que l’ajout d’informations redondantes ne modifie pas inutilement le résultat de la fusion.
    
- **Renforcement des informations redondantes** : Certains opérateurs, comme l’opérateur produit, permettent de renforcer les informations répétées dans plusieurs bases en augmentant leur degré de possibilité après fusion.
    

### 3.5 Exemple de fusion possibiliste

Prenons deux bases possibilistes B1B_1B1​ et B2B_2B2​ avec les formules suivantes :

- **B1B_1B1​** : {(¬c∨¬s∨e,1),(¬c,0.8),(s,0.5)}\{(\neg c \lor \neg s \lor e, 1), (\neg c, 0.8), (s, 0.5)\}{(¬c∨¬s∨e,1),(¬c,0.8),(s,0.5)}
- **B2B_2B2​** : {(¬c∨¬s∨e,1),(¬c,0.6),(e,0.2)}\{(\neg c \lor \neg s \lor e, 1), (\neg c, 0.6), (e, 0.2)\}{(¬c∨¬s∨e,1),(¬c,0.6),(e,0.2)}

Les résultats de la fusion avec différents opérateurs sont :

- **BminB_{\text{min}}Bmin​** :
    
    Bmin={(¬c∨¬s∨e,1),(¬c,0.8),(s,0.5),(e,0.2)}B_{\text{min}} = \{(\neg c \lor \neg s \lor e, 1), (\neg c, 0.8), (s, 0.5), (e, 0.2)\}Bmin​={(¬c∨¬s∨e,1),(¬c,0.8),(s,0.5),(e,0.2)}
    
    Ici, l’opérateur min conserve les formules avec le plus petit degré de possibilité lorsqu’elles sont communes aux deux bases.
    
- **BmaxB_{\text{max}}Bmax​** :
    
    Bmax={(¬c∨¬s∨e,1),(¬c,0.6),(s∨e,0.2)}B_{\text{max}} = \{(\neg c \lor \neg s \lor e, 1), (\neg c, 0.6), (s \lor e, 0.2)\}Bmax​={(¬c∨¬s∨e,1),(¬c,0.6),(s∨e,0.2)}
    
    L’opérateur max maintient les formules avec le degré de possibilité le plus élevé pour chaque information.
    
- **B∗B_*B∗​** :
    
    B∗={(¬c∨¬s∨e,1),(¬c,0.92),(s∨e,0.6),(s,0.5),(e,0.2)}B_* = \{(\neg c \lor \neg s \lor e, 1), (\neg c, 0.92), (s \lor e, 0.6), (s, 0.5), (e, 0.2)\}B∗​={(¬c∨¬s∨e,1),(¬c,0.92),(s∨e,0.6),(s,0.5),(e,0.2)}
    
    L’opérateur produit renforce les informations répétées en augmentant leur degré de possibilité.
    

---

## 4. Applications de la fusion d’informations

La fusion d’informations possède de nombreuses applications dans divers domaines :

1. **Robotique** : Les systèmes de fusion d’informations sont utilisés pour combiner les données provenant de multiples capteurs, permettant ainsi à un robot de construire une représentation cohérente de son environnement et de prendre des décisions en temps réel.
    
2. **Systèmes multi-agents** : Dans les systèmes où plusieurs agents collaborent pour accomplir des tâches, la fusion d’informations permet de combiner les connaissances de chaque agent et d’établir une vue d’ensemble partagée.
    
3. **Décision collective** : Lors de prises de décisions collectives, telles que la fusion des préférences ou l’agrégation des jugements, la fusion d’informations permet de combiner les points de vue et les préférences des différents individus ou parties prenantes pour aboutir à une décision commune.
    

---

## 5. Conclusion

La fusion d’informations est un domaine clé dans la gestion des connaissances pour l’aide à la décision. Elle permet de combiner des données hétérogènes tout en gérant les incohérences et en s’assurant que le résultat est exploitable pour la prise de décision. Selon les priorités explicites ou implicites, et en fonction des degrés de possibilité associés aux informations, différents opérateurs de fusion peuvent être utilisés pour obtenir un résultat cohérent, fiable et pertinent.