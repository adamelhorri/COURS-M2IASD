---
tags:
  - TBD
  - cours
---


### 1. Algèbre relationnelle : Langage de requête

L'**algèbre relationnelle** est un langage utilisé pour gérer des bases de données. Elle permet d'effectuer des opérations sur des tables de données, un peu comme on le fait avec des feuilles de calcul. Ces opérations forment la base de **SQL**, le langage utilisé pour interagir avec des bases de données.

#### 1.1 Opérations principales de l’algèbre relationnelle
Voici les opérations de base que l'on utilise :
- **Sélection (σ)** : Choisit des lignes dans une table selon une condition.
- **Projection (π)** : Choisit certaines colonnes d’une table.
- **Jointure (▷◁)** : Combine plusieurs tables en unissant les lignes qui partagent des valeurs.
- **Renommage (δ)** : Change le nom des colonnes pour éviter les confusions.
- **Différence (−)**, **Union (∪)**, et **Intersection (∩)** : Opérations qui comparent plusieurs tables.

---

### 2. Détails des opérations de l'algèbre relationnelle

#### 2.1 Sélection (σ)
La **sélection** permet de filtrer des lignes selon une condition.

**Exemples :**
- Pour trouver toutes les lignes de bus :
  ```text
  σType="bus"(Lines)
  ```
- Pour trouver les connexions entre deux arrêts identiques :
  ```text
  σFrom=To(Connect)
  ```

#### 2.2 Projection (π)
La **projection** choisit certaines colonnes d’une table.

**Exemples :**
- Pour lister tous les types de lignes :
  ```text
  πType(Lines)
  ```
- Pour voir les arrêts adjacents sur la ligne 85 :
  ```text
  πFrom,To(σLine="85"(Connect))
  ```

#### 2.3 Jointure naturelle (▷◁)
La **jointure naturelle** combine deux tables en fusionnant les lignes avec des valeurs communes.

**Exemple :**
```text
Connect ▷◁ Lines
```
Cela combine les informations des deux tables où elles partagent une colonne.

#### 2.4 Renommage (δ)
Le **renommage** change les noms de colonnes pour éviter les conflits lors des jointures.

**Exemple :**
Pour joindre la table **Connect** avec **Stops**, on peut renommer l'attribut **From** en **SID** :
```text
πLine(σAccessible="true"(Stops ▷◁ δFrom→SID(Connect)))
```

#### 2.5 Opérations ensemblistes : Différence, Union, Intersection
Ces opérations comparent deux tables qui ont la même structure :
- **Différence (−)** : Trouve les lignes présentes dans la première table mais pas dans la seconde.
- **Union (∪)** : Trouve les lignes présentes dans au moins une des deux tables.
- **Intersection (∩)** : Trouve les lignes présentes dans les deux tables.

---

### 3. Tableaux constants dans les requêtes

Les **tables constantes** sont des ensembles de valeurs fixes utilisées dans les requêtes.

**Exemple :**
Pour trouver les arrêts proches de Helmholtzstr. (SID 42) :
```text
δTo→StopId(πTo(σFrom="42"(Connect))) ∪ {{StopId ↦ 42}}
```
Ici, la table constante représente l'arrêt Helmholtzstr.

---

### 4. Accessibilité dans un réseau de transport

L'accessibilité est essentielle pour interroger les réseaux de transport.

**Exemple :**
1. **Arrêts à Helmholtzstr.** :
   ```text
   R0 = {{From ↦ 42}}
   ```

2. **Arrêts adjacents à Helmholtzstr.** :
   ```text
   R1 = δTo→From(πTo(Connect ▷◁ R0))
   ```

3. **Arrêts à distance 2 de Helmholtzstr.** :
   ```text
   R2 = δTo→From(πTo(Connect ▷◁ R1))
   ```

On peut ensuite trouver tous les arrêts accessibles depuis Helmholtzstr. avec :
```text
R0 ∪ R1 ∪ R2
```

---

### 5. Exercices pratiques

Ces exercices montrent comment manipuler des bases de données avec l'algèbre relationnelle.

**Exemple 1 : Films et cinémas**

| **Films**           | **Title**          | **Director**   | **Actor**       |
|---------------------|--------------------|----------------|-----------------|
| The Imitation Game  | Tyldum             | Cumberbatch    |
| The Imitation Game  | Tyldum             | Knightley      |
| Internet's Own Boy  | Knappenberger      | Swartz         |
| Internet's Own Boy  | Knappenberger      | Lessig         |
| Dogma              | Smith              | Damon          |

**Exemples de requêtes :**
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

---

### 6. Identités de l'algèbre relationnelle

Certaines règles sont importantes pour comprendre les opérations :

1. **R ▷◁ S ≡ S ▷◁ R**
   - La jointure est commutative : l'ordre des tables n'a pas d'importance.
  
2. **R ▷◁ (S ▷◁ T) ≡ (R ▷◁ S) ▷◁ T**
   - La jointure est associative : on peut regrouper les jointures.

3. **πX(R ∩ S) ≡ πX(R) ∩ πX(S)**
   - La projection sur une intersection est équivalente à l'intersection des projections.

---

### 7. Conclusion et perspectives

Nous avons vu :
- Les différentes manières de représenter une base de données : en tables, faits, ou hypergraphes.
- Les opérations de l'algèbre relationnelle pour manipuler les données.
- Des exemples pratiques montrant comment utiliser ces opérations.

Les prochaines sections aborderont :
- Les **requêtes du premier ordre**,
- La **complexité des réponses** aux requêtes,
- L'**expressivité** des requêtes.

L'objectif est de comprendre comment améliorer et comparer différentes approches de requêtes dans les bases de données.

---

Si tu as besoin de plus d'explications ou d'exemples, n'hésite pas à demander !