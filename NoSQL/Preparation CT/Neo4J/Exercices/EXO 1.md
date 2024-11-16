---
tags:
  - nosql
---

# Examen Neo4J : Relations professionnelles dans une entreprise internationale

Dans cette entreprise, les employés travaillent dans divers départements, et certains sont sous la supervision d'autres. Par ailleurs, ils peuvent collaborer sur des projets communs, parfois avec des collègues d'autres départements.

## Partie Neo4J (8 points)

#### 2.1 Création du graphe

- [ ] **Question 1**  
> [!question]- Utilisez la syntaxe Cypher pour créer les nœuds et arêtes ci-dessous. Vous construirez un graphe en suivant les ordres de création donnés ci-après :

```cypher
CREATE (jp:Employe:Homme {idEmploye:'EMP123', nom:'Dupont', prenom:'Jean-Pierre', dateNaissance:'10-juil-1985'})
RETURN jp;
MATCH (jp:Employe:Homme {idEmploye:'EMP123'})
CREATE (ml:Employe:Femme {idEmploye:'EMP234', nom:'Leroy', prenom:'Marie-Laure', dateNaissance:'15-sept-1988'})
-[:travailleAvec]-> (jp),
(jp) -[:travailleAvec]-> (ml),
(ml) -[:affecteA {annee:2023}]-> (d1:Departement {codeDept:'R&D', nomDept:'Recherche et Développement', responsable:'P. Roche'}),
(jp) -[:affecteA {annee:2022}]-> (d1), 
(jp) -[:supervise {depuis:2021}]-> (p1:Projet {codeProjet:'PROJ1', nomProjet:'Innovation AI'}),
(jp) -[:supervise {depuis:2022}]-> (ml),
(fb:Employe:Homme {idEmploye:'EMP345', nom:'Bastien', prenom:'François', dateNaissance:'21-mars-1982'})
-[:affecteA {annee:2023}]-> (d2:Departement {codeDept:'HR', nomDept:'Ressources Humaines', responsable:'L. Moreau'})
RETURN *;
```

> [!answer]- **Correction :**
> Les nœuds `jp`, `ml`, et `fb` représentent des employés avec leurs relations professionnelles et départements d’affectation. Les labels sont `Employe`, `Homme` et `Femme`, et les types de relations sont `travailleAvec`, `affecteA`, et `supervise`.

#### 2.2 Consultation du graphe

- [ ] **Question 2**  
> [!question]- Rédigez en langage Cypher la requête suivante : **« Retourner les nœuds d’employés que Jean-Pierre et Marie-Laure supervisent tous les deux »**.

> [!answer]- **Correction :**
> ```cypher
> MATCH (jp:Employe {prenom: 'Jean-Pierre'})-[:supervise]->(e:Employe)<-[:supervise]-(ml:Employe {prenom: 'Marie-Laure'})
> RETURN e
> ```
> Cette requête recherche les employés supervisés à la fois par Jean-Pierre et Marie-Laure, en utilisant `supervise` pour connecter `jp` et `ml`.

- [ ] **Question 3**  
> [!question]- Une requête de consultation en langage Cypher vous est donnée ci-dessous, qui concerne le graphe que vous venez de créer. Donnez la signification de cette requête, ainsi que le résultat renvoyé par celle-ci :
> ```cypher
> MATCH (d1:Departement) <-[aff1:affecteA]- (e1:Employe) -[coll:travailleAvec]-> (e2:Employe) 
> -[aff2:affecteA]-> (d1:Departement)
> WHERE aff1.annee = aff2.annee
> RETURN e1.prenom, e2.prenom, d1.nomDept, aff1.annee;
> ```

> [!answer]- **Correction :**
> Cette requête trouve les paires d'employés `e1` et `e2` qui travaillent ensemble (`travailleAvec`) et sont affectés au même département `d1` la même année. Elle retourne les prénoms des deux employés, le nom du département et l’année.

- [ ] **Question 4**  
> [!question]- Une nouvelle requête Cypher vous est fournie (toujours sur le graphe créé). Donnez la signification de cette requête et expliquez le résultat obtenu, en illustrant éventuellement le graphe associé :
> ```cypher
> :POST /rdf/cypher { "cypher":"MATCH (e:Employe {prenom:'Marie-Laure'}) RETURN e" , "format" : "N3"}
> @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
> @prefix neovoc: <neo4j://vocabulary#> .
> @prefix neoind: <neo4j://individuals#> .
> neoind:152 a neovoc:Employe, neovoc:Femme;
> neovoc:dateNaissance "15-sept-1988";
> neovoc:nom "Leroy";
> neovoc:idEmploye "EMP234";
> neovoc:prenom "Marie-Laure" .
> ```

> [!answer]- **Correction :**
> Cette requête utilise l’API REST pour récupérer le nœud représentant `Marie-Laure` au format RDF. La sortie inclut ses propriétés : `prenom`, `nom`, `idEmploye`, `dateNaissance`, et ses labels `Employe` et `Femme`.
