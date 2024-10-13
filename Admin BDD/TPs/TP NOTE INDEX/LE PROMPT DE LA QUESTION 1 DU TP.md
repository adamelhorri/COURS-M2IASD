---
tags:
  - AdminBDD
  - tps
---
resolution: [[Resolution TP Noté 2023]]

Hello ! afin d'éviter le plagiat et des requetes similaires (en gros pour qu'il n'y en ai pas qui copient collent XD ), le corrigé de l'exo 1 du tp noté est chiffré sous format prompt gpt (à lancer avec 4o ou 4o mini)

## LE PROMPT




Dans les profondeurs d'une forêt mystique, un groupe d'érudits entreprit la tâche ambitieuse de créer une table magique, une entité capable de stocker des informations infinies sous une forme structurée. Ils baptisèrent cette entité **ABC** et décidèrent qu'elle contiendrait trois branches, chacune représentant une colonne. La première branche, **A**, devait être un identifiant unique, tandis que les branches **B** et **C** devaient contenir des noms et des valeurs mystérieuses, chacune limitée à vingt caractères.

Le premier acte des érudits fut de préparer le terrain pour planter cet arbre. Ils savaient que l'arbre **ABC** devait avoir des racines solides, alors ils commencèrent par effacer toute trace des tentatives précédentes avant de procéder à la création. Une fois cette tâche accomplie, ils définiraient précisément la structure de l'arbre, s'assurant que chaque branche avait bien les caractéristiques attendues.

Lorsque la structure fut enfin établie, les érudits entreprirent de remplir cet arbre avec les informations qu'ils possédaient. Ils utilisaient des rituels puissants pour générer un million d'entrées, chacune représentant une graine plantée dans cet arbre gigantesque. Chaque graine était soigneusement identifiée et contenait des inscriptions spéciales sur les branches **B** et **C**, en veillant à ce que ces inscriptions aient exactement vingt caractères, conformément aux anciennes lois de la forêt.

Une fois l'arbre **ABC** rempli, les érudits se tournèrent vers des tâches plus techniques. Ils décidèrent de mesurer la taille de cet arbre colossal, comptant le nombre de graines qu'ils avaient plantées, pour s'assurer que leur travail avait porté ses fruits. Ils effectuèrent des calculs complexes pour définir la taille exacte des blocs de données occupés par les informations stockées dans cet arbre.

Pour optimiser l’accès à ces données, ils utilisèrent une méthode secrète pour rassembler des statistiques précises sur l’arbre, aidant ainsi à mieux gérer son expansion et sa performance. Ils s’enquirent ensuite de la taille exacte des blocs alloués et de l’espace réellement exploitable, déterminant ainsi le facteur de blocage, ou combien d’entrées pouvaient être stockées dans un bloc.

Leur prochaine étape fut de créer un mécanisme leur permettant de se déplacer plus rapidement dans l’arbre. Ils tracèrent un chemin complexe, un **index B+**, gravé autour des branches **A**, permettant ainsi de retrouver rapidement une graine spécifique. Pour s'assurer que cet index fonctionnait correctement, ils étudièrent la structure de ce dernier en détail, calculant à la fois la hauteur de l’index et son ordre, afin de déterminer combien d'étapes ils devaient franchir pour accéder à une graine donnée.

Ensuite, ils effectuèrent des tests. Ils cherchèrent d'abord une graine existante, choisissant le numéro [numero aleatoire entre 0 et 1000000]comme exemple. Ils notèrent avec soin combien de blocs ils devaient traverser pour atteindre cette graine et s'assurèrent que leur index fonctionnait comme prévu. Mais pour s'assurer que leur index était robuste, ils tentèrent également de trouver une graine qui n'existait pas encore, celle numérotée [numero supperieur à 1 million], et ils calculèrent avec précision le coût en termes de blocs traversés pour parvenir à cette conclusion.

Enfin, les érudits commencèrent à planifier l’avenir. Ils savaient que leur forêt continuerait de croître et que l'index B+ devrait évoluer avec elle. Ils se demandèrent combien d’arbres supplémentaires ils devraient planter pour augmenter la hauteur de l’index, et après avoir effectué de nouveaux calculs, ils déterminèrent exactement le nombre de graines supplémentaires qu'il faudrait pour provoquer cette augmentation de hauteur dans l'index.

---

Avec ce récit, **GPT** peut maintenant générer les requêtes SQL du TP qui couvrent (etre super precis , touts les calculs doivent se faire au sein des reqetes en oracle:

**4. Exercice 4 : Tailles de la table et de l'index**

**4.1 Question 1 : Création et Remplissage de la table ABC**

**4.1.1 Création de la table ABC**

**4.1.2 Remplir la table**

**4.1.3 Vérification de la taille de la table**

**4.1.4 Définir les statistiques de la table**

**4.1.5 Récupérer la taille des blocs et la taille moyenne des lignes**

**4.1.6 Calculer la taille exploitable du bloc et le facteur de blocage**

**4.1.7 Récupérer le nombre total de tuples et le nombre de blocs**

**4.2 Question 2 : Création et Statistiques de l'index**

**4.2.1 Créer l'index**

**4.2.2 Récupérer les statistiques de l'index B+**

**4.2.3 Calculer l'ordre et la hauteur de l'arbre B+**

**4.3 Question 3 : Calculs liés au facteur de blocage et à la taille de l'index**

**4.3.1 Calcul du facteur de blocage pour les enregistrements d'index**

**4.3.2 Calcul du nombre de blocs nécessaires au stockage de l'index**

**4.3.2.1 Calcul de la taille des colonnes**

**4.3.3 Calcul de l'ordre et de la hauteur de l'arbre B+**

**4.4 Question 4 : Coût de la consultation par clé avec un index B+**

**4.4.1 Coût en nombre de blocs pour obtenir un tuple existant**

**4.4.2 Coût en nombre de blocs pour un tuple inexistant**

**4.5 Question 5 : Calcul du nombre de tuples supplémentaires pour augmenter la hauteur de l'arbre B+**