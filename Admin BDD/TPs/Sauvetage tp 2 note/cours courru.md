Ci-dessous se trouve une réécriture structurée et concise, sans perte d’information, du contenu fourni. Les références et figures sont mentionnées comme dans l’original. On évite le “blabla” superflu en se concentrant sur l’essentiel et la structure logique du cours.

---

# Introduction à l’Optimisation de Requêtes (HAI901I)

## Définitions clés

- **Optimisation** : Capacité à adapter l’architecture d’un système aux besoins des applications.
- **Performance** : Qualité d’un système à répondre aux exigences des usagers, en tenant compte des demandes futures, des délais, du nombre d’usagers/transactions, de la volumétrie des données, etc.

## Ressources cibles

- Processeurs
- Mémoires caches
- Disques (mémoires secondaires)
- Processus (avant et arrière-plans)
- Réseau et bande passante (transit données serveur-client)

## Éléments à considérer dans un SGBDR

- Temps d’accès aux enregistrements physiques
- Méthodes d’accès aux données (caches, index)
- Clusters (jointures physiques sur segments spécifiques)
- Organisation physique (fichiers, taille de bloc, etc.)
- Organisation logique (tablespaces, extensions, etc.)
- Optimisation des requêtes

## Paramètres associés à une requête

- Chemins d’accès aux données
- Opérations nécessaires (opérateurs sous-jacents)
- Enchaînement des opérations (plan d’exécution)

---

# Décortiquer une Requête

1. Requête déclarative initiale (sans ordre d’opérations).
2. Analyse, simplification, réécriture : obtention d’un arbre algébrique.
3. Transformation et choix : plan d’exécution physique (procédural).

**Plan d’exécution** : Arbre de nœuds (opérateurs physiques) et liens (flux de données). Plusieurs plans équivalents peuvent exister.

## Principaux opérateurs physiques

- Intersection, concaténation (union)
- Filter (sélection), projection
- Jointure : nested loop, sort/merge, hash join

## Retour vers l’algèbre relationnelle

- Opérateurs algébriques : sélection, projection, jointure, union, intersection, différence, etc.
- Propriétés : commutativité, associativité, distributivité
- Exemples d’équivalences :
    - Jointure commutative : Dept ⋊⋉ Emp = Emp ⋊⋉ Dept
    - Jointure associative : (Dept ⋊⋉ Emp) ⋊⋉ Fonction = Dept ⋊⋉ (Emp ⋊⋉ Fonction)

## Exemples d’arbres algébriques équivalents

Exemple :  
Donner le num. et nom des employés travaillant dans un département localisé à Montpellier.

```sql
SELECT numE, nomE
FROM Dept D JOIN Emp E ON D.n_dept = E.n_dept
WHERE localisation = 'Montpellier';
```

- Premier plan : sélection après la jointure.
- Deuxième plan (amélioré) : filtrer le département dès que possible.
- Troisième plan : changer l’ordre, mais parfois moins performant si on commence par la relation la plus volumineuse.

## Formes déclaratives équivalentes

La même requête peut s’écrire avec une semi-jointure, un EXISTS, etc. Le but est de montrer qu’il existe plusieurs expressions équivalentes fournissant le même résultat.

---

# Optimiseur Oracle

## Rôles de l’optimiseur

- Rechercher des expressions équivalentes via réécritures
- Évaluer ces expressions et choisir la meilleure
- Choix basé sur différents critères (débit, temps de réponse, etc.)

## Orienter l’optimiseur

- Objectifs : all_rows (défaut, privilégie le débit), first_rows_X (privilégier temps de réponse pour X premiers tuples)
    
- Exemples :
    
    ```sql
    ALTER SESSION SET optimizer_mode=FIRST_ROWS_10;
    ```
    
- Optimisation basée sur le coût (CPU/I/O) ou sur la performance (temps écoulé).
    

## Critères d’évaluation

- Schéma logique : tables, attributs, contraintes
- Schéma physique : index, taille bloc, chemins d’accès
- Algorithmes disponibles pour chaque opérateur
- Architecture système (parallélisme)

## Statistiques collectées

- Par table : cardinalité, taille moyenne des tuples, nombre d’enregistrements, blocage (nb blocs)
- Par opérateur : coût d’exécution des algorithmes
- Par attribut : type, distribution, sélectivité (density)
- Par index : type, densité, présence en cache

**Sélectivité** : entre 0 et 1, plus proche de 0 ⇒ plus sélectif. Dans `user_tab_columns`, l’attribut `density` est utilisé si pas d’histogrammes précis.

```sql
SELECT density 
FROM user_tab_columns
WHERE table_name='COMMUNE' 
AND column_name='NOMCOMMAJ';
```

**Rafraîchir les stats** :

```sql
EXEC dbms_utility.analyze_schema(user,'COMPUTE');
```

## Optimiseur et réutilisation de requêtes

- Hard parse / Soft parse : si requête déjà analysée, éviter un nouveau calcul du plan.

---

# Outil Explain Plan

- Estimer la performance d’un plan :
    
    ```sql
    EXPLAIN PLAN FOR SELECT * FROM emp WHERE num=33000;
    SELECT * FROM TABLE(dbms_xplan.display());
    ```
    
- Influencer le plan avec des hints :
    
    ```sql
    EXPLAIN PLAN FOR SELECT /*+ full(f) */ * FROM fonction f WHERE nom_f LIKE 'd%';
    ```
    

## Chemins d’accès aux données

- Différentes méthodes : full scan, index scan, etc. (cf. figure)

## Jointure et opérateurs physiques

- Mérites comparés du nested loop, sort/merge, hash join (cf. figure)

---

# Exemples

## Premier exemple

- Requête de sélection sur table sans index : full table scan, coût essentiellement I/O.

## Second exemple

- Même requête avec index sur num : utilisation de l’index, coût réduit.

## Troisième exemple

- Jointure sans index : hash join, full scan des deux tables, temps plus long.

## Sous forme algébrique

- Montrer l’arbre algébrique avec annotations sur la cardinalité, le coût, etc.

## Quatrième, Cinquième, Sixième exemples

- Ajouter index sur n_dept du Dept, puis sur n_dept de Emp, voir l’évolution du plan (hash join, sort/merge), et l’impact sur les coûts.
- Comparer différents opérateurs de jointure selon la présence d’index.

## Septième exemple

- Requête sans exploitation d’un index pourtant disponible (par exemple si l’optimiseur juge un full scan plus rentable).

---

# Obtenir le Plan d’Exécution après exécution

- Utiliser v$sql pour retrouver SQL_ID et SQL_HASH_VALUE
- dbms_xplan.display_cursor pour voir le plan réellement exploité
- Format ALLSTATS LAST pour avoir les vraies stats d’exécution.

```sql
SELECT /*+ GATHER_PLAN_STATISTICS */ codeinsee, nomcommaj
FROM commune WHERE codeinsee LIKE '34%';

SELECT * FROM TABLE(DBMS_XPLAN.display_cursor(format=>'ALLSTATS LAST'));
```

## Autotrace

- Outil simple pour obtenir le plan d’exécution et les stats d’exécution dans SQL*Plus.

## Outils externes

- SQL Developer, extensions pour VSCodium, SQL Monitor, etc., fournissent des interfaces visuelles pour analyser les plans d’exécution.

---

# Conclusion

- L’optimisation de requêtes repose sur la réécriture, l’estimation de coûts, la sélection du meilleur plan d’exécution.
- L’optimiseur s’appuie sur des statistiques, des propriétés algébriques et des choix d’objectifs (débit, temps de réponse).
- Des outils (EXPLAIN PLAN, DBMS_XPLAN, AUTOTRACE) et des hints permettent d’évaluer et d’orienter le plan d’exécution.
- L’organisation physique, la présence d’index, la distribution des données influent considérablement sur le plan choisi.

---

Fin du document.