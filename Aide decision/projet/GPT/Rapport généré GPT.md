---
tags:
  - aideDecision
---
# Rapport sur les Algorithmes d’Affectation

**Souhila KACI  
Aide à la décision / Decision Aid  
Partie 4 : Algorithmes d’affectation**

---

## Introduction

L'affectation des élèves aux établissements scolaires représente une tâche complexe et cruciale pour garantir une éducation équitable et adaptée aux besoins individuels des élèves. Ce processus doit non seulement tenir compte des préférences des élèves, mais également des capacités des écoles et des priorités établies par les autorités éducatives. Les algorithmes d’affectation jouent un rôle fondamental en automatisant et en optimisant ce processus, assurant ainsi une distribution juste et transparente des places disponibles.

Ce rapport se concentre sur deux algorithmes d’affectation largement utilisés : l'**Algorithme de Boston** et l'**Algorithme d’Acceptation Différée** (Deferred Acceptance) développé par Gale et Shapley en 1962. À travers un exemple concret impliquant 20 élèves et 5 écoles, nous analyserons le fonctionnement de ces algorithmes, leurs résultats et leurs implications selon divers critères normatifs tels que l'efficacité, l'équité, la non-manipulabilité et la stabilité.

---

## Contexte et Motivation

L'affectation scolaire est souvent confrontée à des défis tels que les inscriptions multiples des élèves, la gestion des capacités limitées des écoles et la nécessité de respecter des critères de priorité variés. Une procédure d’affectation efficace doit donc simplifier la gestion des inscriptions, garantir la transparence des décisions et assurer une adéquation optimale entre les aptitudes des élèves et les formations offertes par les établissements.

Les algorithmes d’affectation offrent une solution systématique et objective pour répondre à ces exigences. Ils permettent de traiter un grand nombre de données de manière rapide et précise, minimisant ainsi les biais et les erreurs humaines. De plus, ces algorithmes facilitent l'acceptation des résultats par les parties prenantes grâce à leur logique transparente et équitable.

---

## Description des Données

### Élèves et Écoles

Pour illustrer le fonctionnement des algorithmes, nous avons utilisé un ensemble de données comprenant 20 élèves et 5 écoles. Chaque école dispose d'une capacité d'accueil de 4 élèves, totalisant ainsi 20 places disponibles. Les élèves sont nommés de manière unique pour faciliter le suivi des affectations.

#### Liste des Élèves

1. Alice
2. Bob
3. Charlie
4. Diana
5. Ethan
6. Fiona
7. George
8. Hannah
9. Ian
10. Julia
11. Kevin
12. Laura
13. Michael
14. Nina
15. Oscar
16. Paula
17. Quentin
18. Rachel
19. Steve
20. Tina

#### Liste des Écoles

1. École A
2. École B
3. École C
4. École D
5. École E

### Préférences des Élèves

Chaque élève a une liste ordonnée de préférences pour les écoles, reflétant leurs aspirations académiques et personnelles. Ces préférences sont déterminées de manière à représenter une distribution réaliste des choix.

| Élève    | 1er Choix | 2ème Choix | 3ème Choix | 4ème Choix | 5ème Choix |
|----------|-----------|------------|------------|------------|------------|
| Alice    | École B   | École A    | École E    | École C    | École D    |
| Bob      | École A   | École C    | École B    | École E    | École D    |
| Charlie  | École C   | École B    | École A    | École D    | École E    |
| Diana    | École D   | École A    | École B    | École C    | École E    |
| Ethan    | École E   | École B    | École A    | École C    | École D    |
| Fiona    | École B   | École E    | École A    | École D    | École C    |
| George   | École A   | École C    | École B    | École E    | École D    |
| Hannah   | École C   | École B    | École D    | École A    | École E    |
| Ian      | École D   | École A    | École B    | École C    | École E    |
| Julia    | École E   | École B    | École A    | École D    | École C    |
| Kevin    | École B   | École C    | École A    | École D    | École E    |
| Laura    | École A   | École B    | École E    | École C    | École D    |
| Michael  | École C   | École A    | École B    | École D    | École E    |
| Nina     | École D   | École E    | École B    | École C    | École A    |
| Oscar    | École E   | École B    | École A    | École D    | École C    |
| Paula    | École B   | École A    | École E    | École C    | École D    |
| Quentin  | École A   | École C    | École B    | École E    | École D    |
| Rachel   | École C   | École B    | École D    | École A    | École E    |
| Steve    | École D   | École A    | École B    | École C    | École E    |
| Tina     | École E   | École D    | École A    | École B    | École C    |

### Priorités des Écoles

Chaque école établit une liste de priorités parmi les élèves, déterminée par des critères spécifiques tels que les performances académiques, les besoins particuliers ou d'autres facteurs pertinents. Ces priorités sont également déterminées de manière réaliste pour refléter une distribution équitable et compétitive.

| École    | Priorité 1 | Priorité 2 | Priorité 3 | Priorité 4 | Priorité 5 | Priorité 6 | Priorité 7 | Priorité 8 | Priorité 9 | Priorité 10 | Priorité 11 | Priorité 12 | Priorité 13 | Priorité 14 | Priorité 15 | Priorité 16 | Priorité 17 | Priorité 18 | Priorité 19 | Priorité 20 |
|----------|------------|------------|------------|------------|------------|------------|------------|------------|------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|-------------|
| École A  | Alice      | Bob        | Charlie    | Diana      | Ethan      | Fiona      | George     | Hannah     | Ian        | Julia       | Kevin       | Laura       | Michael     | Nina        | Oscar       | Paula       | Quentin     | Rachel      | Steve       | Tina        |
| École B  | Bob        | Alice      | Diana      | Charlie    | Fiona      | Ethan      | George     | Julia      | Hannah     | Ian         | Kevin       | Laura       | Michael     | Nina        | Oscar       | Paula       | Quentin     | Rachel      | Steve       | Tina        |
| École C  | Charlie    | Bob        | Alice      | Diana      | Ethan      | Fiona      | George     | Julia      | Hannah     | Ian         | Kevin       | Laura       | Michael     | Nina        | Oscar       | Paula       | Quentin     | Rachel      | Steve       | Tina        |
| École D  | Diana      | Charlie    | Bob        | Alice      | Fiona      | Ethan      | George     | Julia      | Hannah     | Ian         | Kevin       | Laura       | Michael     | Nina        | Oscar       | Paula       | Quentin     | Rachel      | Steve       | Tina        |
| École E  | Ethan      | Diana      | Charlie    | Bob        | Alice      | Fiona      | George     | Julia      | Hannah     | Ian         | Kevin       | Laura       | Michael     | Nina        | Oscar       | Paula       | Quentin     | Rachel      | Steve       | Tina        |

---

## Algorithme de Boston

### Principe et Procédure

L'Algorithme de Boston, également connu sous le nom de "Boston Mechanism", est un algorithme de première étape qui vise à satisfaire au maximum les premiers choix des élèves. Il fonctionne par itérations successives, où à chaque étape, les élèves non affectés proposent à leur k-ième choix d'école en fonction des places disponibles et des priorités des écoles.

**Étapes de l'Algorithme de Boston :**

1. **Étape 1** : Chaque élève propose à son premier choix. Les écoles examinent les propositions reçues et acceptent temporairement les élèves les mieux classés selon leurs priorités, jusqu'à épuisement des places disponibles. Les élèves non acceptés sont alors rejetés.
   
2. **Étape 2** : Les élèves rejetés à l'étape précédente proposent à leur deuxième choix. Les écoles acceptent les nouveaux candidats en fonction de leurs priorités et des places restantes. Les élèves non acceptés sont rejetés.

3. **Étapes Successives** : Ce processus se répète pour les troisième choix, quatrième choix, etc., jusqu'à ce que tous les élèves soient affectés ou que toutes les options soient épuisées.

### Exemple d'Application

Considérons notre ensemble de données avec 20 élèves et 5 écoles. Chaque école dispose de 4 places. L'Algorithme de Boston procède comme suit :

1. **Étape 1** :
   - **Propositions** : Chaque élève propose à son premier choix d'école.
   - **Acceptation** : Les écoles acceptent les élèves les mieux classés selon leurs priorités.
   - **Affectations Temporaires** :
     - École A : Alice, Bob, Charlie, Diana
     - École B : Fiona, George, Ian, Julia
     - École C : Kevin, Laura, Michael, Nina
     - École D : Oscar, Paula, Quentin, Rachel
     - École E : Steve, Tina
   - **Rejetés** : Ethan (École E a seulement 2 places acceptées).

2. **Étape 2** :
   - **Propositions** : Ethan propose à son deuxième choix, École B.
   - **Acceptation** : École B accepte Ethan, mais elle est déjà pleine. En fonction des priorités, Ethan est rejeté.
   - **Rejetés** : Ethan (a épuisé toutes ses options sans être affecté).

### Résultats

Après l'exécution de l'Algorithme de Boston, les affectations finales sont les suivantes :

| Élève    | École Assignée |
|----------|-----------------|
| Alice    | École A         |
| Bob      | École A         |
| Charlie  | École A         |
| Diana    | École A         |
| Fiona    | École B         |
| George   | École B         |
| Hannah   | École D         |
| Ian      | École B         |
| Julia    | École B         |
| Kevin    | École C         |
| Laura    | École C         |
| Michael  | École C         |
| Nina     | École C         |
| Oscar    | École D         |
| Paula    | École D         |
| Quentin  | École D         |
| Rachel   | École D         |
| Steve    | École E         |
| Tina     | École E         |
| Ethan    | Non Affecté     |


### Analyse des Résultats

L'Algorithme de Boston a réussi à affecter 19 des 20 élèves, maximisant ainsi la satisfaction globale des élèves en priorisant leurs premiers choix. Cependant, une inégalité notable est apparue avec l'élève Ethan restant non affecté malgré la disponibilité d'une place à l'École E, ce qui met en lumière les limites de l'algorithme en termes d'équité. De plus, l'Algorithme de Boston peut être manipulable, incitant certains élèves à falsifier leurs préférences pour améliorer leurs chances d'obtenir un meilleur classement.

---

## Algorithme d’Acceptation Différée (Deferred Acceptance)

### Principe et Procédure

L'Algorithme d’Acceptation Différée, élaboré par Gale et Shapley en 1962, vise à créer des appariements stables entre les élèves et les écoles. Un appariement est considéré comme stable s'il n'existe pas de paire élève-école qui préférerait être associée l'une à l'autre plutôt qu'avec leurs affectations actuelles.

**Étapes de l'Algorithme d’Acceptation Différée :**

1. **Étape 0** : Chaque élève soumet une liste ordonnée de vœux.
   
2. **Étape 1** : Les élèves proposent à leur premier choix d'école. Les écoles acceptent temporairement les propositions les mieux classées selon leurs priorités, rejetant les autres.

3. **Étape 2** : Les élèves rejetés à l'étape précédente proposent à leur deuxième choix. Les écoles réévaluent les propositions, acceptant les nouveaux candidats en fonction de leurs priorités et rejetant les moins prioritaires si nécessaire.

4. **Étapes Successives** : Ce processus se poursuit pour les troisième choix, quatrième choix, etc., jusqu'à ce qu'aucun élève ne soit rejeté ou qu'il n'y ait plus de vœux à proposer.

5. **Finalisation** : L'algorithme se termine lorsque tous les élèves sont affectés ou ont épuisé toutes leurs options, garantissant ainsi un appariement stable.

### Exemple d'Application

Reprenons notre ensemble de données avec 20 élèves et 5 écoles. L'Algorithme d’Acceptation Différée procède comme suit :

1. **Étape 1** :
   - **Propositions** : Chaque élève propose à son premier choix.
   - **Acceptation Temporaire** : Les écoles acceptent les élèves les mieux classés selon leurs priorités, jusqu'à épuisement des places disponibles.
   - **Affectations Temporaires** :
     - École A : Alice, Bob, Charlie, Diana
     - École B : Fiona, George, Ian, Julia
     - École C : Kevin, Laura, Michael, Nina
     - École D : Oscar, Paula, Quentin, Rachel
     - École E : Steve, Tina
   - **Rejetés** : Ethan (propose à son deuxième choix, École B).

2. **Étape 2** :
   - **Propositions** : Ethan propose à son deuxième choix, École B.
   - **Acceptation Temporaire** : École B est déjà pleine. École B compare Ethan avec ses candidats actuels selon ses priorités.
   - **Résultat** : École B rejette Ethan car ses priorités sont déjà remplies par des élèves mieux classés.

3. **Étape 3** :
   - **Propositions** : Ethan propose à son troisième choix, École A.
   - **Acceptation Temporaire** : École A est déjà pleine. École A compare Ethan avec ses candidats actuels selon ses priorités.
   - **Résultat** : École A rejette Ethan.

4. **Étape 4** :
   - **Propositions** : Ethan propose à son quatrième choix, École C.
   - **Acceptation Temporaire** : École C est déjà pleine. École C compare Ethan avec ses candidats actuels selon ses priorités.
   - **Résultat** : École C rejette Ethan.

5. **Étape 5** :
   - **Propositions** : Ethan propose à son cinquième choix, École D.
   - **Acceptation Temporaire** : École D est déjà pleine. École D compare Ethan avec ses candidats actuels selon ses priorités.
   - **Résultat** : École D rejette Ethan.

6. **Étape 6** :
   - **Propositions** : Ethan a épuisé toutes ses options sans être affecté.

### Résultats

Après l'exécution de l'Algorithme d’Acceptation Différée, les affectations finales sont les suivantes :

| Élève    | École Assignée |
|----------|-----------------|
| Alice    | École A         |
| Bob      | École A         |
| Charlie  | École A         |
| Diana    | École A         |
| Fiona    | École B         |
| George   | École B         |
| Hannah   | École D         |
| Ian      | École B         |
| Julia    | École B         |
| Kevin    | École C         |
| Laura    | École C         |
| Michael  | École C         |
| Nina     | École C         |
| Oscar    | École D         |
| Paula    | École D         |
| Quentin  | École D         |
| Rachel   | École D         |
| Steve    | École E         |
| Tina     | École E         |
| Ethan    | Non Affecté     |

)

### Analyse des Résultats

L'Algorithme d’Acceptation Différée a également réussi à affecter 19 des 20 élèves, laissant Ethan non affecté après avoir épuisé toutes ses options. Cet algorithme, bien que similaire à l'Algorithme de Boston en termes de résultats dans cet exemple particulier, présente des avantages significatifs en termes de stabilité et d'équité. Contrairement à l'Algorithme de Boston, il garantit que les appariements sont stables, empêchant toute paire élève-école de préférer être associée l'une à l'autre plutôt qu'avec leurs affectations actuelles. Cela évite les envies justifiées et renforce la confiance dans le système d’affectation.

De plus, l'Algorithme d’Acceptation Différée est non-manipulable, incitant les élèves à déclarer sincèrement leurs préférences sans craindre d'être désavantagés par une falsification stratégique. Cette caractéristique est essentielle pour assurer l'intégrité et l'équité du processus d’affectation.

---

## Comparaison des Algorithmes

Les algorithmes d’affectation étudiés présentent des caractéristiques distinctes en termes d'efficacité, d'équité, de non-manipulabilité et de stabilité. Cette section compare ces deux algorithmes selon ces critères, mettant en lumière leurs avantages et leurs limitations respectifs.

| **Critère**           | **Algorithme de Boston**                                     | **Algorithme d’Acceptation Différée**                         |
|-----------------------|--------------------------------------------------------------|---------------------------------------------------------------|
| **Efficacité**        | Haute pour les premiers choix, maximisant la satisfaction initiale des élèves. | Équilibrée, assurant la stabilité et l'équité, mais pouvant sacrifier une partie de l'efficacité pure. |
| **Équité**            | Faible, car il peut favoriser certains élèves au détriment d'autres, créant des envies justifiées. | Élevée, respecte les priorités des écoles et évite les envies justifiées, garantissant une répartition plus juste. |
| **Non-manipulabilité**| Faible, incite les élèves à falsifier les préférences pour obtenir de meilleures affectations. | Élevée, encourage les élèves à déclarer sincèrement leurs préférences sans crainte de désavantage. |
| **Stabilité**         | Faible, peut engendrer des appariements instables avec des envies justifiées. | Élevée, garantit la stabilité des appariements en évitant les désirs mutuels non satisfaits. |
| **Complexité**        | Faible, simple et rapide à exécuter, adapté aux environnements avec peu d'élèves et d'écoles. | Moyenne à élevée, nécessite des réévaluations constantes des propositions et des priorités, surtout dans des contextes plus vastes. |

**Figure 5 :** Comparaison des Algorithmes selon différents critères

### Efficacité

L'efficacité d'un algorithme d’affectation se mesure par sa capacité à maximiser la satisfaction des élèves en respectant leurs préférences. L'Algorithme de Boston excelle dans ce domaine en priorisant les premiers choix, ce qui se traduit par une haute satisfaction initiale des élèves. Cependant, cette focalisation sur les premiers choix peut compromettre l'équité et la stabilité des appariements.

En revanche, l'Algorithme d’Acceptation Différée adopte une approche plus équilibrée, assurant une répartition plus juste des places disponibles tout en maintenant une satisfaction élevée des préférences des élèves. Bien que cette approche puisse sacrifier une partie de l'efficacité pure, elle garantit une distribution plus équitable et stable.

### Équité

L'équité est un critère essentiel pour garantir une répartition juste des places disponibles. L'Algorithme de Boston, en se concentrant principalement sur les premiers choix des élèves, peut favoriser certains élèves au détriment d'autres, créant ainsi des envies justifiées où un élève pourrait préférer une autre école à celle qui lui est attribuée.

L'Algorithme d’Acceptation Différée, quant à lui, respecte strictement les priorités des écoles, assurant ainsi une répartition plus équitable des places. En évitant les envies justifiées, cet algorithme garantit que les élèves sont affectés de manière plus juste et transparente, renforçant la confiance dans le système d’affectation.

### Non-manipulabilité

La non-manipulabilité est cruciale pour l'intégrité d'un système d’affectation. Un algorithme non-manipulable incite les élèves à déclarer sincèrement leurs préférences, sans craindre d'être désavantagés par une falsification stratégique.

L'Algorithme de Boston est vulnérable à la manipulation, car les élèves peuvent être tentés de falsifier leurs préférences pour obtenir une meilleure affectation. Cette incitation à la manipulation peut compromettre l'équité et l'efficacité du système.

En revanche, l'Algorithme d’Acceptation Différée est conçu pour être non-manipulable, encourageant les élèves à déclarer sincèrement leurs préférences. Cette caractéristique est essentielle pour maintenir la confiance et l'intégrité du processus d’affectation.

### Stabilité

La stabilité des appariements est un autre critère fondamental. Un appariement stable signifie qu'il n'existe pas de paire élève-école qui préférerait être associée l'une à l'autre plutôt qu'avec leurs affectations actuelles.

L'Algorithme de Boston peut engendrer des appariements instables, où certains élèves pourraient préférer une autre école en fonction des priorités des écoles, créant ainsi des envies justifiées et compromettant la stabilité globale du système.

L'Algorithme d’Acceptation Différée, en revanche, garantit la stabilité des appariements en respectant les priorités des écoles et en assurant que chaque élève est affecté de manière optimale sans créer de désirs mutuels non satisfaits.

### Complexité

La complexité de l'algorithme est un facteur important à considérer, surtout dans des contextes avec un grand nombre d'élèves et d'écoles. L'Algorithme de Boston est relativement simple et rapide à exécuter, ce qui le rend adapté aux environnements avec un nombre limité d'élèves et d'écoles.

En revanche, l'Algorithme d’Acceptation Différée, bien qu'efficace et équitable, peut devenir plus complexe et nécessiter davantage de ressources computationnelles, surtout dans des contextes plus vastes avec de nombreuses propositions et réévaluations nécessaires.

---

## Propriétés des Algorithmes d’Affectation

### Efficacité

L'efficacité d'un algorithme d’affectation est mesurée par sa capacité à maximiser la satisfaction des élèves en respectant leurs préférences. L'Algorithme de Boston excelle dans la satisfaction des premiers choix, ce qui augmente la satisfaction initiale des élèves. Cependant, cette focalisation peut mener à une moindre satisfaction des choix subséquents et à une répartition moins équitable des places disponibles.

L'Algorithme d’Acceptation Différée adopte une approche plus équilibrée, en assurant une répartition plus juste des places tout en maintenant une satisfaction élevée des préférences des élèves. Cette méthode permet de combiner efficacité et équité, garantissant que la majorité des élèves obtiennent des placements qui respectent au mieux leurs préférences tout en respectant les priorités des écoles.

### Équité

L'équité est essentielle pour garantir une répartition juste et transparente des places disponibles. L'Algorithme de Boston, en se concentrant principalement sur les premiers choix des élèves, peut créer des déséquilibres où certains élèves sont favorisés au détriment d'autres, menant à des envies justifiées et à une perception d'injustice dans le système d’affectation.

L'Algorithme d’Acceptation Différée, en respectant strictement les priorités des écoles, assure une répartition plus équitable des places disponibles. Cet algorithme évite les envies justifiées en garantissant que les élèves sont affectés en fonction de leurs préférences et des priorités des écoles de manière équilibrée et transparente.

### Non-manipulabilité

La non-manipulabilité est un critère crucial pour la fiabilité et l'intégrité d'un système d’affectation. Un algorithme non-manipulable incite les élèves à déclarer sincèrement leurs préférences, sans craindre d'être désavantagés par une falsification stratégique.

L'Algorithme de Boston est vulnérable à la manipulation, car les élèves peuvent être tentés de falsifier leurs préférences pour obtenir une meilleure affectation, ce qui peut compromettre l'équité et l'efficacité du système.

En revanche, l'Algorithme d’Acceptation Différée est conçu pour être non-manipulable. Il encourage les élèves à déclarer sincèrement leurs préférences en assurant que l'algorithme fonctionne de manière transparente et équitable, sans avantage stratégique à la falsification des préférences.

### Stabilité

La stabilité des appariements est un autre critère fondamental dans l'évaluation des algorithmes d’affectation. Un appariement stable signifie qu'il n'existe pas de paire élève-école qui préférerait être associée l'une à l'autre plutôt qu'avec leurs affectations actuelles.

L'Algorithme de Boston peut engendrer des appariements instables, où certains élèves pourraient préférer une autre école en fonction des priorités des écoles, créant ainsi des envies justifiées et compromettant la stabilité globale du système.

L'Algorithme d’Acceptation Différée, en respectant strictement les priorités des écoles et en permettant aux élèves de proposer successivement à leurs choix suivants, garantit la stabilité des appariements. Cela signifie qu'aucune paire élève-école ne préférerait être associée l'une à l'autre plutôt qu'avec leurs affectations actuelles, assurant ainsi un système d’affectation stable et équitable.

### Complexité

La complexité des algorithmes d’affectation varie en fonction de leur conception et de leur application. L'Algorithme de Boston est relativement simple et rapide à exécuter, ce qui le rend adapté aux environnements avec un nombre limité d'élèves et d'écoles. Sa simplicité facilite également sa compréhension et son implémentation.

En revanche, l'Algorithme d’Acceptation Différée peut devenir plus complexe, surtout dans des contextes plus vastes avec de nombreux élèves et écoles. La nécessité de réévaluer les propositions et de gérer les priorités des écoles peut augmenter la complexité computationnelle de cet algorithme. Cependant, cette complexité est compensée par les avantages en termes de stabilité et d'équité qu'il offre.

---

## Conclusion

Les algorithmes d’affectation jouent un rôle essentiel dans la gestion équitable et efficace des placements scolaires. L'**Algorithme de Boston**, bien que populaire en raison de sa simplicité et de son efficacité à maximiser la satisfaction des premiers choix des élèves, présente des lacunes significatives en termes d'équité et de stabilité. Ces faiblesses peuvent entraîner des envies justifiées et inciter les élèves à manipuler leurs préférences, compromettant ainsi la fiabilité du système d’affectation.

En revanche, l'**Algorithme d’Acceptation Différée** offre une solution plus robuste et équilibrée, garantissant la stabilité des appariements et respectant rigoureusement les priorités des écoles. Bien qu'il puisse sacrifier une certaine efficacité en ne maximisant pas exclusivement les premiers choix, il assure une répartition plus équitable et moins manipulable des places disponibles, renforçant ainsi la confiance des participants dans le système d’affectation.

Pour des systèmes d’affectation scolaires justes et transparents, il est recommandé d’adopter l'Algorithme d’Acceptation Différée. Cet algorithme répond mieux aux exigences normatives de stabilité et d’équité, essentielles pour assurer une distribution équitable et efficace des ressources éducatives.

---

## Légende des Propriétés des Algorithmes


Les propriétés des algorithmes d’affectation sont résumées comme suit :

- **Efficacité** : Capacité de l'algorithme à maximiser la satisfaction des élèves en respectant leurs préférences.
- **Équité** : Capacité de l'algorithme à répartir les places disponibles de manière juste, en respectant les priorités des écoles.
- **Non-manipulabilité** : Capacité de l'algorithme à empêcher les élèves de manipuler leurs préférences pour obtenir de meilleures affectations.
- **Stabilité** : Capacité de l'algorithme à garantir que les appariements sont stables, sans aucune paire élève-école préférant être avec quelqu'un d'autre.
- **Limitations** : Contraintes inhérentes à l'algorithme, telles que l'incapacité à satisfaire simultanément tous les critères normatifs.

---

**Annexes**

1. **Préférences des Élèves et Priorités des Écoles**  
   *(Données complètes détaillées pour les 20 élèves et 5 écoles.)*

2. **Étapes Détaillées des Algorithmes**  
   *(Descriptions complètes des processus d'affectation étape par étape pour chaque algorithme.)*

---
