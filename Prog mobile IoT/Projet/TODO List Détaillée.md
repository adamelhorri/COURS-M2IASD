## Liste de tâches détaillées pour le projet ESP32 avec API RESTful (generé par prompt GPT O1 )

## Préparation du matériel

- [ ] **Rassembler le matériel nécessaire**
    - [ ] **Microcontrôleur TTGO T-Display (ESP32)**
        - [ ] Vérifier que l'écran OLED est intégré
        - [ ] Vérifier la présence des boutons physiques
    - [ ] **Capteur de température**
        - [ ] Modèle recommandé : DHT22 ou DS18B20
    - [ ] **Capteur de luminosité**
        - [ ] Modèle recommandé : BH1750 ou une photoresistance avec résistance appropriée
    - [ ] **LED**
        - [ ] Choisir une LED standard (couleur au choix)
        - [ ] Obtenir une résistance de 220Ω pour limiter le courant
    - [ ] **Breadboard (plaque de prototypage)**
    - [ ] **Câbles de connexion (jumpers)**
    - [ ] **Câble USB de type C pour connecter l'ESP32 à l'ordinateur**
    - [ ] **Ordinateur avec connexion Internet**
    - [ ] **Alimentation si nécessaire (bien que l'USB suffise généralement)**

## Installation des logiciels nécessaires

- [ ] **Installer l'Arduino IDE**
    - [ ] Accéder au site officiel : https://www.arduino.cc/en/software
    - [ ] Télécharger la version compatible avec votre système d'exploitation
    - [ ] Suivre les instructions d'installation à l'écran
- [ ] **Configurer l'Arduino IDE pour l'ESP32**
    - [ ] Ouvrir l'Arduino IDE
    - [ ] Aller dans **Fichier** > **Préférences**
    - [ ] Dans le champ **URL de gestionnaire de cartes supplémentaires**, ajouter :
        - [ ] `https://dl.espressif.com/dl/package_esp32_index.json`
    - [ ] Cliquer sur **OK**
    - [ ] Aller dans **Outils** > **Type de carte** > **Gestionnaire de cartes**
    - [ ] Rechercher **ESP32** dans la barre de recherche
    - [ ] Installer **esp32 by Espressif Systems**
    - [ ] Après l'installation, sélectionner la carte **TTGO T1** ou **ESP32 Dev Module**
        - [ ] Aller dans **Outils** > **Type de carte** > **ESP32 Arduino** > **TTGO T1**
    - [ ] Sélectionner le port correspondant à l'ESP32 dans **Outils** > **Port**

## Connexion des composants

- [ ] **Connecter le capteur de température (DHT22)**
    - [ ] Identifier les broches du DHT22 : VCC, DATA, GND
    - [ ] Connecter **VCC** à la broche **3.3V** de l'ESP32
    - [ ] Connecter **GND** à la broche **GND** de l'ESP32
    - [ ] Connecter **DATA** à la broche **GPIO 15** de l'ESP32
    - [ ] Placer une résistance de **10kΩ** entre **VCC** et **DATA** (pull-up)
- [ ] **Connecter le capteur de luminosité (BH1750)**
    - [ ] Identifier les broches : VCC, GND, SDA, SCL
    - [ ] Connecter **VCC** à **3.3V** de l'ESP32
    - [ ] Connecter **GND** à **GND** de l'ESP32
    - [ ] Connecter **SDA** à **GPIO 21** de l'ESP32
    - [ ] Connecter **SCL** à **GPIO 22** de l'ESP32
- [ ] **Connecter la LED**
    - [ ] Connecter la patte **anode (+)** de la LED à la broche **GPIO 2** de l'ESP32 via une résistance de **220Ω**
    - [ ] Connecter la patte **cathode (-)** de la LED à **GND** de l'ESP32

## Configuration du code

- [ ] **Installer les bibliothèques nécessaires dans l'Arduino IDE**
    - [ ] Ouvrir l'Arduino IDE
    - [ ] Aller dans **Croquis** > **Inclure une bibliothèque** > **Gérer les bibliothèques**
    - [ ] Rechercher et installer les bibliothèques suivantes :
        - [ ] **DHT sensor library** (par Adafruit)
        - [ ] **Adafruit Unified Sensor**
        - [ ] **BH1750**
        - [ ] **ESPAsyncWebServer**
        - [ ] **AsyncTCP**
        - [ ] **ArduinoJson**
        - [ ] **TFT_eSPI** (pour l'écran OLED)
- [ ] **Configurer le code pour la connexion Wi-Fi**
    - [ ] Créer un nouveau croquis dans l'Arduino IDE
    - [ ] Inclure les bibliothèques Wi-Fi :
        - [ ] `#include <WiFi.h>`
    - [ ] Définir les identifiants Wi-Fi :
        - [ ] `const char* ssid = "Votre_SSID";`
        - [ ] `const char* password = "Votre_Mot_de_Passe";`
- [ ] **Initialiser les capteurs dans le code**
    - [ ] Pour le **DHT22** :
        - [ ] `#include <DHT.h>`
        - [ ] `#define DHTPIN 15`
        - [ ] `#define DHTTYPE DHT22`
        - [ ] `DHT dht(DHTPIN, DHTTYPE);`
    - [ ] Pour le **BH1750** :
        - [ ] `#include <BH1750.h>`
        - [ ] `BH1750 lightMeter;`
- [ ] **Configurer l'écran OLED**
    - [ ] Inclure la bibliothèque :
        - [ ] `#include <TFT_eSPI.h>`
    - [ ] Créer une instance :
        - [ ] `TFT_eSPI tft = TFT_eSPI();`
- [ ] **Configurer les broches pour les boutons**
    - [ ] Définir les broches :
        - [ ] `#define BUTTON_1 0` (si disponible)
        - [ ] `#define BUTTON_2 35` (si disponible)
    - [ ] Configurer les broches en entrée :
        - [ ] `pinMode(BUTTON_1, INPUT_PULLUP);`
        - [ ] `pinMode(BUTTON_2, INPUT_PULLUP);`
- [ ] **Configurer la broche de la LED**
    - [ ] Définir la broche :
        - [ ] `#define LED_PIN 2`
    - [ ] Configurer la broche en sortie :
        - [ ] `pinMode(LED_PIN, OUTPUT);`

## Écriture du code principal

- [ ] **Initialiser la connexion Wi-Fi dans `setup()`**
    - [ ] `WiFi.begin(ssid, password);`
    - [ ] Ajouter une boucle pour attendre la connexion :
        - [ ] `while (WiFi.status() != WL_CONNECTED) { delay(500); }`
- [ ] **Initialiser les capteurs dans `setup()`**
    - [ ] Pour le **DHT22** :
        - [ ] `dht.begin();`
    - [ ] Pour le **BH1750** :
        - [ ] `lightMeter.begin();`
- [ ] **Initialiser l'écran OLED dans `setup()`**
    - [ ] `tft.init();`
    - [ ] `tft.setRotation(1);`
- [ ] **Configurer le serveur web**
    - [ ] Inclure les bibliothèques :
        - [ ] `#include <ESPAsyncWebServer.h>`
        - [ ] `#include <AsyncTCP.h>`
    - [ ] Créer une instance du serveur :
        - [ ] `AsyncWebServer server(80);`
    - [ ] **Définir les routes de l'API RESTful**
        - [ ] Route pour **lister les capteurs** :
            - [ ] `server.on("/sensors", HTTP_GET, [](AsyncWebServerRequest *request){ /* code */ });`
        - [ ] Route pour **récupérer les données des capteurs** :
            - [ ] `server.on("/sensors/data", HTTP_GET, [](AsyncWebServerRequest *request){ /* code */ });`
        - [ ] Route pour **contrôler la LED** :
            - [ ] `server.on("/led", HTTP_POST, [](AsyncWebServerRequest *request){ /* code */ });`
        - [ ] Route pour **définir les seuils** :
            - [ ] `server.on("/threshold", HTTP_POST, [](AsyncWebServerRequest *request){ /* code */ });`
    - [ ] Démarrer le serveur :
        - [ ] `server.begin();`
- [ ] **Écrire les fonctions pour les routes de l'API**
    - [ ] **Pour `/sensors`** :
        - [ ] Créer un JSON avec la liste des capteurs et leurs types
        - [ ] Envoyer la réponse avec `request->send(200, "application/json", jsonString);`
    - [ ] **Pour `/sensors/data`** :
        - [ ] Lire la température :
            - [ ] `float temperature = dht.readTemperature();`
        - [ ] Lire la luminosité :
            - [ ] `float lux = lightMeter.readLightLevel();`
        - [ ] Créer un JSON avec les valeurs lues
        - [ ] Envoyer la réponse
    - [ ] **Pour `/led`** :
        - [ ] Récupérer le paramètre d'état (`on` ou `off`)
        - [ ] Allumer ou éteindre la LED en conséquence :
            - [ ] `digitalWrite(LED_PIN, HIGH);` ou `digitalWrite(LED_PIN, LOW);`
        - [ ] Envoyer une confirmation
    - [ ] **Pour `/threshold`** :
        - [ ] Récupérer les valeurs de seuil depuis la requête
        - [ ] Stocker les seuils pour une utilisation dans le `loop()`
        - [ ] Envoyer une confirmation
- [ ] **Mettre à jour l'écran OLED dans la boucle `loop()`**
    - [ ] Afficher la température :
        - [ ] `tft.drawString("Temp: " + String(temperature) + "C", x, y);`
    - [ ] Afficher la luminosité :
        - [ ] `tft.drawString("Light: " + String(lux) + "lx", x, y+20);`
    - [ ] Afficher l'état de la LED :
        - [ ] `tft.drawString("LED: " + String(ledState ? "On" : "Off"), x, y+40);`
- [ ] **Gérer les boutons physiques dans la boucle `loop()`**
    - [ ] Lire l'état des boutons :
        - [ ] `if (digitalRead(BUTTON_1) == LOW) { /* code pour allumer la LED */ }`
        - [ ] `if (digitalRead(BUTTON_2) == LOW) { /* code pour éteindre la LED */ }`
- [ ] **Implémenter la logique des seuils dans la boucle `loop()`**
    - [ ] Comparer les valeurs lues des capteurs aux seuils définis
    - [ ] Allumer ou éteindre la LED en fonction des conditions

## Compilation et téléversement du code

- [ ] **Vérifier le code**
    - [ ] Cliquer sur le bouton **Vérifier** dans l'Arduino IDE
    - [ ] Corriger toutes les erreurs éventuelles
- [ ] **Téléverser le code vers l'ESP32**
    - [ ] Connecter l'ESP32 à l'ordinateur via USB
    - [ ] Sélectionner le bon port dans **Outils** > **Port**
    - [ ] Cliquer sur **Téléverser**
    - [ ] Attendre que le processus se termine avec succès

## Test du système

- [ ] **Vérifier la connexion Wi-Fi**
    - [ ] Ouvrir le **Moniteur Série** dans l'Arduino IDE
    - [ ] Confirmer que l'ESP32 affiche une adresse IP
- [ ] **Tester l'API RESTful**
    - [ ] Ouvrir un navigateur web ou un outil comme Postman
    - [ ] Accéder à `http://[adresse_IP]/sensors` pour vérifier la liste des capteurs
    - [ ] Accéder à `http://[adresse_IP]/sensors/data` pour obtenir les données
    - [ ] Envoyer une requête POST à `http://[adresse_IP]/led` avec le paramètre `state=on` ou `state=off`
    - [ ] Vérifier que la LED réagit correctement
- [ ] **Tester l'affichage sur l'écran OLED**
    - [ ] Vérifier que les données des capteurs s'affichent correctement
    - [ ] Vérifier que l'état de la LED est mis à jour
- [ ] **Tester les boutons physiques**
    - [ ] Appuyer sur le bouton pour allumer la LED
    - [ ] Appuyer sur l'autre bouton pour éteindre la LED
    - [ ] Vérifier que l'écran OLED et l'API reflètent ces changements
- [ ] **Tester les seuils automatiques**
    - [ ] Définir un seuil via l'API (par exemple, température > 25°C)
    - [ ] Chauffer le capteur de température (en le tenant dans la main)
    - [ ] Observer si la LED s'allume automatiquement

## Rédaction du rapport en LaTeX

- [ ] **Installer un éditeur LaTeX**
    - [ ] Télécharger et installer **TeXstudio** ou un autre éditeur
    - [ ] Installer une distribution LaTeX complète (par exemple, MiKTeX pour Windows)
- [ ] **Créer la structure du document**
    - [ ] Créer un nouveau document `.tex`
    - [ ] Ajouter les sections :
        - [ ] **Introduction**
        - [ ] **Architecture du code**
        - [ ] **Interaction avec les capteurs et la LED**
        - [ ] **Gestion des erreurs**
        - [ ] **Sécurité de l'API**
        - [ ] **Optimisation de la consommation d'énergie**
        - [ ] **Mise à jour et reconfiguration du système**
        - [ ] **Conclusion**
- [ ] **Rédiger chaque section en détail**
    - [ ] Expliquer les choix techniques
    - [ ] Inclure des extraits de code si nécessaire
    - [ ] Ajouter des schémas ou des diagrammes pour illustrer
- [ ] **Compiler le document pour générer un PDF**
    - [ ] Utiliser la fonction **Compiler** dans l'éditeur LaTeX
    - [ ] Corriger les erreurs de compilation s'il y en a
    - [ ] Vérifier la mise en page et le contenu du PDF final

## Vérifications finales

- [ ] **Relecture et correction du code**
    - [ ] S'assurer que le code est bien commenté
    - [ ] Vérifier la cohérence des noms de variables et de fonctions
- [ ] **Vérification du matériel**
    - [ ] S'assurer que toutes les connexions sont sécurisées
    - [ ] Tester la durabilité du montage
- [ ] **Documentation supplémentaire**
    - [ ] Créer un guide d'utilisateur pour l'installation et l'utilisation
    - [ ] Inclure des photos du montage et des captures d'écran
- [ ] **Sauvegarde du projet**
    - [ ] Sauvegarder le code source
    - [ ] Sauvegarder le rapport LaTeX et le PDF
    - [ ] Stocker les fichiers sur un support externe ou dans le cloud

## Présentation du projet

- [ ] **Préparer une présentation (si nécessaire)**
    - [ ] Créer des diapositives pour présenter le projet
    - [ ] Inclure les objectifs, la méthodologie, les résultats
- [ ] **Démonstration pratique**
    - [ ] Préparer le matériel pour une démonstration en direct
    - [ ] Vérifier que tous les éléments fonctionnent correctement
- [ ] **Session de questions/réponses**
    - [ ] Anticiper les questions possibles sur le projet
    - [ ] Préparer des réponses détaillées et claires

---

En suivant cette liste de tâches détaillée, même une personne sans expérience en programmation ou en IoT pourra réaliser le projet étape par étape. Chaque section est conçue pour guider l'utilisateur à travers les aspects techniques avec des instructions claires et précises.
