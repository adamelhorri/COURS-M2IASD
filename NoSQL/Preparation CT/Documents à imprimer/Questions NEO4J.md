---
tags:
  - nosql
---
## 1. TAM réseau


```Cypher
CREATE 
    (s1:Station {nom: 'Occitanie', latitude: 43.63435811, longitude: 3.84863696}) 
        -[:STOP_DE]-> 
    (la1:Liaison {distance: 500}),
    (la1) -[:APPARTIENT_A]-> 
    (li1:Ligne {nom: 'Mosson-Odysseum', num: 1, type: 'tram', longueur: 15700, tempsTotal: 50}),
    
    (la2:Liaison {distance: 1000}) -[:APPARTIENT_A]-> (li1),
    
    (s2:Station {nom: 'Hopital Lapeyronie', latitude: 43.63166867, longitude: 3.85260055}) 
        -[:STOP_DE]-> 
    (la2),
    
    (s1) -[:STOP_DE]-> (la2),
    
    (s3:Station {nom: 'Chateau d O', latitude: 43.63131727, longitude: 3.84299936}) 
        -[:STOP_DE]-> 
    (la2),
    
    (s4:Station {nom: 'Univ. Sciences et Lettres', latitude: 43.63131727, longitude: 3.84299936}) 
        -[:STOP_DE]-> 
    (la3:Liaison {distance: 1200}),
    
    (s2) -[:STOP_DE]-> (la3),
    (la3) -[:APPARTIENT_A]-> (li1)
RETURN s1, s2, s3, s4, li1, la1, la2, la3;
MATCH (o:Station {nom: 'Occitanie'})
CREATE 
    (sp:Station {nom: 'Saint-Priest', latitude: 43.836699, longitude: 4.360054}) 
        -[:STOP_DE]-> 
    (l:Liaison {distance: 400}),
    (l) -[:APPARTIENT_A]-> 
    (li:Ligne {nom: 'Euromedecine-Pas du Loup', num: 6, type: 'bus'}),
    (o) -[:STOP_DE]-> (l);

```
todo

### 1.2. Donner sous-graphe

```Cypher

MATCH (o:Station {nom:'Occitanie'})
CREATE
	(sp:Station {nom:'Saint-Priest', latitude:43.836699, longitude:4.360054}) -[:STOP_DE]-> (l:Liaison {distance:400}),
	(l) -[:APPARTIENT_A]-> (li:Ligne {nom:'Euromedecine-Pas du Loup', num:6, type:'bus'}),
	(o) -[:STOP_DE]-> (l)
```

![[Pasted image 20241118131509.png]]

### 1.3. Donner résultat

```Cypher
MATCH (l:Ligne {type:'bus'}) <-[a:APPARTIENT_A] - () <-[st:STOP_DE]- (s:Station)
RETURN DISTINCT l.nom, s.nom
```

| l.nom                                  | s.nom                        |
| -------------------------------------- | ---------------------------- |
| ```<br>Euromedecine-Pas du Loup<br>``` | ```<br>"Occitanie" <br>```   |
| ```<br>Euromedecine-Pas du Loup<br>``` | ```<br>"Saint-Priest"<br>``` |

Récupère tous les noms de stations de bus et les noms des lignes de bus du réseau de la TAM.

### 1.4. Expliquer requête

```Cypher
MATCH (l1:Liaison) <-[:STOP_DE]- (s:Station) -[:STOP_DE]-> (l2:Liaison)
RETURN l1, s, l2
```
Cette requête recherche toutes les stations (`s:Station`) connectées à deux liaisons différentes (`l1:Liaison` et `l2:Liaison`) via la relation `[:STOP_DE]`. Elle retourne les deux liaisons et la station qui les relie.

![[Pasted image 20241118132340.png]]

### 1.5. Requête : nombre de stations situées sur la ligne Mosson-Odysseum

```Cypher
MATCH (s:Station)-[:STOP_DE]-(:Liaison)-[:APPARTIENT_A]-(l:Ligne {nom: "Mosson-Odysseum"})
RETURN count(DISTINCT s) AS nb_station_ligne_1;

```
![[Pasted image 20241118133248.png]]

### 1.6. Simplifier modèle :
Lien direct entre stations et lignes avec distance comme attribut du lien

## 2. Étudiant univ

### 2.1. Draw graph

```Cypher
// Création de l'étudiante Salomé Bordet
CREATE (sb:Etudiant:Femme {numINE:'20203344', nom:'Bordet', prenom:'Salome', dob:'1997-04-01'})
RETURN sb;

// Création des relations et des autres nœuds
MATCH (sb:Etudiant:Femme {numINE:'20203344'})
CREATE 
    // Étudiante Marie Martin
    (mm:Etudiant:Femme {numINE:'20142345', nom:'Martin', prenom:'Marie', dob:'1996-04-19'}) -[:apprecie]-> (sb),
    (sb) -[:apprecie]-> (mm),
    // Salomé et Marie inscrites dans la même formation
    (mm) -[:inscritDans {annee:2021}]-> 
        (f1:Formation {codeF:'HAM1FE-300', nomF:'ICo', niveau:'M1', responsable:'K. T.'}),
    (sb) -[:inscritDans {annee:2021}]-> (f1),
    (mm) -[:inscritDans {annee:2020}]-> (f1),
    // Étudiant Paul Martin
    (pm:Etudiant:Homme {numINE:'20161234', nom:'Martin', prenom:'Paul', dob:'1995-08-20'}) -[:apprecie]-> (mm),
    (pm) -[:inscritDans {annee:2021}]-> 
        (f2:Formation {codeF:'HAM1GEO', nomF:'Geomatique', niveau:'M1', responsable:'C. G.'})
RETURN *;
```

![[Pasted image 20241118134750.png]]

### 2.2. Écrire requête : retourner les nœuds d’étudiants que Paul et Salomé apprécient tous les deux

```Cypher
MATCH (paul:Etudiant:Homme {prenom: 'Paul'})-[:apprecie]->(e:Etudiant),

(salome:Etudiant:Femme {prenom: 'Salome'})-[:apprecie]->(e)

RETURN DISTINCT e AS etudiants_apprecies_communs;
```

![[Pasted image 20241118135304.png]]

### 2.3. Signification requête

```cypher
MATCH (f1:Formation) <-[id1:inscritDans]- (e1:Etudiant) -[a:apprecie]-> (e2:Etudiant)
      -[id2:inscritDans]-> (f1:Formation)
WHERE id1.annee = id2.annee
RETURN e1.prenom, e2.prenom, f1.nomF, id1.annee;
```
Identifier les étudiants inscrits dans la même formation, qui s'apprécient mutuellement, et pour lesquels l'inscription se fait la même année.

### 2.4. Requête Cypher et signification (Neosemantics)

```cypher
:POST /rdf/cypher { "cypher":"MATCH (c:Etudiant {prenom:'Marie'}) RETURN c", "format" : "N3"}
```

1. **`MATCH (c:Etudiant {prenom:'Marie'})`** :  
   - Cette commande cherche les nœuds ayant le label `Etudiant` et dont la propriété `prenom` est égale à `"Marie"`.

2. **`RETURN c`** :  
   - Retourne le nœud correspondant au résultat de la recherche, avec toutes ses propriétés.

3. **`"format": "N3"`** :  
   - Spécifie que le résultat doit être retourné au format RDF N3 (Notation3), utilisé pour représenter les données du graphe dans un format standardisé.

---

### 2.5. Résultat obtenu au format RDF N3

Le résultat montre un nœud correspondant à l'étudiante Marie Martin. Voici sa description en RDF N3 :

```cypher
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix neovoc: <neo4j://vocabulary#> .
@prefix neoind: <neo4j://individuals#> .

neoind:76 a neovoc:Etudiant, neovoc:Femme;
    neovoc:dob "19-apr-1996";
    neovoc:nom "Martin";
    neovoc:numINE "20142345";
    neovoc:prenom "Marie" .
```

#### 2.5.1. Interprétation du résultat :
- **`neoind:76`** : Représente un identifiant unique pour le nœud correspondant à l’étudiante.
- **`a neovoc:Etudiant, neovoc:Femme`** : Ce nœud est typé comme un étudiant (`Etudiant`) et appartient à la catégorie des femmes (`Femme`).
- **Propriétés du nœud** :
  - `neovoc:dob` : Date de naissance, `"19-apr-1996"`.
  - `neovoc:nom` : Nom, `"Martin"`.
  - `neovoc:numINE` : Numéro INE, `"20142345"`.
  - `neovoc:prenom` : Prénom, `"Marie"`

## 3. TAM 2

### 3.1. Dessiner graphe 

```Cypher
CREATE (s1:Station {nom:'Occitanie', latitude:43.63435811, longitude:3.84863696})
       -[:STOP_DE]-> (la1:Liaison {distance:500}),
       (la1) -[:APPARTIENT_A]-> (li1:Ligne {nom:'Mosson-Odysseum', num:1, type:'tram', longueur:15700, tempsTotal:50}),
       (la2:Liaison {distance:1000}) -[:APPARTIENT_A]-> (li1),
       (s2:Station {nom:'Hopital Lapeyronie', latitude:43.63166867, longitude:3.85260055}) -[:STOP_DE]-> (la2),
       (s1) -[:STOP_DE]-> (la2),
       (s3:Station {nom:'Chateau d O', latitude:43.63131727, longitude:3.84299936}) -[:STOP_DE]-> (la2),
       (s4:Station {nom:'Univ. Sciences et Lettres', latitude:43.63131727, longitude:3.84299936}) -[:STOP_DE]-> (la3:Liaison {distance:1200}),
       (s2) -[:STOP_DE]-> (la3),
       (la3) -[:APPARTIENT_A]-> (li1)
RETURN s1, s2, s3, s4, li1, la1, la2, la3;

MATCH (o:Station {nom:'Occitanie'})
CREATE (sp:Station {nom:'Saint-Priest', latitude:43.836699, longitude:4.360054})
       -[:STOP_DE]-> (l:Liaison {distance:400}),
       (l) -[:APPARTIENT_A]-> (li:Ligne {nom:'Euromedecine-Pas du Loup', num:6, type:'bus'}),
       (o) -[:STOP_DE]-> (l);
```
![[Pasted image 20241118154343.png]]

### 3.2. Expliquer requête

```Cypher
MATCH (l:Ligne {nom:'Mosson-Odysseum'}) <-[a:APPARTIENT_A]- () <-[st:STOP_DE]- (s:Station)
RETURN DISTINCT l.nom, s.nom;

```
--> Toutes les **stations desservies par la ligne "Mosson-Odysseum"**

![[Pasted image 20241118154620.png]]

### 3.3. Expliquer requête

```Cypher
MATCH (l:Liaison) <-[:STOP_DE]- (s:Station)
RETURN l, s;

```
La requête identifie toutes les **stations connectées aux liaisons** dans le graphe. Chaque liaison représente un segment de trajet, et chaque station identifiée est un arrêt desservi par ces liaisons.

![[Pasted image 20241118155139.png]]

### 3.4. Écrire : donner le nom des stations qui sont des stops pour le même tronçon (liaison de ligne)
- Retour : tableau 
```Cypher
MATCH (s1:Station)-[:STOP_DE]->(l:Liaison)<-[:STOP_DE]-(s2:Station)
RETURN DISTINCT s1.nom AS Station1, s2.nom AS Station2, l.distance AS DistanceLiaison;

```
![[Pasted image 20241118160013.png]]

- Retour : graphe :
```Cypher
MATCH (s1:Station)-[:STOP_DE]->(l:Liaison)<-[:STOP_DE]-(s2:Station)

RETURN s1, s2, l;
```
![[Pasted image 20241118160043.png]]

### 3.5. Simplifier le graphe 

#### 3.5.1. Fusionner les Liaisons avec les Relations

Au lieu de représenter explicitement les `Liaison` comme des nœuds séparés, elles peuvent être modélisées directement comme des relations entre les stations. Cela simplifie le graphe tout en conservant les informations sur les distances et les temps de trajet.

#### Nouveau Modèle :

- **Nœuds** :
    
    - `Station` : Les stations restent des nœuds avec leurs coordonnées et propriétés associées.
    - `Ligne` : Les lignes restent des nœuds pour contenir les propriétés globales comme le nom, le type, et le temps total.
- **Relations** :
    
    - `[:RELIE]` : Une relation directe entre deux stations pour représenter une liaison, avec des propriétés telles que `distance` et `temps`.
    - `[:FAIT_PARTIE_DE]` : Une relation entre chaque station et la ligne à laquelle elle appartient.

**Résultat**
```Cypher
CREATE (s1:Station {nom: 'Occitanie', latitude: 43.63435811, longitude: 3.84863696}),
       (s2:Station {nom: 'Hopital Lapeyronie', latitude: 43.63166867, longitude: 3.85260055}),
       (s3:Station {nom: 'Chateau d O', latitude: 43.63131727, longitude: 3.84299936}),
       (s4:Station {nom: 'Univ. Sciences et Lettres', latitude: 43.63131727, longitude: 3.84299936}),
       (sp:Station {nom: 'Saint-Priest', latitude: 43.836699, longitude: 4.360054});

CREATE (l1:Ligne {nom: 'Mosson-Odysseum', num: 1, type:'tram', longueur:15700, tempsTotal:50}),
       (l2:Ligne {nom: 'Euromedecine-Pas du Loup', num: 6, type:'bus'});
MATCH (s1:Station {nom: 'Occitanie'}), (s2:Station {nom: 'Hopital Lapeyronie'}),
      (s3:Station {nom: 'Chateau d O'}), (s4:Station {nom: 'Univ. Sciences et Lettres'}),
      (sp:Station {nom: 'Saint-Priest'}), (l1:Ligne {nom: 'Mosson-Odysseum'}),
      (l2:Ligne {nom: 'Euromedecine-Pas du Loup'})
CREATE 
       // Connexions pour la ligne "Mosson-Odysseum"
       (s1)-[:RELIE {distance: 500, temps: 5}]->(s2),
       (s2)-[:RELIE {distance: 1000, temps: 10}]->(s3),
       (s3)-[:RELIE {distance: 1200, temps: 15}]->(s4),
       
       // Connexion entre Occitanie et Saint-Priest
       (s1)-[:RELIE {distance: 400, temps: 7}]->(sp),
       
       // Associations des stations aux lignes
       (s1)-[:FAIT_PARTIE_DE]->(l1),
       (s2)-[:FAIT_PARTIE_DE]->(l1),
       (s3)-[:FAIT_PARTIE_DE]->(l1),
       (s4)-[:FAIT_PARTIE_DE]->(l1),
       (s1)-[:FAIT_PARTIE_DE]->(l2),
       (sp)-[:FAIT_PARTIE_DE]->(l2);
```
![[Pasted image 20241118160818.png]]

