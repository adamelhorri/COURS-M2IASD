---
tags:
  - nosql
---
# To-Do List pour le Projet HAI914I

## 📋 Formation du Groupe
- [x] **Décider de la composition du groupe**
  - [x] Choisir entre travailler en solo ou en binôme.
  - [x] Discuter des rôles et responsabilités si en binôme.
- [x] **S'inscrire dans le tableau de l'encadrant**
  - [x] Accéder au [tableau d'inscription](https://docs.google.com/spreadsheets/d/1bQY-Xba11DNcbCC5gujvPnidhajN-Kcb306PjXveF7M/edit?usp=sharing).
  - [x] Remplir les informations requises (noms, emails, groupe).

## 🛠️ Configuration de l'Environnement de Développement
### Installation de Java
- [x] **Télécharger Java 21**
  - [x] Accéder à [Oracle Java 21](https://www.oracle.com/java/technologies/javase-jdk21-downloads.html) ou [OpenJDK 21](https://jdk.java.net/21/).
- [x] **Installer Java 21**
  - [x] Suivre les instructions d'installation spécifiques à votre système d'exploitation.
- [x] **Configurer la variable d'environnement `JAVA_HOME`**
  - [x] Définir `JAVA_HOME` pointant vers le répertoire d'installation de Java 21.
- [x] **Vérifier l'installation**
  - [x] Ouvrir le terminal et exécuter `java -version` pour confirmer que Java 21 est installé correctement.

### Installation de l'IDE
- [x] **Choisir et installer un IDE**
  - [x] Télécharger **Eclipse IDE for Java Developers** depuis [Eclipse](https://www.eclipse.org/downloads/packages/) **OU**
  - [ ] Télécharger **IntelliJ IDEA Community Edition** depuis [JetBrains](https://www.jetbrains.com/idea/download/).
- [ ] **Configurer l'IDE**
  - [ ] Installer les plugins nécessaires (par exemple, Maven pour Eclipse).
  - [ ] Configurer les préférences de l'IDE (formatage du code, thèmes, etc.).

### Clonage et Importation du Projet
- [ ] **Cloner le squelette du projet**
  - [ ] Ouvrir le terminal.
  - [ ] Exécuter :
    ```bash
    git clone https://gitlab.etu.umontpellier.fr/p00000415795/nosql-engine-skeleton.git
    ```
- [ ] **Importer le projet dans l'IDE**
  - **Pour Eclipse :**
    - [ ] Ouvrir Eclipse.
    - [ ] Aller dans `File > Import > Existing Maven Projects`.
    - [ ] Sélectionner le dossier cloné.
    - [ ] Cliquer sur `Finish`.
  - **Pour IntelliJ IDEA :**
    - [ ] Ouvrir IntelliJ.
    - [ ] Aller dans `File > New > Project from Existing Sources`.
    - [ ] Sélectionner le dossier cloné.
    - [ ] Choisir `Import project from external model` et sélectionner `Maven`.
    - [ ] Cliquer sur `Finish`.
- [ ] **Vérifier le fonctionnement du projet**
  - [ ] Localiser la classe `Example.java` dans l'IDE.
  - [ ] Exécuter `Example.java`.
  - [ ] S'assurer qu'il s'exécute sans erreurs.

## 📚 Étude du Squelette du Projet
- [ ] **Lire et comprendre les classes fournies**
  - [x] **Example.java** : Comprendre la lecture des données et requêtes.
  - [ ] **RDFAtom.java** : Comprendre la représentation des triplets RDF.
  - [ ] **StarQuery.java** : Comprendre la représentation des requêtes en étoile.
  - [ ] **RDFStorage.java** : Comprendre les méthodes à implémenter.
- [ ] **Étudier les dépendances externes**
  - [ ] Lire la documentation de [rdf4j](https://rdf4j.org/documentation).
  - [ ] Lire la Javadoc de [rdf4j](https://rdf4j.org/javadoc/latest/).
  - [ ] Lire la documentation d'[Integraal](https://rules.gitlabpages.inria.fr/integraal-website/).

## 🗓️ Planification du Projet
- [ ] **Définir les sous-tâches**
  - [ ] Répartir les tâches entre les membres du groupe (si en binôme).
- [ ] **Établir un calendrier**
  - [ ] Planifier les étapes clés jusqu'au 15 novembre, 29 novembre, et 13 décembre.
  - [ ] Allouer du temps pour les tests, la validation, et l'écriture des rapports.
- [ ] **Créer un diagramme de Gantt (optionnel)**
  - [ ] Utiliser un outil comme [Trello](https://trello.com/) ou [GanttProject](https://www.ganttproject.biz/) pour visualiser les échéances.

## 🛠️ Implémentation du Dictionnaire (Rendu 15 Novembre)
### Conception du Dictionnaire
- [ ] **Définir la structure du dictionnaire**
  - [ ] Chaque ressource RDF est associée à un entier unique.
  - [ ] Prévoir des méthodes pour ajouter, encoder, et décoder les ressources.
- [ ] **Créer des diagrammes UML (optionnel)**
  - [ ] Visualiser la classe `Dictionary` et ses méthodes.

### Écriture des Tests Unitaires pour le Dictionnaire
- [ ] **Définir les cas de test**
  - [ ] Ajout de nouvelles ressources.
  - [ ] Encodage de ressources existantes.
  - [ ] Décodage de codes valides et invalides.
  - [ ] Gestion des doublons.
- [ ] **Générer les prompts GPT pour les tests unitaires**
  - [ ] Exemple de prompt :
    ```
    Génère des tests unitaires en Java pour une classe Dictionary qui encode et décode des ressources RDF en entiers. Couvre les cas normaux (ajout, encodage, décodage) et les cas limites (ressource inexistante, doublons).
    ```
- [ ] **Implémenter les tests unitaires**
  - [ ] Copier-coller le code généré par GPT dans les fichiers de test (`src/test/java`).
  - [ ] Adapter les tests selon les besoins spécifiques du projet.

### Implémentation de la Classe `Dictionary`
- [ ] **Générer le prompt GPT pour la classe Dictionary**
  - Exemple de prompt :
    ```
    Écris une classe Java nommée Dictionary qui associe chaque ressource RDF à un entier unique. La classe doit inclure les méthodes suivantes :
    - addResource(String resource) : ajoute une ressource et retourne son code.
    - encode(String resource) : retourne le code associé à une ressource.
    - decode(int code) : retourne la ressource associée à un code.
    Assure-toi que la classe gère les doublons et les exceptions pour les codes inexistants.
    ```
- [ ] **Copier-coller le code généré par GPT**
  - [ ] Créer la classe `Dictionary.java` dans le package approprié.
  - [ ] Insérer le code généré et ajuster si nécessaire.
- [ ] **Intégrer le dictionnaire dans l'Hexastore**
  - [ ] Modifier la classe `Hexastore.java` pour inclure une instance de `Dictionary`.
  - [ ] Adapter la méthode `add(RDFAtom a)` pour utiliser le dictionnaire lors de l'ajout de triplets.
  - [ ] Exemple de prompt GPT pour l'intégration :
    ```
    Modifie la classe Hexastore en Java pour inclure une instance de la classe Dictionary. Adapte la méthode add(RDFAtom a) pour utiliser le dictionnaire lors de l'ajout des triplets RDF.
    ```

### Exécution et Validation
- [ ] **Exécuter les tests unitaires du dictionnaire**
  - [ ] Utiliser l'outil de couverture de code de l'IDE.
  - [ ] Assurer une couverture de code supérieure à 80%.
- [ ] **Corriger les erreurs éventuelles**
  - [ ] Réviser le code et les tests si des tests échouent.
- [ ] **Revue de code**
  - [ ] Vérifier la lisibilité, la modularité, et la conformité aux standards de codage.
  - [ ] Ajouter des commentaires pertinents.

## 🛠️ Implémentation de l'Index (Hexastore) (Rendu 15 Novembre)
### Étude de l'Approche Hexastore
- [ ] **Comprendre les différents index utilisés dans Hexastore**
  - [ ] SPO, SOP, PSO, POS, OSP, OPS.
- [ ] **Lire des articles ou tutoriels sur Hexastore**
  - [ ] Rechercher des ressources académiques ou des implémentations existantes.

### Conception des Structures de Données pour l'Index
- [ ] **Définir les maps ou structures nécessaires pour chaque type d'index**
  - [ ] Par exemple, Map<S, Map<P, set o pour SPO.
- [ ] **Créer des diagrammes UML (optionnel)**
  - [ ] Visualiser les différentes structures d'index.

### Écriture des Tests Unitaires pour l'Index
- [ ] **Définir les cas de test**
  - [ ] Ajout de triplets.
  - [ ] Recherche simple et complexe.
  - [ ] Gestion des doublons.
  - [ ] Cas limites (triplet inexistant, recherches non correspondantes).
- [ ] **Générer les prompts GPT pour les tests unitaires**
  - [ ] Exemple de prompt :
    ```
    Génère des tests unitaires en Java pour une classe Hexastore qui implémente les méthodes add(RDFAtom a) et match(RDFAtom a). Couvre les cas de recherche simples (par un seul patron) et complexes (multiples patrons), ainsi que les cas limites.
    ```
- [ ] **Implémenter les tests unitaires**
  - [ ] Copier-coller le code généré par GPT dans les fichiers de test (`src/test/java`).
  - [ ] Adapter les tests selon les besoins spécifiques du projet.

### Implémentation des Méthodes `add` et `match` de l'Hexastore
- [ ] **Générer le prompt GPT pour les méthodes**
  - Exemple de prompt :
    ```
    Écris les méthodes add(RDFAtom a) et match(RDFAtom a) pour une classe Hexastore en Java, en utilisant plusieurs index (SPO, SOP, PSO, POS, OSP, OPS) pour une recherche efficace des triplets RDF. La méthode add doit insérer le triplet dans tous les index, et la méthode match doit permettre de rechercher les triplets en fonction des patrons partiels.
    ```
- [ ] **Copier-coller le code généré par GPT**
  - [ ] Insérer le code dans la classe `Hexastore.java`.
  - [ ] Adapter le code pour intégrer le dictionnaire lors de l'ajout des triplets.
- [ ] **Optimiser les méthodes**
  - [ ] Assurer l'efficacité des recherches.
  - [ ] Gérer les cas particuliers et les exceptions.

### Exécution et Validation
- [ ] **Exécuter les tests unitaires de l'index**
  - [ ] Utiliser l'outil de couverture de code de l'IDE.
  - [ ] Assurer une couverture de code supérieure à 80%.
- [ ] **Corriger les erreurs éventuelles**
  - [ ] Réviser le code et les tests si des tests échouent.
- [ ] **Revue de code**
  - [ ] Vérifier la lisibilité, la modularité, et la conformité aux standards de codage.
  - [ ] Ajouter des commentaires pertinents.

## 🔗 Intégration du Dictionnaire et de l'Index dans l'Hexastore
- [ ] **Modifier la classe `Hexastore` pour utiliser le dictionnaire et les index**
  - [ ] Assurer que les méthodes `add` et `match` intègrent correctement les composants.
- [ ] **Vérifier le fonctionnement avec `Example.java`**
  - [ ] Exécuter `Example.java` et s'assurer que les données sont correctement ajoutées et que les requêtes fonctionnent.
- [ ] **Déboguer les problèmes éventuels**
  - [ ] Utiliser les logs et les messages d'erreur pour identifier et corriger les problèmes.

## ✅ Vérification et Validation Finale (Rendu 15 Novembre)
- [ ] **Exécuter tous les tests unitaires**
  - [ ] S'assurer qu'il n'y a pas d'échecs.
- [ ] **Vérifier la couverture de code**
  - [ ] Utiliser les outils de l'IDE pour s'assurer que tous les chemins sont couverts.
- [ ] **Corriger les éventuelles erreurs**
  - [ ] Revoir le code et les tests en cas de problèmes détectés.
- [ ] **Revoir le code pour la qualité**
  - [ ] Assurer la lisibilité, la modularité et la conformité aux standards de codage.
- [ ] **Nettoyer le projet**
  - [ ] Supprimer les fichiers temporaires ou non nécessaires.
- [ ] **Vérifier la compilation**
  - [ ] S'assurer que le projet compile sans erreurs.
- [ ] **Mettre à jour le dépôt Git**
  - [ ] Ajouter tous les fichiers modifiés :
    ```bash
    git add .
    ```
  - [ ] Committer les changements avec un message clair :
    ```bash
    git commit -m "Implémentation du dictionnaire et de l'index Hexastore"
    ```
  - [ ] Pousser les commits vers le dépôt distant :
    ```bash
    git push origin main
    ```
- [ ] **Finaliser la soumission**
  - [ ] S'assurer que tous les fichiers requis sont présents dans le dépôt.
  - [ ] Soumettre le dépôt via la plateforme demandée avant le **15 novembre**.

## 🛠️ Évaluation des Requêtes en Étoile (Rendu 29 Novembre)
### Implémentation de l'Évaluation des Requêtes
- [ ] **Concevoir la méthode d'évaluation des requêtes en étoile**
  - [ ] Définir comment filtrer les résultats étape par étape.
- [ ] **Écrire les tests unitaires pour l'évaluation des requêtes**
  - [ ] Définir des cas de test pour différentes requêtes en étoile.
  - [ ] Générer les prompts GPT pour les tests unitaires.
    ```
    Génère des tests unitaires en Java pour une méthode evaluateStarQuery(StarQuery q) dans la classe Hexastore. Couvre différents scénarios de requêtes en étoile, incluant des requêtes avec plusieurs patrons et des cas limites.
    ```
- [ ] **Implémenter la méthode d'évaluation des requêtes**
  - [ ] Utiliser un prompt GPT pour générer le code.
    ```
    Écris une méthode evaluateStarQuery(StarQuery q) dans la classe Hexastore en Java, qui évalue une requête en étoile en utilisant les index existants et retourne les résultats correspondants.
    ```
  - [ ] Copier-coller et adapter le code généré.
- [ ] **Intégrer la méthode dans le flux de travail**
  - [ ] Modifier `Example.java` ou créer une nouvelle classe pour tester l'évaluation des requêtes.
- [ ] **Exécuter les tests et vérifier les résultats**
  - [ ] Utiliser l'outil de couverture de code de l'IDE.
  - [ ] Assurer une couverture de code supérieure à 80%.

### Vérification de Correction et Complétude
- [ ] **Implémenter la procédure de comparaison avec Integraal**
  - [ ] Utiliser `SimpleInMemoryGraphStore` pour le stockage des triplets.
  - [ ] Utiliser la méthode `asFOQuery` de la classe `StarQuery` pour convertir les requêtes.
  - [ ] Générer un prompt GPT pour la procédure de comparaison.
    ```
    Écris une méthode en Java pour comparer les résultats d'une requête en étoile exécutée par le moteur Hexastore avec ceux obtenus par Integraal. Utilise SimpleInMemoryGraphStore pour le stockage des triplets et convertis les requêtes avec asFOQuery.
    ```
  - [ ] Copier-coller et adapter le code généré.
- [ ] **Écrire des tests unitaires pour la procédure de comparaison**
  - [ ] Définir des cas de test où les résultats doivent être identiques.
  - [ ] Générer les prompts GPT pour les tests unitaires.
    ```
    Génère des tests unitaires en Java pour une méthode compareResults(List<Result> hexastoreResults, List<Result> integraalResults) qui vérifie que les deux listes de résultats sont identiques.
    ```
- [ ] **Exécuter les tests et corriger les erreurs**
  - [ ] S'assurer que les résultats de Hexastore correspondent à ceux d'Integraal.
- [ ] **Documenter les résultats de la vérification**
  - [ ] Noter les différences éventuelles et les corriger.

### Rédaction du Rapport (3 Pages)
- [ ] **Structurer le rapport**
  - [ ] Introduction
  - [ ] Méthodologie
  - [ ] Résultats
  - [ ] Conclusion
- [ ] **Rédiger chaque section**
  - [ ] Décrire l'implémentation du dictionnaire et de l'index.
  - [ ] Expliquer le processus d'évaluation des requêtes en étoile.
  - [ ] Présenter les résultats de la comparaison avec Integraal.
- [ ] **Revoir et corriger le rapport**
  - [ ] Vérifier la clarté et la concision.
  - [ ] S'assurer que le rapport respecte la limite de 3 pages.
- [ ] **Soumettre le rapport et le code**
  - [ ] Mettre à jour le dépôt Git avec le rapport.
  - [ ] Soumettre via la plateforme demandée avant le **29 novembre**.

## 🛠️ Analyse des Performances (Rendu 13 Décembre)
### Implémentation de la Phase d'Analyse des Performances
- [ ] **Définir les métriques de performance**
  - [ ] Temps d'exécution des requêtes.
  - [ ] Utilisation de la mémoire.
  - [ ] Scalabilité avec l'augmentation des données.
- [ ] **Configurer l'environnement pour les tests de performance**
  - [ ] Préparer des jeux de données volumineux (e.g., `100K.nt`).
  - [ ] Préparer des ensembles de requêtes variées.
- [ ] **Écrire des scripts pour automatiser les tests de performance**
  - [ ] Générer un prompt GPT pour les scripts.
    ```
    Écris un script en Java pour mesurer le temps d'exécution et l'utilisation de la mémoire lors de l'exécution de requêtes en étoile sur le moteur Hexastore. Utilise des jeux de données volumineux et enregistre les résultats dans un fichier CSV.
    ```
  - [ ] Copier-coller et adapter le code généré.
- [ ] **Exécuter les tests de performance**
  - [ ] Lancer les scripts sur différents jeux de données.
  - [ ] Collecter les résultats.

### Analyse des Résultats
- [ ] **Comparer les performances avec Integraal**
  - [ ] Exécuter les mêmes tests sur Integraal.
  - [ ] Collecter les résultats.
- [ ] **Identifier les goulots d'étranglement**
  - [ ] Analyser les temps d'exécution et l'utilisation de la mémoire.
  - [ ] Proposer des améliorations si nécessaire.
- [ ] **Visualiser les résultats**
  - [ ] Créer des graphiques et des tableaux pour illustrer les performances.
  - [ ] Utiliser des outils comme Excel, Google Sheets, ou des bibliothèques Java (e.g., JFreeChart).

### Rédaction du Rapport (5 Pages)
- [ ] **Structurer le rapport**
  - [ ] Introduction
  - [ ] Méthodologie des tests de performance
  - [ ] Présentation des résultats
  - [ ] Analyse et discussion
  - [ ] Conclusion
- [ ] **Rédiger chaque section**
  - [ ] Décrire les configurations des tests.
  - [ ] Présenter les résultats sous forme de graphiques et tableaux.
  - [ ] Analyser les performances comparées à Integraal.
  - [ ] Discuter des améliorations possibles.
- [ ] **Revoir et corriger le rapport**
  - [ ] Vérifier la clarté et la concision.
  - [ ] S'assurer que le rapport respecte la limite de 5 pages.
- [ ] **Soumettre le rapport et le code**
  - [ ] Mettre à jour le dépôt Git avec le rapport.
  - [ ] Soumettre via la plateforme demandée avant le **13 décembre**.

## 📄 Documentation et Qualité du Code
- [ ] **Commenter le code**
  - [ ] Ajouter des commentaires clairs et pertinents dans toutes les classes et méthodes.
- [ ] **Mettre à jour le README**
  - [ ] Décrire le projet, les fonctionnalités implémentées, et comment exécuter le code.
  - [ ] Inclure des instructions pour configurer l'environnement et exécuter les tests.
- [ ] **Assurer la conformité aux standards de codage**
  - [ ] Utiliser des outils de linting ou de formatage automatique.
- [ ] **Effectuer une revue de code finale**
  - [ ] Relire le code pour détecter des améliorations possibles.
  - [ ] Assurer la cohérence et la modularité du code.

## 🚀 Préparation des Soumissions
### Pour le Rendu du 15 Novembre
- [ ] **Vérifier l'implémentation du dictionnaire et de l'index**
  - [ ] S'assurer que toutes les fonctionnalités sont implémentées et testées.
- [ ] **Nettoyer le projet**
  - [ ] Supprimer les fichiers temporaires ou non nécessaires.
- [ ] **Vérifier la compilation**
  - [ ] S'assurer que le projet compile sans erreurs.
- [ ] **Mettre à jour le dépôt Git**
  - [ ] Ajouter tous les fichiers modifiés :
    ```bash
    git add .
    ```
  - [ ] Committer les changements avec un message clair :
    ```bash
    git commit -m "Finalisation du dictionnaire et de l'index Hexastore"
    ```
  - [ ] Pousser les commits vers le dépôt distant :
    ```bash
    git push origin main
    ```
- [ ] **Finaliser la soumission**
  - [ ] S'assurer que tous les fichiers requis sont présents dans le dépôt.
  - [ ] Soumettre le dépôt via la plateforme demandée avant le **15 novembre**.

### Pour le Rendu du 29 Novembre et 13 Décembre
- [ ] **Répéter les étapes de vérification et de soumission**
  - [ ] Pour chaque rendu, vérifier que le code et les rapports sont complets.
  - [ ] Nettoyer, compiler, committer, et pousser les changements.
  - [ ] Soumettre via la plateforme demandée avant les dates limites.

## 🔍 Présence en TD et Explications
- [ ] **Assurer la présence en TD**
  - [ ] Participer activement aux séances de TD.
- [ ] **Préparer les explications du travail réalisé**
  - [ ] Préparer une présentation orale des fonctionnalités implémentées.
  - [ ] Être prêt à répondre aux questions des encadrants sur le code et les choix techniques.

## 📌 Notes Importantes
- **Utilisation de GPT pour le Code** : Pour chaque étape nécessitant l'écriture de code, utilisez un prompt GPT adapté pour générer des squelettes ou des implémentations complètes. Vérifiez et adaptez le code généré pour qu'il corresponde aux besoins spécifiques du projet.
  
- **Méthodologie TDD (Test-Driven Development)** : Écrivez d'abord les tests unitaires avant d'implémenter les fonctionnalités. Cela garantit la fiabilité et la robustesse du code.

- **Couverture de Code** : Visez une couverture de code supérieure à 80% en utilisant les outils intégrés de votre IDE. Identifiez et testez les parties non couvertes.

- **Gestion du Dépôt Git** : Commitez régulièrement avec des messages clairs et descriptifs. Utilisez des branches si nécessaire pour organiser le développement.

- **Documentation et Commentaires** : Un code bien documenté est plus facile à maintenir et à comprendre. Ajoutez des commentaires pertinents et maintenez une documentation à jour.

- **Collaboration en Binôme** : Si vous travaillez en binôme, utilisez des outils de collaboration comme GitLab, Trello, ou Slack pour communiquer et gérer les tâches efficacement.

---

**Bonne chance pour votre projet ! 🚀**

