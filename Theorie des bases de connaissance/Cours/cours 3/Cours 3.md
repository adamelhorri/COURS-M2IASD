---
tags:
  - TBD
  - cours
---
comme d'hab , une petite banalisation : [[Banalisation Cours 3]]
### 1. Algèbre relationnelle : Langage de requête

L'**algèbre relationnelle** est un langage formel utilisé pour manipuler des bases de données relationnelles. Il est basé sur un ensemble d'**opérations** qui prennent des tables en entrée et produisent de nouvelles tables en sortie. Ce langage constitue la base théorique de **SQL**.

#### 1 Opérations principales de l’algèbre relationnelle
Voici les principales opérations de l’algèbre relationnelle :
- **Sélection (σ)** : filtre les lignes d’une table en fonction d’une condition.
- **Projection (π)** : sélectionne des colonnes spécifiques d’une table.
- **Jointure (▷◁)** : combine les tables en fusionnant les lignes ayant des valeurs communes sur certains attributs.
- **Renommage (δ)** : renomme les attributs d'une table pour éviter les conflits lors des jointures.
- **Différence (−)**, **Union (∪)**, et **Intersection (∩)** : opérations ensemblistes permettant de manipuler plusieurs tables ayant le même schéma.

---

### 2. Détails des opérations de l'algèbre relationnelle

####  1 Sélection (σ)
L'opérateur de **sélection** permet de filtrer les lignes d'une table en fonction d'une condition sur ses attributs.

Exemples :
- **Trouver toutes les lignes de bus** :
  ```text
  σType="bus"(Lines)
  ```
- **Trouver toutes les connexions commençant et finissant au même arrêt** :
  ```text
  σFrom=To(Connect)
  ```

Définition formelle de l'opérateur de sélection :
- **σn=m** : sélectionne les lignes où l’attribut **n** a la même valeur que **m** (qui peut être une constante ou un autre attribut).

####  2 Projection (π)
L'opérateur de **projection** permet de sélectionner certaines colonnes d’une table, en éliminant les autres.

Exemples :
- **Trouver tous les types de lignes** :
  ```text
  πType(Lines)
  ```
- **Trouver toutes les paires d’arrêts adjacents sur la ligne 85** :
  ```text
  πFrom,To(σLine="85"(Connect))
  ```

Définition formelle de l'opérateur de projection :
- **πa1,...,an** : renvoie une nouvelle table contenant uniquement les attributs **a1,...,an** de la table d’origine.

####  3 Jointure naturelle (▷◁)
La **jointure naturelle** combine deux tables en fusionnant les lignes ayant des valeurs communes sur certains attributs.

Exemple :
```text
Connect ▷◁ Lines
```
Résultat : combinaison des lignes de **Connect** et **Lines** ayant une colonne **Line** en commun.

Définition formelle de la jointure naturelle :
- Si **R[U]** et **S[V]** sont deux tables, alors :
  ```text
  RI ▷◁ SI = {f : U ∪ V → dom | fU ∈ RI et fV ∈ SI}
  ```

####  4 Renommage (δ)
L'opérateur de **renommage** permet de changer le nom des attributs d'une table pour éviter les conflits lors des jointures.

Exemple :
Si nous voulons associer la table **Connect** avec **Stops** sur l'attribut **SID**, il est nécessaire de renommer l'attribut **From** de **Connect** en **SID** :
```text
πLine(σAccessible="true"(Stops ▷◁ δFrom→SID(Connect)))
```

Définition formelle de l'opérateur de renommage :
- **δa1,...,an→b1,...,bn** : renomme les attributs **a1,...,an** en **b1,...,bn**.

####  5 Opérations ensemblistes : Différence, Union, Intersection
Ces opérations permettent de comparer deux tables ayant le même schéma :
- **Différence (−)** : sélectionne les lignes présentes dans la première table mais pas dans la seconde.
  - Ex. : **Trouver les arrêts où la ligne 3 passe mais pas la ligne 8**.
- **Union (∪)** : sélectionne les lignes présentes dans l'une ou l'autre des deux tables.
  - Ex. : **Trouver les arrêts où passe la ligne 3 ou la ligne 8**.
- **Intersection (∩)** : sélectionne les lignes présentes dans les deux tables.
  - Ex. : **Trouver les arrêts où passent à la fois la ligne 3 et la ligne 8**.

### 3. Tableaux constants dans les requêtes

Il est parfois utile d'inclure des **tables constantes** dans les requêtes. Ces tables contiennent des valeurs fixes qui peuvent être utilisées dans les opérations de requêtes. L'idée est de définir des ensembles de données qui ne dépendent pas de la base de données sous-jacente mais servent d'éléments de référence dans une requête.

#### Exemple d'utilisation de tableaux constants :
**"Trouver tous les arrêts proches de Helmholtzstr. (SID 42), y compris Helmholtzstr."**  
La requête suivante utilise une table constante pour représenter l'arrêt Helmholtzstr. :
```text
δTo→StopId(πTo(σFrom="42"(Connect))) ∪ {{StopId ↦ 42}}
```
Ici, la table constante contient l'association **StopId ↦ 42**.

On peut généraliser cette approche à des **tables constantes** avec plusieurs colonnes ou plusieurs lignes.

---

### 4. Accessibilité dans un réseau de transport

L'accessibilité est un concept clé dans les bases de données relationnelles, particulièrement utile pour les requêtes sur des réseaux de transport ou des graphes de connexions.

#### Exemple : Accessibilité depuis Helmholtzstr.
1. **Arrêts à Helmholtzstr.** :
   ```text
   R0 = {{From ↦ 42}}
   ```

2. **Arrêts adjacents à Helmholtzstr.** (arrêts directement connectés) :
   ```text
   R1 = δTo→From(πTo(Connect ▷◁ R0))
   ```

3. **Arrêts à distance 2 de Helmholtzstr.** :
   ```text
   R2 = δTo→From(πTo(Connect ▷◁ R1))
   ```

On peut ensuite trouver tous les arrêts accessibles depuis Helmholtzstr. avec un **ticket à courte distance** :
```text
R0 ∪ R1 ∪ R2 ∪ R3 ∪ R4
```

Pour les arrêts accessibles à n'importe quelle distance, cela nécessite des algorithmes d'accessibilité plus avancés, qui seront discutés dans les prochains cours.

---

### 5. Exercices pratiques

Ces exercices illustrent comment manipuler des bases de données à l’aide des opérations de l'algèbre relationnelle.

#### Exemple 1 : Films et cinémas
Considérons les tables **Films**, **Venues** (cinémas), et **Program** (programme des films).

| **Films**           | **Title**          | **Director**   | **Actor**       |
|---------------------|--------------------|----------------|-----------------|
| The Imitation Game  | Tyldum             | Cumberbatch    |
| The Imitation Game  | Tyldum             | Knightley      |
| Internet's Own Boy  | Knappenberger      | Swartz         |
| Internet's Own Boy  | Knappenberger      | Lessig         |
| Dogma              | Smith              | Damon          |

| **Venues**          | **Cinema**         | **Address**     | **Phone**       |
|---------------------|--------------------|-----------------|-----------------|
| UFA                 | St. Peter St. 24   | 4825825         |
| Diagon              | King St. 55        | 8032185         |

| **Program**         | **Cinema**         | **Title**       | **Time**        |
|---------------------|--------------------|-----------------|-----------------|
| Diagon              | The Imitation Game | 19:30           |
| Diagon              | Dogma              | 20:45           |
| UFA                 | The Imitation Game | 22:45           |

Exemples de requêtes algébriques :
1. **Quel est le réalisateur de "The Imitation Game" ?**
   ```text
   πDirector(σTitle="The Imitation Game"(Films))
   ```
   
2. **Dans quels cinémas joue "The Imitation Game" ?**
   ```text
   πCinema(σTitle="The Imitation Game"(Program))
   ```

3. **Adresse et numéro de téléphone de "UFA" ?**
   ```text
   πAddress,Phone(σCinema="UFA"(Venues))
   ```

#### Autres exemples de requêtes complexes
- **Quels sont les acteurs qui ne jouent pas dans un film réalisé par Smith ?**
  ```text
  πActor(Films) − πActor(σDirector="Smith"(Films))
  ```

- **Trouver les réalisateurs qui ont également joué dans leurs propres films :**
  ```text
  πDirector(σActor=Director(Films))
  ```

Ces exercices montrent la flexibilité de l'algèbre relationnelle pour exprimer des requêtes complexes sur des bases de données de grande taille.

---

### 6. Identités de l'algèbre relationnelle

Certaines identités sont fondamentales pour comprendre les interactions entre les opérations algébriques et les optimisations potentielles.

#### Exemples d'identités algébriques :
1. **R ▷◁ S ≡ S ▷◁ R**
   - La jointure naturelle est commutative : l'ordre des tables n'a pas d'importance.
  
2. **R ▷◁ (S ▷◁ T) ≡ (R ▷◁ S) ▷◁ T**
   - La jointure est associative : on peut regrouper les jointures sans changer le résultat.

3. **πX(R ∩ S) ≡ πX(R) ∩ πX(S)**
   - La projection sur une intersection est équivalente à l'intersection des projections.

Ces identités sont utilisées pour optimiser les requêtes en réduisant le nombre d’opérations nécessaires ou en réorganisant les calculs pour améliorer les performances.

---

### 7. Conclusion et perspectives

Dans cette partie, nous avons couvert les éléments suivants :
- Les **différentes perspectives** pour représenter une base de données : en tant que tables, faits, ou hypergraphes.
- Les **opérations de l'algèbre relationnelle** qui permettent de manipuler les données.
- Des exemples concrets illustrant l'utilisation de ces opérations dans des scénarios pratiques.
  
Les prochaines sections exploreront :
- Les **requêtes du premier ordre (First-Order Queries)**,
- La **complexité des réponses aux requêtes**,
- L'**expressivité** des requêtes en algèbre relationnelle par rapport aux requêtes du premier ordre.

L'objectif est de comprendre comment optimiser et comparer différentes approches de requêtes dans les bases de données.