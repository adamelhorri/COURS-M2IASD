---
tags:
  - progmob
  - tps
---

Voici comment réécrire l'énoncé et la résolution en respectant la syntaxe d'Obsidian pour afficher correctement les équations en LaTeX avec MathJax :

### Énoncé et résolution pour Obsidian

#### Énoncé

**Exercice 2.5**

Montrer que, dans le schéma de la figure 2, la résistance  R  entre les points  A  et  C  est telle que :
 
$R = R_1 + R_2$
 

**Exercice 2.6**

Montrer que, dans le schéma de la figure 2, la tension entre les points  A  et  B  est telle que :
 
$U_{A \to B} = U \times \frac{R_1}{R_1 + R_2}$
 
$et que la tension entre les points  B  et  C  est telle que :$
 
$U_{B \to C} = U \times \frac{R_2}{R_1 + R_2}$
 

**Exercice 2.7**

Toujours sur la base du schéma de la figure 2, étant donné un générateur de 9V, calculer des valeurs de résistance pour  R_1  et  R_2  de sorte à obtenir une tension de sortie de 5V ±1% entre les points  B  et  C , sachant que nous disposons de résistances ayant des valeurs de 560Ω, 1kΩ, 10kΩ et 47kΩ.

---

#### Résolution

**Exercice 2.5**

Dans un circuit en série, la résistance totale  R  est la somme des résistances individuelles. Comme  R_1  et  R_2  sont en série, la résistance entre les points  A  et  C  est :
 
$R = R_1 + R_2$
 

**Exercice 2.6**

La loi des diviseurs de tension indique que la tension aux bornes d'une résistance dans un circuit en série est proportionnelle à la valeur de cette résistance par rapport à la somme totale des résistances.

1. $Pour   U_{A \to B}  , la tension entre  A  et  B  est :$
    
   $U_{A \to B} = U \times \frac{R_1}{R_1 + R_2}$
    

2. $Pour   U_{B \to C}  , la tension entre  B  et  C  est :$
    
   $U_{B \to C} = U \times \frac{R_2}{R_1 + R_2}$
    

**Exercice 2.7**

Pour obtenir une tension de 5V entre  B  et  C  avec une tension totale de 9V, nous utilisons la formule suivante :
 
$U_{B \to C} = U \times \frac{R_2}{R_1 + R_2}$
 

$Substituons  U = 9V  et  U_{B \to C} = 5V  :$
 
$5 = 9 \times \frac{R_2}{R_1 + R_2}$
 
$Ce qui donne :$
 
$\frac{R_2}{R_1 + R_2} = \frac{5}{9}$
 

$Ainsi,  R_2  est lié à  R_1  par la relation :$
 
$R_2 = \frac{5}{4} R_1$
 

En utilisant les valeurs disponibles (560Ω, 1kΩ, 10kΩ, 47kΩ), choisissez les résistances les plus proches pour approximer la tension souhaitée.
