---
tags:
  - nosql
---

# Examen Neo4J : Contexte estudiantin

Le contexte estudiantin est exploité. Des étudiants sont inscrits annuellement dans des formations universitaires. Ces étudiants nouent par ailleurs des relations d’amitié avec d’autres étudiants (inscrits ou non dans la même formation).

## Partie Neo4J (8 points)

#### 2.1 Création du graphe

- [ ] **Question 1**  
> [!question]- Des ordres de création de nœuds et d’arêtes vous sont donnés dans la syntaxe Cypher. Vous construirez un graphe à partir des deux ordres de création donnés ci-dessous :

```cypher
CREATE (sb:Etudiant:Femme {numINE:'20203344', nom:'Bordet', prenom:'Salome', dob:'1-apr-1997'})
RETURN sb;
MATCH (sb:Etudiant:Femme {numINE:'20203344'})
CREATE (mm:Etudiant:Femme {numINE:'20142345', nom:'Martin', prenom:'Marie', dob:'19-apr-1996'})
-[:apprecie]-> (sb),
(sb) -[:apprecie]-> (mm),
(mm) -[:inscritDans {annee:2021}]-> (f1:Formation {codeF:'HAM1FE-300', nomF:'ICo', niveau:'M1', responsable:'K. T.'}),
(sb) -[:inscritDans {annee:2021}]-> (f1),
(mm) -[:inscritDans {annee:2020}]-> (f1),
(pm:Etudiant:Homme {numINE:'20161234', nom:'Martin', prenom:'Paul', dob:'20-aug-1995'})
-[:apprecie]-> (mm),
(pm) -[:inscritDans {annee:2021}]-> (f2:Formation {codeF:'HAM1GEO', nomF:'Geomatique', niveau:'M1', responsable:'C. G.'})
RETURN *;
```

> [!answer]- **Correction :**
> Les nœuds `sb`, `mm`, et `pm` représentent les étudiants avec leurs inscriptions dans les formations `f1` et `f2`. Les labels sont `Etudiant`, `Femme`, `Homme`, et `Formation`, et les types de relations sont `apprecie` et `inscritDans`.

#### 2.2 Consultation du graphe

- [ ] **Question 2**  
> [!question]- Écrivez en langage Cypher la requête suivante : **« Retourner les nœuds d’étudiants que Paul et Salome apprécient tous les deux »**.

> [!answer]- **Correction :**
> ```cypher
> MATCH (paul:Etudiant {prenom: 'Paul'})-[:apprecie]->(e:Etudiant)<-[:apprecie]-(salome:Etudiant {prenom: 'Salome'})
> RETURN e
> ```
> Cette requête retourne les étudiants appréciés à la fois par Paul et Salome.

- [ ] **Question 3**  
> [!question]- Une requête de consultation en langage Cypher vous est donnée ci-dessous, qui concerne le graphe que vous venez de créer. Donnez la signification de cette requête, ainsi que le résultat renvoyé par celle-ci :
> ```cypher
> MATCH (f1:Formation) <-[id1:inscritDans]- (e1:Etudiant) -[a:apprecie]-> (e2:Etudiant)
> -[id2:inscritDans]-> (f1:Formation)
> WHERE id1.annee = id2.annee
> RETURN e1.prenom, e2.prenom, f1.nomF, id1.annee;
> ```

> [!answer]- **Correction :**
> Cette requête recherche les paires d’étudiants `e1` et `e2` qui sont inscrits dans la même formation `f1` la même année et s’apprécient mutuellement. Elle retourne les prénoms des deux étudiants, le nom de la formation et l’année.

- [ ] **Question 4**  
> [!question]- Une nouvelle requête Cypher vous est donnée (toujours sur le graphe créé). Donnez la signification de cette requête et expliquez le résultat obtenu, en illustrant éventuellement le graphe associé :
> ```cypher
> :POST /rdf/cypher { "cypher":"MATCH (c:Etudiant {prenom:'Marie'}) RETURN c" , "format" : "N3"}
> @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
> @prefix neovoc: <neo4j://vocabulary#> .
> @prefix neoind: <neo4j://individuals#> .
> neoind:76 a neovoc:Etudiant, neovoc:Femme;
> neovoc:dob "19-apr-1996";
> neovoc:nom "Martin";
> neovoc:numINE "20142345";
> neovoc:prenom "Marie" .
> ```

> [!answer]- **Correction :**
> Cette requête utilise l’API REST pour récupérer le nœud représentant `Marie` au format RDF. La sortie RDF inclut les informations de `prenom`, `nom`, `numINE`, `dob`, et les labels associés `Etudiant` et `Femme`.
