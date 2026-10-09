# [Hardware Design] Carte Électronique de Contrôle — Robot Autonome Tri-Canettes

Ce dépôt rassemble les fichiers de conception matérielle de la carte de contrôle dédiée à un robot autonome de collecte et de tri de déchets. 

La carte centralise la gestion de l'alimentation (Li-Ion), le pilotage des moteurs DC et l'acquisition des capteurs de navigation sur bus partagés.

📌 **Statut du projet :** 
* 🟧 **Routage PCB (KiCad) :** En cours de réalisation.

---

## 🗺️ Architecture Système 

Le schéma architectural qu'on a conçu pour la carte :

<p align="center">
<img width="800" alt="Schéma fonctionnel" src="https://github.com/user-attachments/assets/a17226a6-bb5c-43a6-9627-ac4e2321dc44" />
</p> 

---

## 🛠️ Spécifications & Choix des Composants

### 1. Unité de Traitement & Logique (MCU)
* **Microcontrôleur :** **STM32G431RBT6** (Cortex-M4, 170 MHz). Sélectionné pour ses timers avancés (gestion des moteurs et servo) et sa large connectivité matérielle.

### 2. Gestion de l'Énergie (Power Management)
* **Contrôleur de Charge :** **bq25896RTWR** (Switching Charger). Assure la charge rapide de la batterie Lithium-Ion via USB-C et la gestion dynamique du chemin de puissance (Power Path).
* **Jauge de Batterie :** **bq27220** (Fuel Gauge). Suivi précis de l'état de charge (SoC) via le bus I²C2.
* **Régulation :** LDO **AP2112K-3.3** (600mA) pour l'alimentation stable de la section logique et des capteurs.

### 3. Contrôle des Actionneurs (Motor Drive)
* **Driver Moteurs :** **DRV8833** (Double Pont en H) pour le pilotage de 2 motoréducteurs CC (CH-N20-3) via signaux PWM (`TIM1` et `TIM3`).
* **Servomoteur :** Sortie dédiée pour un **Servo MG90S** (Pince de préhension) piloté par le timer `TIM15`.

### 4. Acquisition Capteurs (Sensors)
Répartition sur trois bus physiques I²C indépendants pour éviter la saturation et les conflits d'adresses :
* **Bus I²C1 :** Centrale inertielle IMU **MPU-6050** (Odométrie).
* **Bus I²C2 :** Section Power Management (**bq27220** et **bq25896**).
* **Bus I²C3 :** Télémètres Laser ToF **VL53L0X** (Évitement d'obstacles) et capteur de couleur/proximité **APDS9960** (Tri des canettes). 
  * *Note de conception :* Les broches `XSHUT` des VL53L0X sont routées vers des GPIOs du STM32 pour configurer leurs adresses de manière logicielle et séquentielle.

---

## 📄 Dossier de Conception (Schématiques)

### Étage Microcontrôleur (MCU)
<p align="center">
<img width="700" alt="MCU" src="https://github.com/user-attachments/assets/f04841df-3208-4843-a7be-add28e1a69e5" />
</p> 

### Gestion d'Énergie (Power Management)
<p align="center">
<img width="700" alt="Power Management" src="https://github.com/user-attachments/assets/d4b6c70a-bada-430d-b90c-8217d9fbbf3a" />
</p> 

### Bloc Capteurs (Sensors)
<p align="center">
<img width="700" alt="Sensors" src="https://github.com/user-attachments/assets/32cf09ca-8c01-4ea6-8fae-80763e91b008" />
</p>

### Commande Moteurs (Motor Drive)
<p align="center">
<img width="700" alt="Motor Drive" src="https://github.com/user-attachments/assets/166a9b3e-42f0-486b-bd58-d52d4fa22434" />
</p>

---

## 📂 Organisation du Dépôt

```text
├── 📂 Hardware/               # Fichiers sources CAO KiCad (.kicad_sch, .kicad_pcb en cours)
├── 📂 Documents/              # Livrables et visuels techniques
│   ├── 📄 Schematic_Print.pdf # Schématique complet exporté en haute définition PDF
└── 📄 README.md
```
