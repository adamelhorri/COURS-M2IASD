---
tags:
  - nosql
---

```cypher
CREATE (sb:Etudiant:Femme {numINE: '20203344', nom: 'Bordet', prenom: 'Salome', dob: '1-apr-1997'})
RETURN sb;

MATCH (sb:Etudiant:Femme {numINE: '20203344'})
CREATE (mm:Etudiant:Femme {numINE: '20142345', nom: 'Martin', prenom: 'Marie', dob: '19-apr-1996'})
  -[:apprecie]-> (sb),
  (sb) -[:apprecie]-> (mm),
  (mm) -[:inscritDans {annee: 2021}]-> (f1:Formation {codeF: 'HAM1FE-300', nomF: 'ICo', niveau: 'M1', responsable: 'K. T.'}),
  (sb) -[:inscritDans {annee: 2021}]-> (f1),
  (mm) -[:inscritDans {annee: 2020}]-> (f1),
  (pm:Etudiant:Homme {numINE: '20161234', nom: 'Martin', prenom: 'Paul', dob: '20-aug-1995'})
  -[:apprecie]-> (mm),
  (pm) -[:inscritDans {annee: 2021}]-> (f2:Formation {codeF: 'HAM1GEO', nomF: 'Geomatique', niveau: 'M1', responsable: 'C. G.'})
RETURN *;

```

![[Pasted image 20241118110905.png]]

