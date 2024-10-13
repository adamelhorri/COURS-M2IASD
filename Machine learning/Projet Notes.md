---
tags:
  - tps
  - ML
---
## Objectifs

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
## Jeu de données 
Voici une description plus précise du jeu de données d'après le notebook :

### 1. **Classes du jeu de données** :
   - **Trois classes principales** : 
     - **Tigres** : images positives de tigres et des images négatives contenant d'autres animaux.
     - **Éléphants** : images positives d'éléphants et des images négatives d'autres animaux.
     - **Renards** : images positives de renards et des images négatives d'autres animaux.

### 2. **Nombre total d'images** :
   - **200 images par classe** :
     - **Tigres** : 100 images positives (tigres) + 100 images négatives (autres animaux).
     - **Éléphants** : 100 images positives (éléphants) + 100 images négatives (autres animaux).
     - **Renards** : 100 images positives (renards) + 100 images négatives (autres animaux).
   - **Total** : 600 images (3 classes avec 200 images chacune).

### 3. **Taille des images** :
   - **Dimension fixe** : Chaque image est redimensionnée à 128x128 pixels.
   - **Format des données** : Les images sont représentées en format matriciel 3D de dimension `(128, 128, 3)` (128 pixels de hauteur, 128 pixels de largeur, et 3 canaux de couleur correspondant à RGB).

### 4. **Prétraitement des données** :
   - **Normalisation** : Chaque pixel des images est divisé par 255 pour que les valeurs des pixels soient comprises entre 0 et 1.
   - **Redimensionnement** : Les images originales, qui peuvent avoir différentes tailles, sont toutes redimensionnées à 128x128 pixels.

### 5. **Structure des données** :
   - **Tableau `X`** : Contient les images après redimensionnement et normalisation. Chaque image est représentée sous forme de tableau 3D de dimensions `(128, 128, 3)`.
   - **Tableau `y`** : Contient les étiquettes (labels) associées aux images. Ces labels sont des entiers représentant les classes :
     - `0` pour la classe positive (ex : tigres),
     - `1` pour la classe négative (ex : non-tigres).

### 6. **Exemples d'utilisation du jeu de données** :
   - **Affichage d'images** : Le notebook propose des fonctions permettant de visualiser un échantillon de 25 images de chaque classe, converties de BGR (format OpenCV) à RGB (format Matplotlib), accompagnées de leur label.
   
### 7. **Répartition des classes** :
   - **Équilibrage des classes** : Chaque classe a un nombre égal d'images positives et négatives (100 positives, 100 négatives).
   
Ces caractéristiques définissent un jeu de données bien structuré pour des tâches de classification d'images où le modèle doit différencier des animaux (tigres, éléphants, renards) à partir d'images.