---
tags:
  - tps
  - ML
---


**But 1 : Créer un classifieur binaire de base (baseline) pour détecter un tigre ou non-tigre à partir d'images (200 images de tigres et 200 images de non-tigres).**

- Modèle binaire simple.
- Architecture : *réseau de neurones convolutionnel (CNN)*.
- Couches à inclure :
  - Couches de convolution.
  - Couches de *max pooling*.
  - Couches de *flatten* (aplatir les sorties).
  - Couches *dense* (complètement connectées) pour la classification finale.

**But 2 : Tester et améliorer la classification pour chaque catégorie d'animaux.**

- Tester le modèle de base sur différentes catégories d'animaux.
- Améliorer la classification :
  - Modifier les hyperparamètres (taille des lots, taux d'apprentissage, nombre de couches, etc.) jusqu'à obtenir des améliorations.
  - Comparer les performances pour chaque catégorie.
- Objectif :
  - Trouver le meilleur modèle pour chaque catégorie d'animaux.
- Question clé :
  - **Le même modèle est-il optimal pour toutes les catégories d'animaux ?**
- Documenter les résultats :
  - Créer un fichier partagé contenant tous les résultats des expérimentations.

**But 3 : Utiliser un modificateur d'images (Image Data Generator).**

- Pourquoi :
  - Générer plus d'images pour enrichir le jeu d'entraînement.
  - Éviter le surapprentissage (*overfitting*).
- Méthodes d'augmentation d'images :
  - Rotation, zoom, retournement, translation, etc.
- Transfer learning :
  - Utiliser des modèles pré-entraînés sur de grandes bases de données pour améliorer la classification.
  - Adapter ces modèles à chaque catégorie d'animaux.

**But 4 : Génération d'images avec des techniques non supervisées.**

- Génération d'images :
  - Produire des images artificielles à partir de bruit ou de données existantes.
- Techniques :
  - **Variational Autoencoders (VAE)** :
    - Réduire la dimensionnalité et générer des images réalistes à partir d'une représentation latente.
  - **Generative Adversarial Networks (GAN)** :
    - Utiliser un générateur pour créer des images à partir de bruit aléatoire.
    - Utiliser un discriminateur pour distinguer les images réelles des images générées.
    - Optimiser le générateur et le discriminateur pour générer des images de haute qualité.
