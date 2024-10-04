---
tags:
  - nosql
  - annales
---
### Partie 1 : Questions de cours (4 points)

1. **Vous expliquerez (en une vingtaine de lignes) l’importance de la scalabilité (ou passage à l’échelle) pour tout ce qui touche aux bases de données NoSQL. Vous illustrerez vos propos avec le système QNRW abordé pendant le cours consacré au système CouchDB (mais qui dépasse le cadre de ce seul système), et qui met en pratique les deux grands mécanismes sous-tendant la scalabilité.**

2. **Vous expliquerez brièvement (moins de dix lignes) en quoi consiste le postulat CAP.**

---

### Partie Neo4J (8 points)

Les données du projet CLIWOC (Climatological database for the World’s Oceans) sont exploitées. Ces données sont retraitées à partir de journaux de bord de voiliers marchands entre 1750 et 1850, et sont très précieuses pour la reconstitution des données météorologiques de l’époque. Elles donnent l’exemple de la météo, direction des vents et force du vent, etc. De manière simplifiée, des **bateaux** (voiliers) ont entrepris des voyages commerciaux à travers le monde. Pendant le voyage, l’équipage a pris le soin de relever chaque jour, ou presque, la **position géographique** du bateau, ainsi que d’autres observations météorologiques portant notamment sur les vents. 

#### 2.1 Création du graphe (3 points)

1. **Des ordres de création de nœuds et d’arêtes vous sont donnés dans la syntaxe Cypher. Vous construirez un graphe à partir des deux ordres de création donnés ci-dessous.**

```cypher
create (soolo:Bateau {nom:'SOOLO', pays:'PAYS BAS'})-[:ENTREPREND]->(PB_INDO:Voyage {depart:'SCHIEDAM', arrivee:'OOST INDE', capitaine:'T.B. TEUNISSEN'}), (soolo)-[:ENTREPREND]->(OOST_INDE:Voyage {depart:'OOST INDE', arrivee:'SCHIEDAM', capitaine:'T.B. TEUNISSEN'})
return *
```

```cypher
MATCH (P_IND:Voyage {depart:'SCHIEDAM', arrivee:'OOST INDE'}) 
CREATE (P_IND)-[:A_POSITION]->(SCHIEDAM_0:Position {longitude:-6.83, latitude:48.25, temperature:12.8, jour:'11-JAN-1844'}),
(P_IND)-[:A_POSITION]->(SCHIEDAM_1:Position {longitude:-4.6, latitude:47.75, temperature:12.7, jour:'12-JAN-1844'}),
(P_IND)-[:A_POSITION]->(SCHIEDAM_2:Position {longitude:-3.25, latitude:46.55, temperature:12.6, jour:'13-JAN-1844'}),
(P_IND)-[:A_POSITION]->(SCHIEDAM_3:Position {longitude:-1.75, latitude:45.25, temperature:12.5, jour:'14-JAN-1844'}),
(P_IND)-[:A_POSITION]->(SCHIEDAM_4:Position {longitude:-14.85, latitude:39.42, temperature:12.2, jour:'15-JAN-1844'})
RETURN *
```

Vous choisirez le nom du bateau, le point de départ (depart) des voyages et la date du jour des positions pour donner une étiquette aux nœuds visualisés et vous mentionnerez les labels de chacun des nœuds. Les autres propriétés des nœuds ne seront pas représentées. Vous indiquerez également le type des relations.

---

#### 2.2 Consultation du graphe (5 points)

2. **Vous écrirez en langage Cypher, la requête : "retourner les voyages du bateau SOOLO".**

Une requête de consultation en langage Cypher, vous est donnée qui porte sur le graphe qui vient d’être créé. Vous donnerez la signification de cette requête, ainsi que le résultat renvoyé par cette requête.

```cypher
MATCH (s:Bateau {nom:'SOOLO'})-[:ENTREPREND]->(v:Voyage)-[:A_POSITION]->(p:Position)
WHERE p.latitude > 40
RETURN s.nom, v.depart, v.arrivee, avg(p.temperature)
```

3. **Expliquez en quelques lignes ce qui rend difficile le passage d’un modèle à base de graphe attribué tel que celui de Neo4J à un modèle RDF.**

```bash
:POST rdf/cypher {"cypher": "MATCH g = (soolo:Bateau {nom:'SOOLO'})-[*2..3]->(p:Position {jour:'14-JAN-1844'}) RETURN g", "format": "N3"}
```

Le résultat de la requête au format N3 :
```plaintext
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix neovoc: <neo4j://vocabulary#> .
@prefix neoid: <neo4j://individuals#> .

neoid:327 a neovoc:Bateau;
 neovoc:ENTREPREND neoid:328;
 neovoc:nom "SOOLO";
 neovoc:pays "PAYS BAS".

neoid:328 a neovoc:Voyage;
 neovoc:A_POSITION neoid:350, neoid:351;
 neovoc:arrivee "OOST INDE";
 neovoc:capitaine "T.B. TEUNISSEN";
 neovoc:depart "SCHIEDAM".

neoid:350 a neovoc:Position;
 neovoc:SUIT_DE neoid:351;
 neovoc:jour "14-JAN-1844";
 neovoc:latitude 43.25681;
 neovoc:longitude -1.262621;
 neovoc:temperature 1.22E1.
```

--- 

Cela couvre les parties pertinentes du cours analysé sur Neo4J.
