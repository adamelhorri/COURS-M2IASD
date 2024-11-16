---
tags:
  - nosql
---

# Examen Neo4J : Relations familiales de Camille Claudel

Le contexte des liens familiaux dans la famille de Camille Claudel (artiste reconnue dans le domaine de la sculpture du XIXe siècle) et de son frère Paul Claudel (écrivain tout aussi connu) est exploité.

## Partie Neo4J (8 points)

#### 2.1 Création du graphe

- [ ] **Question 1**  
> [!question]- Des ordres de création de nœuds et d'arêtes vous sont donnés dans la syntaxe Cypher. Vous construirez un graphe à partir des deux ordres de création donnés ci-dessous :

```cypher
CREATE (louis:Personne:Homme {nom:'Claudel', prenom:'Louis', naissance:1826})
-[:MARIE_A {annee_mariage:1866}]-> (louise:Personne:Femme {nom:'Cerveaux', prenom:'Louise', naissance:1840}),
(louis) <-[:A_PARENT]- (camille:Personne:Femme {nom:'Claudel', prenom:'Camille', naissance:1864}),
(camille) -[:A_PARENT]-> (louise), 
(louis) <-[:A_PARENT]- (paul:Personne:Homme {nom:'Claudel', prenom:'Paul', naissance:1868})
RETURN louise, louis, camille, paul;

MATCH (paul:Homme {nom:'Claudel', prenom:'Paul', naissance:1868})
CREATE (marie:Personne:Femme {nom:'Perrin', prenom:'Marie'}) -[:MARIE_A {annee_mariage:1906}]-> (paul),
(louise:Personne:Femme {nom:'Vetch', prenom:'Louise', naissance:1905}) -[:A_PARENT]-> (paul),
(louise) -[:A_PARENT]-> (rosalie:Personne:Femme {nom:'Vetch', prenom:'Rosalie', naissance:1871}),
(henri:Personne:Homme {nom:'Vetch', prenom:'Henri', naissance:1898}) -[:A_PARENT]-> (rosalie),
(pierre:Personne:Homme {nom:'Claudel', prenom:'Pierre', naissance:1908}) -[:A_PARENT]-> (paul),
(pierre) -[:A_PARENT]-> (marie),
(reine:Personne:Femme {nom:'Paris', prenom:'Reine', naissance:1910}) -[:A_PARENT]-> (marie),
(reine) -[:A_PARENT]-> (paul),
(reinemarie:Personne:Femme {nom:'Paris', prenom:'Reine-Marie'}) -[:A_PARENT]-> (reine),
(renee:Personne:Femme {nom:'Nantet', prenom:'Renee', naissance:1917}) -[:A_PARENT]-> (marie),
(renee) -[:A_PARENT]-> (paul),
(victoire:Personne:Femme {nom:'Nantet', prenom:'Marie-Victoire'}) -[:A_PARENT]-> (renee)
RETURN *;
```

> [!answer]- **Correction :**
> Les nœuds créés représentent les membres de la famille Claudel avec leurs relations de parenté et de mariage. Les labels sont `Personne`, `Homme`, et `Femme`, et les types de relations sont `MARIE_A` et `A_PARENT`.

#### 2.2 Consultation du graphe

- [ ] **Question 2**  
> [!question]- Écrivez en langage Cypher la requête suivante : **« Donner les petits-enfants de Paul Claudel »**.

> [!answer]- **Correction :**
> ```cypher
> MATCH (paul:Personne {prenom:'Paul'})-[:A_PARENT]->(enfant:Personne)-[:A_PARENT]->(petit_enfant:Personne)
> RETURN petit_enfant
> ```
> Cette requête retourne tous les petits-enfants de Paul Claudel en suivant les relations `A_PARENT` sur deux niveaux de profondeur.

- [ ] **Question 3**  
> [!question]- Une requête de consultation en langage Cypher vous est donnée ci-dessous, qui concerne le graphe que vous venez de créer. Donnez la signification de cette requête, ainsi que le résultat renvoyé par celle-ci :
> ```cypher
> MATCH (:Femme {nom:'Claudel', prenom:'Camille'}) -[:A_PARENT]-> (pe:Personne) <-[:A_PARENT]- (fs:Personne) <-[:A_PARENT]- (nn:Personne)
> WHERE pe.prenom <> fs.prenom
> RETURN *
> ```

> [!answer]- **Correction :**
> Cette requête recherche les relations de parenté indirectes de Camille Claudel, où `pe` est un enfant de Camille, `fs` un petit-enfant, et `nn` un arrière-petit-enfant. La clause `WHERE` exclut les répétitions de prénoms identiques pour éviter les doublons.

- [ ] **Question 4**  
> [!question]- Une nouvelle requête Cypher vous est donnée (toujours sur le graphe créé). Donnez la signification de cette requête et expliquez le résultat obtenu, en illustrant éventuellement le graphe associé :
> ```cypher
> :POST /rdf/cypher { "cypher":"MATCH (:Personne {prenom:'Camille'}) RETURN *" , "format" : "N3"}
> @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
> @prefix neovoc: <neo4j://vocabulary#> .
> @prefix neoind: <neo4j://individuals#> .
> neoind:1069 a neovoc:Femme, neovoc:Personne;
> neovoc:naissance "1864"^^<http://www.w3.org/2001/XMLSchema#long>;
> neovoc:nom "Claudel";
> neovoc:prenom "Camille" .
> ```

> [!answer]- **Correction :**
> Cette requête utilise l’API REST pour récupérer le nœud représentant `Camille Claudel` au format RDF. La sortie RDF inclut les informations de `prenom`, `nom`, `naissance`, et les labels associés `Personne` et `Femme`.