
# 📋 Verdict Général du Projet TTGO T-Display avec ESP32

## 🎯 **Objectifs du Projet**

Votre projet vise à :

1. **Implémenter une API RESTful** sur un module **TTGO T-Display (ESP32)**.
2. **Lister les capteurs** connectés (température via **DHT22** et luminosité via **BH1750**).
3. **Récupérer les informations** des capteurs.
4. **Contrôler une LED** (allumage/extinction).
5. **Définir des seuils** pour automatiser le contrôle de la LED en fonction des données des capteurs.
6. **Utiliser l'écran intégré** pour afficher les données et **les boutons** pour une interaction supplémentaire.

## 🛠️ **État Actuel du Projet**

### **Fonctionnalités Implémentées :**

- **Connexion Wi-Fi** : Code en place pour connecter l'ESP32 à un réseau Wi-Fi.
- **Lecture des Capteurs** :
  - **DHT22** pour la température et l'humidité.
  - **BH1750** pour la luminosité.
- **Contrôle de la LED** : Fonctionnalité pour allumer/éteindre la LED via l'API.
- **API RESTful** : Routes configurées pour interagir avec les capteurs et la LED.
- **Affichage TFT** : Tentative d'affichage des données sur l'écran intégré.
- **Boutons Intégrés** : Code partiellement intégré pour utiliser les boutons du TTGO T-Display.

### **Problèmes Identifiés :**

1. **Écran TFT Noir** : L'écran reste noir malgré le téléversement du code.
2. **Erreur `[BH1750] device not configured`** : Indique un problème avec le capteur de luminosité BH1750.
3. **Connexion Wi-Fi** : L'ESP32 ne parvient pas à se connecter au partage de connexion de l'iPhone.

## 🔧 **Schéma Visuel des Branchements**

### **1. Schéma Visuel Simplifié**

Voici un schéma visuel simplifié des branchements entre le **TTGO T-Display (ESP32)** et les composants supplémentaires (**DHT22**, **BH1750**, et **LED**).

```
+----------------------------------+
|         TTGO T-Display           |
|----------------------------------|
|                                  |
| Côté gauche (colonne B)          |  Côté droit (colonne i)
|                                  |
| 1B : 5V                          | 1i : 3V3
| 2B : GND                         | 2i : GND
| 3B : GPIO27                      | 3i : GND
| 4B : GPIO26                      | 4i : GPIO12
| 5B : GPIO25                      | 5i : GPIO13
| 6B : GPIO33                      | 6i : GPIO15 (DHT22 DATA)
| 7B : GPIO32                      | 7i : GPIO2 (LED Anode)
| 8B : GPIO39                      | 8i : GPIO17
| 9B : GPIO38                      | 9i : GPIO22 (BH1750 SCL)
|10B : GPIO37                      |10i : GPIO21 (BH1750 SDA)
|11B : GPIO36                      |11i : GND
|12B : 3V3                         |12i : GND
+----------------------------------+
```

### **2. Branchements Détaillés**

#### **Composants Nécessaires :**

- **TTGO T-Display (ESP32)**
- **Capteur DHT22** (Température et Humidité)
- **Capteur BH1750** (Luminosité)
- **LED**
- **Résistance 220Ω** (pour la LED)
- **Résistance 10kΩ** (pour le DHT22)
- **Câbles de connexion** (Dupont)
- **Breadboard** (facultatif, pour faciliter les connexions)

#### **Tableau des Connexions :**

| **Composant** | **Broche sur TTGO T-Display** | **Fonction**                            |
|---------------|-------------------------------|-----------------------------------------|
| **DHT22**     | 1i : 3V3 → VCC                | Alimentation                            |
|               | 2i : GND → GND                | Masse                                   |
|               | 6i : GPIO15 → DATA            | Signal de données                       |
| **BH1750**    | 1i : 3V3 → VCC                | Alimentation                            |
|               | 2i : GND → GND                | Masse                                   |
|               | 10i : GPIO21 → SDA            | Données I2C                             |
|               | 9i : GPIO22 → SCL             | Horloge I2C                             |
| **LED**       | 7i : GPIO2 → Anode            | Contrôle de la LED via résistance        |
|               | 2i : GND → Cathode            | Masse                                   |
| **Résistances** | - | 10kΩ entre VCC (1i) et DATA (6i) du DHT22; 220Ω entre GPIO2 et l'anode de la LED |

#### **3. Schéma des Branchements Visuel**

```
TTGO T-Display (ESP32)
+----------------------------------+
|                                  |
|    +---------+         +-------+  |
|    |         |         |       |  |
|    | DHT22   |         | BH1750|  |
|    |         |         |       |  |
|    +----+----+         +---+---+  |
|         |                  |      |
|        DATA               SDA     |
|         |                  |      |
|        GPIO15            GPIO21   |
|                                  |
|    +-----+         +-----+        |
|    | LED |         | GND |        |
|    |     |         |     |        |
|    +--+--+         +--+--+        |
|       |               |           |
|      GPIO2          GND          |
|                                  |
+----------------------------------+
```

*Note : Ce schéma est une représentation simplifiée. Assurez-vous de suivre les connexions détaillées ci-dessus.*

## 📝 **Todo List Super Précise et Compréhensible**

### **Étape 1 : Préparer les Composants et Outils**

**Matériel Nécessaire :**

- **TTGO T-Display (ESP32)**
- **Capteur DHT22** (Température et Humidité)
- **Capteur BH1750** (Luminosité)
- **LED**
- **Résistance 220Ω**
- **Résistance 10kΩ**
- **Câbles de connexion** (Dupont)
- **Breadboard** (facultatif)
- **Ordinateur avec Arduino IDE installé**

**Logiciel Nécessaire :**

- **Arduino IDE** : [Télécharger ici](https://www.arduino.cc/en/software)
- **Bibliothèques Arduino** :
  - **TFT_eSPI**
  - **DHT sensor library** par Adafruit
  - **BH1750** par Christopher Laws
  - **AsyncTCP**
  - **ESPAsyncWebServer**
  - **ArduinoJson**

### **Étape 2 : Installer les Bibliothèques Nécessaires**

1. **Ouvrez l'Arduino IDE.**

2. **Installer les Bibliothèques :**

   - **Allez dans** `Croquis` > `Inclure une bibliothèque` > `Gérer les bibliothèques...`

   - **Recherchez et installez** les bibliothèques suivantes :

     - **TFT_eSPI**
       - **Rechercher** : Tapez "TFT_eSPI" dans la barre de recherche.
       - **Installer** : Cliquez sur "Installer".

     - **DHT sensor library** par Adafruit
       - **Rechercher** : Tapez "DHT sensor library".
       - **Installer** : Cliquez sur "Installer".

     - **BH1750** par Christopher Laws
       - **Rechercher** : Tapez "BH1750".
       - **Installer** : Cliquez sur "Installer".

     - **AsyncTCP**
       - **Rechercher** : Tapez "AsyncTCP".
       - **Installer** : Cliquez sur "Installer".

     - **ESPAsyncWebServer**
       - **Rechercher** : Tapez "ESPAsyncWebServer".
       - **Installer** : Cliquez sur "Installer".

     - **ArduinoJson**
       - **Rechercher** : Tapez "ArduinoJson".
       - **Installer** : Cliquez sur "Installer".

   ![Gestionnaire de Bibliothèques](https://i.imgur.com/7Y1JQmI.png)

### **Étape 3 : Configurer la Bibliothèque TFT_eSPI**

1. **Naviguez vers le dossier de la bibliothèque :**

   - **Windows :** `Documents\Arduino\libraries\TFT_eSPI\`
   - **macOS/Linux :** `~/Arduino/libraries/TFT_eSPI/`

2. **Accédez au dossier `User_Setups` :**

   - **Chemin Typique :** `TFT_eSPI/User_Setups/`

3. **Créer un fichier de configuration personnalisé :**

   - **Nom du Fichier :** `User_Setup_ttgo_T_Display.h`
   - **Méthode :**
     - **Windows/macOS/Linux :** Clic droit > Nouveau fichier > Nommer le fichier.


5. **Sauvegardez le Fichier :**

   - **Enregistrez** le fichier `User_Setup_ttgo_T_Display.h` dans le dossier `User_Setups`.

6. **Modifier le Fichier `User_Setup_Select.h` :**

   - **Ouvrez** `User_Setup_Select.h` dans le dossier `TFT_eSPI`.
   
   - **Commentez ou supprimez** toutes les autres inclusions.

   - **Ajoutez la ligne suivante** pour inclure votre configuration personnalisée :

     ```cpp
     // User_Setup_Select.h

     // Cette section sélectionne une configuration personnalisée
     #include "User_Setups/User_Setup_ttgo_T_Display.h"
     ```

7. **Sauvegardez et Fermez `User_Setup_Select.h`.**

### **Étape 4 : Connecter les Composants Physiques**

#### **1. Connecter le DHT22**

- **VCC** du DHT22 → **1i : 3V3** du TTGO

- **GND** du DHT22 → **2i : GND** du TTGO

- **DATA** du DHT22 → **6i : GPIO15** du TTGO

- **Résistance 10kΩ** entre **1i : 3V3** et **6i : GPIO15** du TTGO

#### **2. Connecter le BH1750**

- **VCC** du BH1750 → **1i : 3V3** du TTGO

- **GND** du BH1750 → **2i : GND** du TTGO

- **SDA** du BH1750 → **10i : GPIO21** du TTGO

- **SCL** du BH1750 → **9i : GPIO22** du TTGO

#### **3. Connecter la LED**

- **Anode (longue patte)** de la LED → **7i : GPIO2** du TTGO via une **résistance de 220Ω**

- **Cathode (courte patte)** de la LED → **2i : GND** du TTGO

#### **4. Boutons Intégrés**

Les boutons A et B sont intégrés au TTGO T-Display et connectés respectivement aux broches **GPIO0** et **GPIO4**. **Aucune connexion externe n'est nécessaire.**

### **Étape 5 : Vérifier les Connexions Physiques**

1. **Câbles de Connexion :** Utilisez des câbles fiables et assurez-vous qu'ils sont bien insérés dans les broches correspondantes.

2. **Alimentation :** Vérifiez que tous les composants sont correctement alimentés en 3.3V.

3. **Résistances :** Assurez-vous que les résistances de 10kΩ (pour le DHT22) et 220Ω (pour la LED) sont correctement placées et de bonne valeur.

4. **Solidité des Connexions :** Assurez-vous que les fils ne sont pas desserrés ou mal connectés.

### **Étape 6 : Téléverser un Sketch de Test pour le BH1750**

Avant de passer au code complet, assurez-vous que le capteur BH1750 fonctionne correctement.

1. **Ouvrez l'Arduino IDE.**

2. **Créez un Nouveau Sketch :** Allez dans `Fichier` > `Nouveau`.

3. **Copiez et Collez le Code Suivant :**

   ```cpp
   #include <Wire.h>
   #include <BH1750.h>

   BH1750 lightMeter;

   void setup(){
     Serial.begin(115200);
     Wire.begin();
     
     if (lightMeter.begin(BH1750::CONTINUOUS_HIGH_RES_MODE)) {
       Serial.println("BH1750 initialisé avec succès.");
     }
     else {
       Serial.println("Erreur lors de l'initialisation du BH1750.");
     }
   }

   void loop(){
     float lux = lightMeter.readLightLevel();
     Serial.print("Luminosité: ");
     Serial.print(lux);
     Serial.println(" lx");
     delay(1000);
   }
   ```

4. **Sélectionner la Carte et le Port Corrects :**

   - **Carte :** Allez dans `Outils` > `Type de carte` > `ESP32 Arduino` > **"TTGO T1"** ou **"ESP32 Dev Module"**.

   - **Port :** Allez dans `Outils` > `Port` > sélectionnez le port correspondant (par exemple, **COM3** sous Windows).

5. **Téléverser le Code :** Cliquez sur le bouton **Téléverser** (icône en forme de flèche droite).

6. **Ouvrir le Moniteur Série :**

   - Allez dans `Outils` > `Moniteur Série`.
   
   - **Réglez le Baud Rate** sur **115200**.

7. **Vérifiez les Résultats :**

   - Vous devriez voir des messages indiquant si le BH1750 est initialisé correctement et les lectures de luminosité toutes les secondes.
   
   - **Exemple :**
     ```
     BH1750 initialisé avec succès.
     Luminosité: 123.45 lx
     Luminosité: 130.67 lx
     ...
     ```

#### **Si vous rencontrez l'erreur `[BH1750] device not configured` :**

1. **Vérifiez les Connexions Physiques :**

   - **SDA et SCL** sont correctement connectés aux broches **GPIO21** et **GPIO22** du TTGO.
   
   - **VCC** est connecté à **3V3** et **GND** à **GND**.

2. **Vérifiez l'Adresse I2C :**

   - Utilisez un **scanner I2C** pour vérifier l'adresse du BH1750.

   - **Code pour Scanner l'I2C :**

     ```cpp
     #include <Wire.h>

     void setup(){
       Serial.begin(115200);
       Wire.begin();
       Serial.println("\nI2C Scanner");
       
       byte error, address;
       int nDevices = 0;

       for(address = 1; address < 127; address++ ) {
         Wire.beginTransmission(address);
         error = Wire.endTransmission();

         if (error == 0) {
           Serial.print("I2C device found at address 0x");
           if (address<16) 
             Serial.print("0");
           Serial.print(address, HEX);
           Serial.println(" !");
           nDevices++;
         }
         else if (error==4) {
           Serial.print("Unknown error at address 0x");
           if (address<16) 
             Serial.print("0");
           Serial.println(address, HEX);
         }    
       }
       if (nDevices == 0)
         Serial.println("No I2C devices found\n");
       else
         Serial.println("done\n");
     }

     void loop() {}
     ```

   - **Téléversez et exécutez ce code.**
   
   - **Vérifiez le Moniteur Série** pour voir l'adresse détectée (par exemple, `0x23`).
   
   - **Modifiez le Code d'Initialisation du BH1750** si l'adresse est différente.

     ```cpp
     #include <BH1750.h>

     // Spécifiez l'adresse détectée si différente
     BH1750 lightMeter(0x23); // Remplacez 0x23 par l'adresse correcte

Étape 12 : Finaliser et Tester le Projet￼￼

​￼1. ￼￼Assurez-vous que tout fonctionne :￼￼

   - ￼￼Connexion Wi-Fi￼￼ est établie.
   - ￼￼Écran TFT￼￼ affiche les informations correctement.
   - ￼￼Capteurs DHT22￼￼ et ￼￼BH1750￼￼ fournissent des données précÉtape 12 : Finaliser et Tester le Projet￼￼

​￼1. ￼￼Assurez-vous que tout fonctionne :￼￼

   - ￼￼Connexion Wi-Fi￼￼ est établie.
   - ￼￼Écran TFT￼￼ affiche les informations correctement.
   - ￼￼Capteurs DHT22￼￼ et ￼￼BH1750￼￼ fournissent des données préc     void setup(){
       Serial.begin(115200);
       Wire.begin();
       
       if (lightMeter.begin(BH1750::CONTINUOUS_HIGH_RES_MODE)) {
         Serial.println("BH1750 initialisé avec succès.");
       }
       else {
         Serial.println("Erreur lors de l'initialisation du BH1750.");
       }
     }

     void loop(){
       float lux = lightMeter.readLightLevel();
       Serial.print("Luminosité: ");
       Serial.print(lux);
       Serial.println(" lx");
       delay(1000);
     }
     ```

### **Étape 7 : Téléverser un Sketch de Test pour l'Écran TFT**

Assurez-vous que l'écran TFT fonctionne correctement avant d'intégrer les capteurs.

1. **Ouvrez l'Arduino IDE.**
Étape 12 : Finaliser et Tester le Projet￼￼

​￼1. ￼￼Assurez-vous que tout fonctionne :￼￼

   - ￼￼Connexion Wi-Fi￼￼ est établie.
   - ￼￼Écran TFT￼￼ affiche les informations correctement.
   - ￼￼Capteurs DHT22￼￼ et ￼￼BH1750￼￼ fournissent des données préc
2. **Créez un Nouveau Sketch :** Allez dans `Fichier` > `Nouveau`.

3. **Copiez et Collez le Code Suivant :**

   ```cpp
   #include <TFT_eSPI.h> // Bibliothèque pour l'écran TFT intégré

   TFT_eSPI tft = TFT_eSPI(); // Création d'une instance de l'écran TFT

   void setup() {
     Serial.begin(115200);
     tft.init();
     tft.setRotation(1); // Ajustez en fonction de l'orientation souhaitée
     tft.fillScreen(TFT_BLACK);
     tft.setTextSize(2);
     tft.setTextColor(TFT_WHITE, TFT_BLACK);
     tft.setCursor(10, 10);
     tft.println("Test de l'Ecran");
     tft.setCursor(10, 40);
     tft.println("TTGO T-Display");
   }
Étape 12 : Finaliser et Tester le Projet￼￼

​￼1. ￼￼Assurez-vous que tout fonctionne :￼￼

   - ￼￼Connexion Wi-Fi￼￼ est établie.
   - ￼￼Écran TFT￼￼ affiche les informations correctement.
   - ￼￼Capteurs DHT22￼￼ et ￼￼BH1750￼￼ fournissent des données préc
   void loop() {
     // Rien ici
   }
   ```

4. **Sélectionner la Carte et le Port Corrects :**

   - **Carte :** Allez dans `Outils` > `Type de carte` > `ESP32 Arduino` > **"TTGO T1"** ou **"ESP32 Dev Module"**.
   
   - **Port :** Allez dans `Outils` > `Port` > sélectionnez le port correspondant (par exemple, **COM3** sous Windows).

5. **Téléverser le Code :** Cliquez sur le bouton **Téléverser** (icône en forme de flèche droite).

6. **Vérifier l'Écran :**

   - L'écran devrait afficher :
     ```
     Test de l'Ecran
     TTGO T-Display
     ```

#### **Si l'écran reste noir :**
Étape 12 : Finaliser et Tester le Projet￼￼

​￼1. ￼￼Assurez-vous que tout fonctionne :￼￼

   - ￼￼Connexion Wi-Fi￼￼ est établie.
   - ￼￼Écran TFT￼￼ affiche les informations correctement.
   - ￼￼Capteurs DHT22￼￼ et ￼￼BH1750￼￼ fournissent des données préc
1. **Vérifiez les Connexions Physiques :**

   - Assurez-vous que toutes les broches de l'écran TFT sont correctement connectées.
   
   - Vérifiez l'alimentation en **3V3** et la masse (**GND**).

2. **Revérifiez la Configuration de `User_Setup_ttgo_T_Display.h` :**

   - Assurez-vous que les broches définies dans le fichier de configuration correspondent aux connexions matérielles.

3. **Réinitialisez le TTGO :**

   - Appuyez sur le bouton **RESET** pour redémarrer le module.

4. **Tester avec un Autre Sketch :**

   - Utilisez un autre exemple de la bibliothèque **TFT_eSPI**, comme `GraphicTest`.
   
   - **Allez dans** `Fichier` > `Exemples` > `TFT_eSPI` > `GraphicTest`.
   
   - **Téléversez et vérifiez** si l'écran affiche des formes graphiques.

### **Étape 8 : Téléverser le Code Complet avec Intégration des Boutons**

Une fois que le BH1750 et l'écran TFT fonctionnent correctement, téléversez le code complet pour intégrer toutes les fonctionnalités.

1. **Ouvrez l'Arduino IDE.**

2. **Créez un Nouveau Sketch :** Allez dans `Fichier` > `Nouveau`.

3. **Copiez et Collez le Code Complet Suivant :**

   ```cpp
   // **Inclusion des Bibliothèques Nécessaires**
   #include <WiFi.h>
   #include <AsyncTCP.h>
   #include <ESPAsyncWebServer.h>
   #include <ArduinoJson.h>
   #include <DHT.h>
   #include <BH1750.h>
   #include <Wire.h>
   #include <TFT_eSPI.h> // Bibliothèque pour l'écran TFT intégré

   // **Définition des Paramètres Wi-Fi**
   const char* ssid = "Iphone de El Horri";             // Remplacez par votre SSID Wi-Fi
   const char* password = "12345678";                  // Remplacez par votre mot de passe Wi-Fi

   // **Définition des Broches**
   #define DHTPIN 15         // Broche où est connecté le DHT22 (GPIO15)
   #define DHTTYPE DHT22     // Type de capteur DHT
   #define LED_PIN 2         // Broche où est connectée la LED (GPIO2)
   #define BUTTON_A_PIN 0    // Broche du bouton A (GPIO0)
   #define BUTTON_B_PIN 4    // Broche du bouton B (GPIO4)

   // **Initialisation des Objets**
   DHT dht(DHTPIN, DHTTYPE);
   BH1750 lightMeter;
   AsyncWebServer server(80);
   TFT_eSPI tft = TFT_eSPI(); // Création d'une instance de l'écran TFT

   // **Variables Globales**
   float temperature = 0.0;
   float humidity = 0.0;
   float lux = 0.0;
   bool ledState = false;
   float tempThreshold = 30.0;   // Seuil de température par défaut
   float luxThreshold = 500.0;   // Seuil de luminosité par défaut

   // **Variables pour les Boutons**
   bool buttonAState = false;
   bool buttonBState = false;

   // **Configuration Initiale**
   void setup() {
     // **Initialisation des Serial**
     Serial.begin(115200);

     // **Initialisation des Broches**
     pinMode(LED_PIN, OUTPUT);
     digitalWrite(LED_PIN, LOW); // Éteindre la LED par défaut

     // **Initialisation des Boutons**
     pinMode(BUTTON_A_PIN, INPUT_PULLUP);
     pinMode(BUTTON_B_PIN, INPUT_PULLUP);

     // **Initialisation du DHT22**
     dht.begin();Étape 12 : Finaliser et Tester le Projet￼￼

​￼1. ￼￼Assurez-vous que tout fonctionne :￼￼

   - ￼￼Connexion Wi-Fi￼￼ est établie.
   - ￼￼Écran TFT￼￼ affiche les informations correctement.
   - ￼￼Capteurs DHT22￼￼ et ￼￼BH1750￼￼ fournissent des données préc

     // **Initialisation du BH1750**
     Wire.begin();
     if (lightMeter.begin(BH1750::CONTINUOUS_HIGH_RES_MODE)) {
       Serial.println("BH1750 initialisé avec succès.");
     } else {
       Serial.println("Erreur lors de l'initialisation du BH1750.");
     }

     // **Initialisation de l'Écran TFT**
     tft.init();
     tft.setRotation(1); // Ajustez en fonction de l'orientation souhaitée
     tft.fillScreen(TFT_BLACK);
     tft.setTextColor(TFT_WHITE, TFT_BLACK);
     tft.setTextSize(2);
     tft.setCursor(10, 10);
     tft.println("Initialisation...");

     // **Connexion au Wi-Fi**
     tft.setCursor(10, 40);
     tft.println("Connexion Wi-Fi...");
     WiFi.begin(ssid, password);
     Serial.print("Connexion au Wi-Fi");
     while (WiFi.status() != WL_CONNECTED) {
       delay(500);
       Serial.print(".");
       tft.print(".");
     }
     Serial.println("\nConnecté au Wi-Fi");
     tft.println("\nConnecté");
     tft.print("IP : ");
     tft.println(WiFi.localIP());

     // **Configuration des Routes du Serveur Web**

     // Route pour la page principale
     server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
       request->send(200, "text/plain", "Bienvenue sur le serveur ESP32");
     });

     // Route pour obtenir les données des capteurs
     server.on("/sensors/data", HTTP_GET, [](AsyncWebServerRequest *request){
       // Création de l'objet JSON
       DynamicJsonDocument jsonDoc(256);
       jsonDoc["temperature"] = temperature;
       jsonDoc["humidity"] = humidity;
       jsonDoc["lux"] = lux;

       // Conversion en chaîne JSON
       String jsonString;
       serializeJson(jsonDoc, jsonString);

       // Envoi de la réponse
       request->send(200, "application/json", jsonString);
     });

     // Route pour contrôler la LED
     server.on("/led", HTTP_POST, [](AsyncWebServerRequest *request){
       if (request->hasParam("state", true)) {
         String state = request->getParam("state", true)->value();
         if (state == "on") {
           digitalWrite(LED_PIN, HIGH);
           ledState = true;
           request->send(200, "text/plain", "LED allumée");
         } else if (state == "off") {
           digitalWrite(LED_PIN, LOW);
           ledState = false;
           request->send(200, "text/plain", "LED éteinte");
         } else {
           request->send(400, "text/plain", "Paramètre 'state' invalide");
         }
       } else {
         request->send(400, "text/plain", "Paramètre 'state' manquant");
       }
     });

     // Route pour définir les seuils
     server.on("/threshold", HTTP_POST, [](AsyncWebServerRequest *request){
       if (request->hasParam("temp", true)) {
         tempThreshold = request->getParam("temp", true)->value().toFloat();
       }
       if (request->hasParam("lux", true)) {
         luxThreshold = request->getParam("lux", true)->value().toFloat();
       }
       request->send(200, "text/plain", "Seuils mis à jour");
     });

     // Démarrage du serveur
     server.begin();
   }

   // **Boucle Principale**
   void loop() {
     // **Lecture des Capteurs**

     // Lecture de la température et de l'humidité
     float newTemperature = dht.readTemperature();
     float newHumidity = dht.readHumidity();

     // Vérification des lectures valides
     if (!isnan(newTemperature)) {
       temperature = newTemperature;
     }
     if (!isnan(newHumidity)) {
       humidity = newHumidity;
     }

     // Lecture de la luminosité
     float newLux = lightMeter.readLightLevel();
     if (newLux >= 0) {
       lux = newLux;
     }

     // **Lecture des Boutons**
     buttonAState = digitalRead(BUTTON_A_PIN) == LOW;
     buttonBState = digitalRead(BUTTON_B_PIN) == LOW;

     if (buttonAState) {
       // Action pour le bouton A (basculer la LED)
       ledState = !ledState;
       digitalWrite(LED_PIN, ledState ? HIGH : LOW);
       tft.setCursor(10, 110);
       tft.setTextColor(TFT_RED, TFT_BLACK);
       tft.printf("LED: %s\n", ledState ? "On" : "Off");
       delay(200); // Debounce
     }

     if (buttonBState) {
       // Action pour le bouton B (changer les seuils)
       tempThreshold += 1.0;
       luxThreshold += 50.0;
       tft.setCursor(10, 130);
       tft.setTextColor(TFT_GREEN, TFT_BLACK);
       tft.printf("Thresholds Updated\n");
       delay(200); // Debounce
     }

     // **Affichage sur l'Écran TFT**
     tft.fillRect(10, 60, 220, 40, TFT_BLACK); // Efface la zone d'affichage des capteurs
     tft.setCursor(10, 60);
     tft.setTextColor(TFT_WHITE, TFT_BLACK);
     tft.printf("Temp: %.1f C\n", temperature);
     tft.printf("Hum: %.1f %%\n", humidity);
     tft.printf("Lumi: %.1f lx\n", lux);
     tft.printf("LED: %s\n", ledState ? "On" : "Off");

     // **Gestion des Seuils**
     if (temperature > tempThreshold || lux > luxThreshold) {
       digitalWrite(LED_PIN, HIGH);
       ledState = true;
     } else {
       digitalWrite(LED_PIN, LOW);
       ledState = false;
     }

     // **Délai avant la prochaine lecture**
     delay(2000);
   }
   ```

4. **Modifier les Identifiants Wi-Fi :**

   - **Remplacez** `"Iphone de El Horri"` par le nom de votre réseau Wi-Fi (SSID).

   - **Remplacez** `"12345678"` par le mot de passe de votre réseau Wi-Fi.

   ```cpp
   const char* ssid = "NomDeVotreWiFi";             // Remplacez par votre SSID Wi-Fi
   const char* password = "VotreMotDePasse";        // Remplacez par votre mot de passe Wi-Fi
   ```

5. **Sélectionner la Carte et le Port Corrects :**

   - **Carte :** Allez dans `Outils` > `Type de carte` > `ESP32 Arduino` > **"TTGO T1"** ou **"ESP32 Dev Module"**.

   - **Port :** Allez dans `Outils` > `Port` > sélectionnez le port correspondant (par exemple, **COM3** sous Windows).

6. **Vérifier le Code :**

   - **Cliquez** sur le bouton **Vérifier** (icône en forme de coche) pour compiler le code.
   
   - **Assurez-vous** qu'il n'y a pas d'erreurs de compilation.

   - **Si des erreurs apparaissent**, vérifiez que toutes les bibliothèques nécessaires sont installées :

     - **WiFi**
     - **AsyncTCP**
     - **ESPAsyncWebServer**
     - **ArduinoJson**
     - **DHT**
     - **BH1750**
     - **TFT_eSPI**

7. **Téléverser le Code :**

   - **Cliquez** sur le bouton **Téléverser** (icône en forme de flèche droite).

   - **Remarque :** Si le téléversement échoue, appuyez et maintenez le bouton **BOOT** ou **FLASH** sur le TTGO T-Display pendant le téléversement.

8. **Vérifier l'Affichage et les Fonctionnalités :**

   - **Écran TFT** : Devrait afficher les messages d'initialisation, la connexion Wi-Fi, l'adresse IP, les lectures des capteurs, et l'état de la LED.

   - **Boutons Intégrés** :
     - **Bouton A** : Appuyez pour basculer la LED.
     - **Bouton B** : Appuyez pour augmenter les seuils de température et de luminosité.

   - **API RESTful** :
     - **Page Principale** : Ouvrez un navigateur et allez à `http://[adresse_IP]/` pour voir le message de bienvenue.
     - **Données des Capteurs** : Allez à `http://[adresse_IP]/sensors/data` pour obtenir les données des capteurs en JSON.
     - **Contrôle de la LED** : Utilisez Postman ou `curl` pour envoyer des requêtes POST à `http://[adresse_IP]/led` avec `state=on` ou `state=off`.
     - **Définir les Seuils** : Envoyez des requêtes POST à `http://[adresse_IP]/threshold` avec les paramètres `temp` et/ou `lux`.

### **Étape 9 : Tester l'API RESTful**

1. **Accéder à la Page Principale :**

   - **Ouvrez** un navigateur web.

   - **Entrez** l'adresse IP affichée sur l'écran TFT (par exemple, `http://192.168.1.100/`).

   - **Vous devriez voir** le message **"Bienvenue sur le serveur ESP32"**.

2. **Obtenir les Données des Capteurs :**

   - **Allez** à `http://[adresse_IP]/sensors/data` dans votre navigateur.

   - **Vous devriez voir** un JSON avec les valeurs des capteurs :

     ```json
     {
       "temperature": 25.3,
       "humidity": 60.5,
       "lux": 300.0
     }
     ```

3. **Contrôler la LED via l'API :**

   - **Allumer la LED :**

     - Utilisez **Postman** ou **curl** :

       ```bash
       curl -X POST http://[adresse_IP]/led -d "state=on"
       ```

   - **Éteindre la LED :**

     - Utilisez **Postman** ou **curl** :

       ```bash
       curl -X POST http://[adresse_IP]/led -d "state=off"
       ```

4. **Définir les Seuils via l'API :**

   - **Exemple :**

     ```bash
     curl -X POST http://[adresse_IP]/threshold -d "temp=25&lux=300"
     ```

   - **Cela définira** le seuil de température à 25°C et le seuil de luminosité à 300 lx.

### **Étape 10 : Utiliser les Boutons Intégrés**

1. **Bouton A :** Appuyez pour basculer l'état de la LED. L'écran devrait afficher l'état mis à jour.

2. **Bouton B :** Appuyez pour augmenter les seuils de température et de luminosité. L'écran affichera un message indiquant que les seuils ont été mis à jour.

### **Étape 11 : Dépannage des Problèmes Courants**

#### **1. ESP32 Ne se Connecte Pas au Wi-Fi**

- **Vérifiez les Informations Wi-Fi :**
  - Assurez-vous que le **SSID** et le **mot de passe** sont corrects.
  - Vérifiez la **casse** (majuscules/minuscules) des caractères.

- **Compatibilité des Fréquences :**
  - L'ESP32 fonctionne sur **2.4 GHz**. Assurez-vous que votre hotspot iPhone est configuré sur cette bande de fréquence.

- **Paramètres de Sécurité :**
  - Si votre hotspot utilise **WPA3**, essayez de le configurer sur **WPA2**.

- **Tester avec un Autre Réseau :**
  - Connectez l'ESP32 à un autre réseau Wi-Fi (comme un routeur domestique) pour vérifier si le problème persiste.

#### **2. Écran TFT Reste Noir**

- **Vérifiez les Connexions Physiques :**
  - Assurez-vous que toutes les broches de l'écran TFT sont correctement connectées.
  - Vérifiez l'alimentation en **3V3** et la masse (**GND**).

- **Revérifiez la Configuration de `User_Setup_ttgo_T_Display.h` :**
  - Assurez-vous que les broches définies dans le fichier de configuration correspondent aux connexions matérielles.

- **Tester avec un Sketch de Test :**
  - Téléversez un sketch de test simple pour vérifier l'affichage.
  - Utilisez l'exemple `GraphicTest` de la bibliothèque **TFT_eSPI** :
    - Allez dans `Fichier` > `Exemples` > `TFT_eSPI` > `GraphicTest`.
  
- **Réinitialisez le TTGO :**
  - Appuyez sur le bouton **RESET** pour redémarrer le module.

#### **3. Erreur `[BH1750] device not configured`**

- **Vérifiez les Connexions Physiques :**
  - Assurez-vous que **SDA** et **SCL** sont correctement connectés aux broches **GPIO21** et **GPIO22**.
  - Assurez-vous que le **BH1750** est alimenté en **3V3** et que les câbles sont bien insérés.

- **Vérifiez l'Adresse I2C :**
  - Utilisez un **scanner I2C** pour vérifier l'adresse du **BH1750**.
  - Modifiez le code d'initialisation si l'adresse est différente.

- **Bibliothèque BH1750 :**
  - Assurez-vous que vous utilisez une bibliothèque **BH1750** compatible et à jour.

- **Tester avec un Sketch de Test :**
  - Utilisez le **sketch de test du BH1750** fourni à l'Étape 6.

#### **4. LED Ne Fonctionne Pas**

- **Vérifiez la Connexion de la LED :**
  - Assurez-vous que la **LED** est correctement connectée avec la résistance de **220Ω**.
  - Vérifiez que la broche **GPIO2** contrôle bien la LED.

- **Tester la LED :**
  - Téléversez un **sketch simple** pour allumer et éteindre la LED.

    ```cpp
    void setup() {
      pinMode(2, OUTPUT);
    }

    void loop() {
      digitalWrite(2, HIGH); // Allumer la LED
      delay(1000);
      digitalWrite(2, LOW);  // Éteindre la LED
      delay(1000);
    }
    ```

#### **5. ESP32 Ne Répond Pas à l'API**

- **Vérifiez la Connexion Wi-Fi :**
  - Assurez-vous que l'ESP32 est **connecté** au réseau Wi-Fi.
  - Vérifiez l'**adresse IP** affichée sur l'écran TFT.

- **Vérifiez les Routes de l'API :**
  - Assurez-vous que les routes `/sensors/data`, `/led`, et `/threshold` sont correctement définies dans le code.

- **Utilisez le Moniteur Série :**
  - Ouvrez le **Moniteur Série** (`Outils` > `Moniteur Série`) à **115200 bauds** pour voir les messages de débogage et identifier les erreurs potentielles.

### **Étape 12 : Finaliser et Tester le Projet**

1. **Assurez-vous que tout fonctionne :**
    
    - La connexion Wi-Fi est établie.
    - L'écran TFT affiche les informations correctement.
    - Les capteurs DHT22 et BH1750 fournissent des données précises.
    - La LED peut être contrôlée via l'API et les boutons intégrés.
    - Les seuils fonctionnent pour automatiser le contrôle de la LED.
2. **Documentez Votre Projet :**
    
    - Prenez des photos des branchements.
    - Notez les configurations et les paramètres utilisés.
    - Décrivez les fonctionnalités et comment les utiliser.
3. **Présentez Votre Projet :**
    
    - Préparez une présentation simple pour expliquer le fonctionnement.
    - Montrez comment interagir avec l'API et utiliser les boutons.

## 🎉 **Conclusion**

Votre projet est ambitieux et couvre de nombreuses fonctionnalités intéressantes, telles que la création d'une API RESTful, la gestion des capteurs, le contrôle de la LED, et l'affichage des données sur l'écran TFT. En suivant attentivement les étapes décrites dans cette todo list, vous devriez être en mesure de résoudre les problèmes actuels et de compléter votre projet avec succès.

### **Bon Travail et Bonne Chance ! 🚀**

Si vous avez besoin d'aide supplémentaire ou rencontrez d'autres problèmes, n'hésitez pas à me le faire savoir. Je suis là pour vous aider !
