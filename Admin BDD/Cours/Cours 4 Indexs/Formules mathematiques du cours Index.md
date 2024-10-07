---
tags:
  - AdminBDD
  - cours
---


### 1. **Recherche Linéaire avec ISAM**
   - **Formule** : \( B/2 \)
   - **Utilité** : Cette formule calcule le coût moyen de recherche dans une organisation séquentielle de données, en indiquant qu'en moyenne, la moitié des blocs doivent être parcourus pour trouver une valeur spécifique. L'efficacité est mesurée en termes de blocs à lire.
   - **Exemple** : Dans une table organisée de manière séquentielle avec 100 000 blocs de données, une recherche linéaire va en moyenne nécessiter de parcourir 50 000 blocs pour trouver la valeur recherchée. Si la valeur n'existe pas, tous les blocs seront parcourus.

### 2. **Recherche Dichotomique avec Index Dense**
   - **Formule** : 
     \[
     \log_2(10\,000) = \frac{\ln(10\,000)}{\ln(2)} = 13.28 \approx 14 \text{ blocs}
     \]
   - **Utilité** : Lorsque vous avez un index dense (c'est-à-dire que chaque valeur est représentée dans l'index), la recherche se fait par dichotomie. Cela signifie que chaque étape divise en deux l'ensemble des blocs à examiner, garantissant une complexité logarithmique, ce qui est bien plus rapide qu'une recherche linéaire.
   - **Exemple** : Avec un index dense sur une table de 1 000 000 de tuples répartis dans 10 000 blocs, la recherche d'une clé spécifique nécessitera environ 14 lectures de blocs, car chaque lecture permet de diviser par deux le nombre de blocs restants à parcourir.

### 3. **Recherche avec Index Creux**
   - **Formule** : 
     \[
     \log_2(1\,000) = \frac{\ln(1\,000)}{\ln(2)} = 9.96 \approx 10 \text{ blocs}
     \]
   - **Utilité** : Avec un index creux (ou sparse), seul un sous-ensemble de valeurs de clé est indexé, typiquement une valeur par bloc. Cela permet de réduire la taille de l'index, mais augmente légèrement le coût de recherche car il peut être nécessaire de lire des blocs non indexés.
   - **Exemple** : Si l'index ne contient qu'une entrée par bloc de données, pour une table de 1 000 000 de tuples répartis dans 100 000 blocs, environ 10 blocs devront être parcourus pour trouver la clé correspondante.

### 4. **Recherche avec Index Multi-niveaux**
   - **Formule** : 
     \[
     \log_2(10) = \frac{\ln(10)}{\ln(2)} = 3.32 \approx 4 \text{ blocs}
     \]
   - **Utilité** : Un index multi-niveaux permet de réduire encore plus la recherche. En créant un index sur un autre index (un "index secondaire"), vous pouvez diviser la recherche en plusieurs étapes. Cela est particulièrement utile pour les tables volumineuses.
   - **Exemple** : Pour une table de 1 000 000 de tuples avec un index multi-niveaux, un index dense de premier niveau occupe 1 000 blocs, et un index creux de deuxième niveau occupe 10 blocs. Une recherche nécessitera alors environ 4 accès à des blocs, ce qui est plus efficace que de parcourir les blocs de manière séquentielle.

### 5. **Taille des Nœuds d’un B-Tree**
   - **Formule** : 
     \[
     4n + 8(n+1) \leq 4096
     \]
     \[
     12n = 4088 \Rightarrow n = 340
     \]
   - **Utilité** : Cette formule permet de calculer combien de clés peuvent être stockées dans un bloc de 4 Ko. Cela aide à déterminer l’efficacité de l’utilisation de la mémoire dans les B-Trees, où chaque nœud contient des clés et des pointeurs vers les enfants.
   - **Exemple** : Avec cette formule, vous pouvez déduire que chaque bloc de 4 Ko peut héberger environ 340 clés, ce qui influence la profondeur et l'équilibre de l'arbre. Plus le nombre de clés par bloc est élevé, moins l'arbre sera profond, et donc plus les recherches seront rapides.

### 6. **Hauteur d’un B-Tree**
   - **Formule** : 
     \[
     \log_{2m}(N) \leq h \leq \log_{m}(N)
     \]
   - **Utilité** : La hauteur \( h \) d'un B-Tree dépend du nombre total de tuples \( N \) et de l'ordre \( m \), qui est le nombre maximal de pointeurs dans un nœud. Une hauteur faible garantit que les opérations de recherche, insertion et suppression sont rapides.
   - **Exemple** : Un B-Tree avec 1 000 000 d’enregistrements et un ordre \( m = 255 \) aura une hauteur de 3 environ, ce qui signifie qu'il faudra 3 accès pour trouver un enregistrement. Cela est bien plus rapide que de parcourir tous les enregistrements séquentiellement.

### 7. **Recherche, Insertion, Suppression dans un B-Tree**
   - **Formule** : Pour une hauteur de 3, \(3 \text{ I/O + 1 à 2 I/O supplémentaires}\)
   - **Utilité** : Cette formule décrit le coût d'accès en termes d’opérations d'entrée/sortie (I/O) pour rechercher, insérer ou supprimer une clé. Le nombre d'I/O dépend de la hauteur de l'arbre, et chaque accès représente un saut vers un nœud de l'arbre.
   - **Exemple** : Si un B-Tree a une hauteur de 3, il suffit de 3 opérations I/O pour atteindre un nœud feuille et, par conséquent, environ 4 ou 5 opérations d'entrée/sortie pour une insertion ou suppression. Cela est efficace, car la hauteur reste faible même pour un grand nombre de données.

### 8. **Taille de l’Index Bitmap**
   - **Formule** : 
     \[
     \text{Taille de l'index en blocs} = \text{cardinalité de l'attribut} \times \left(\frac{\text{nombre de tuples}}{8 \times 8192}\right)
     \]
   - **Utilité** : Les index bitmap sont utilisés pour représenter les données sous forme de bits (0 ou 1) et sont très efficaces lorsque la cardinalité est faible (peu de valeurs distinctes). Cela permet de réduire la taille de l'index.
   - **Exemple** : Pour une colonne "Genre" avec seulement deux valeurs possibles ("M" et "F"), un index bitmap peut être utilisé. Il permet de compresser les données et de rendre les recherches sur des valeurs spécifiques très rapides.

### 9. **Fonction de Hachage**
   - **Formule** : 
     \[
     h(\text{Num}) = \text{Num} \mod 3
     \]
   - **Utilité** : Cette formule de hachage est utilisée pour déterminer où stocker un enregistrement dans une table de hachage. En prenant le reste de la division par 3, on peut déterminer la position où l’enregistrement doit être placé.
   - **Exemple** : Supposons que vous avez un identifiant d'employé Num = 31. En utilisant la fonction \( h(\text{Num}) = \text{Num} \mod 3 \), on obtient \( 31 \mod 3 = 1 \). Cela signifie que l'enregistrement sera stocké dans le compartiment 1. Cette approche permet un accès direct, et donc très rapide, pour retrouver un enregistrement à partir de sa clé.

Ces explications sont plus précises et détaillées pour chaque formule, en fournissant une explication scientifique de l’utilisation de ces formules et en illustrant chaque concept par des exemples pratiques. Si vous avez encore des questions ou souhaitez des précisions sur une partie spécifique, je suis à votre disposition.