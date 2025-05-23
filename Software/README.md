# Software
In dit document vind je informatie over hoe je zelf sensoren kan toevoegen, hoe je home assistant instelt en meer informatie over de protocollen die je kan gebruiken.

## Inhoudsopgave

### Sensoren toevoegen
- [Protocollen](#protocollen)
- [Voorbeelden](#voorbeelden)


### Home Assistant Dashboard
- [Installatie](#stap-1-home-assistant-downloaden)
- [Dashboard maken](#stap-3-een-nieuw-dashboard-toevoegen)
- [Kaarten toevoegen](#stap-5-kaarten-toevoegen-aan-je-dashboard)
- [Automatiseringen](#stap-6-automatiseringen-en-scripts-toevoegen)
- [Personaliseren](#stap-7-dashboard-personaliseren)
- [Externe toegang](#stap-8-dashboard-beschikbaar-maken-op-externe-apparaten)
- [Ons Dashboard](#Ons-Dashboard)

### ESP installeren
- [Wat?](#Uitleg)
- [Benodigdheden](#Benodigdheden)
- [Protocol](#Protocol)
## Sensoren toevoegen

### Wat?
Dit is een handleiding voor het toevoegen van nieuwe sensoren aan Home Assistant. We tonen stap voor stap met tekst en afbeeldingen hoe je dit eenvoudig doet.

---

### Benodigdheden

- Home Assistant
- Home Assistant URL
- Adapter (Zigbee, Z-Wave, ...)
- Computer
- Internetverbinding
- Sensoren
- Gezond verstand

---

### Protocollen

- [Zigbee](#zigbee-stappenplan)
- [Wi-Fi](#wi-fi-stappenplan)
- [Z-Wave](#z-wave-stappenplan)
- [Bluetooth](#bluetooth-stappenplan)
- [Thread](#thread-stappenplan)
- [Probleemoplossing](#optimalisatie-en-probleemoplossing)

#### Zigbee: Stappenplan

1. Download de Zigbee2MQTT add-on.
2. Open Home Assistant via `http://homeassistant.local:8123`.
3. Ga naar de [Github-pagina van Koenkk](https://github.com/zigbee2mqtt/hassio-zigbee2mqtt#installation).
4. Voeg de repository toe via "Add add-on repository to my home assistant".
5. Ga naar **Instellingen > Add-On Store** en installeer Zigbee2MQTT.
6. Activeer "Start bij opstarten" en "Weergeven in zijbalk".
7. Open **Zigbee2MQTT Web UI** en zet "Aanmelden toestaan" op "alles".
8. Zet je sensor in paring-modus en deze zal verschijnen in de interface.

---

#### Wi-Fi: Stappenplan

##### Benodigdheden
- Home Assistant (op Raspberry Pi, mini-pc of NAS)
- 2.4GHz Wi-Fi-router
- Wi-Fi-compatibele smarthome-apparaten (bv. Shelly, Sonoff, TP-Link)

##### Installatie

**Methode 1: Automatische installatie via Home Assistant**
1. Ga naar **Instellingen > Integraties**.
2. Zoek de integratie van jouw apparaat en installeer deze.
3. Volg de instructies om het apparaat toe te voegen.

**Methode 2: Handmatige installatie via ESPHome**
1. Ga naar **Instellingen > Integraties > ESPHome**.
2. Voeg de volgende YAML-code toe:

```yaml
esphome:
  name: mijn-wifi-apparaat
esp8266:
  board: nodemcuv2
wifi:
  ssid: "Jouw-WiFi-Naam"
  password: "Jouw-WiFi-Wachtwoord"
  manual_ip:
    static_ip: 192.168.1.100
    gateway: 192.168.1.1
    subnet: 255.255.255.0
logger:
api:
ota:
```
3. Upload de configuratie en wacht tot de installatie voltooid is.

---

#### Z-Wave: Stappenplan

1. Sluit de Z-Wave dongle aan op je apparaat.
2. Ga naar [Home Assistant Z-Wave integratie](https://www.home-assistant.io/integrations/zwave_js/).
3. Open Home Assistant en ga naar **Instellingen > Apparaten & Diensten**.
4. Voeg de **Z-Wave JS integratie** toe en volg de instructies.
5. Voeg een nieuw apparaat toe via **Apparaat toevoegen**.
6. Volg de instructies op het scherm en voltooi de configuratie.

---

#### Bluetooth: Stappenplan

**Methodes:**
1. Bluetooth Proxy (ESP32)
2. Lokale Bluetooth-adapter

##### Installatie met ESPHome

```yaml
esphome:
  name: bluetooth-proxy
esp32:
  board: esp32dev
  framework:
    type: esp-idf
esp32_ble_tracker:
  scan_parameters:
    interval: 1100ms
    window: 1100ms
    active: true
bluetooth_proxy:
  active: true
logger:
api:
ota:
```

1. Upload de configuratie naar ESP32.
2. Ga naar **Instellingen > Apparaten & Diensten** en voeg Bluetooth toe.

---

#### Thread: Stappenplan

1. Controleer of je installatie Thread ondersteunt (bv. HomePod Mini, Google Nest Hub, Home Assistant Skyconnect).
2. Voeg een **Thread Border Router** toe.
3. Ga naar **Instellingen > Integraties > Thread** en configureer.
4. Voeg een **Thread-apparaat** toe via **Nieuw apparaat toevoegen**.

---

## Optimalisatie en Probleemoplossing

- **Wi-Fi:** Gebruik een apart 2.4GHz-netwerk voor smarthome-apparaten.
- **Z-Wave:** Verplaats de Z-Wave-stick niet bij het toevoegen van apparaten.
- **Bluetooth:** Gebruik een USB 2.0-poort om storingen te vermijden.
- **Thread:** Voeg extra apparaten toe voor een sterker mesh-netwerk.

---
Voor meer informatie over hoe je sensoren moet toevoegen, kan dit onderste bestand je helpen.

[Sensoren Toevoegen.pdf](https://github.com/user-attachments/files/19482208/Sensoren.Toevoegen.pdf.pdf)




## Voorbeelden

### Eigen sensoren toevoegen via ESP32


### Inhoudstafel
- [BME280 (milieu-sensor)](#installatie-van-de-bme280)
- [CO₂-sensor MH-Z19](#Installatie-van-de-CO2-sensor-MH-Z19)
- [Ultrasone Sensor](#Ultrasone-Sensor)

---

### Installatie van de BME280

#### Benodigdheden:
- Microcontroller: **ESP32-C3-DevKitC-02**
- BME280 sensor (I²C-variant)
- Arduino IDE  
- Jumper wires
- Google Chrome

---


**Pin-out van de microcontroller:** 

<img src="/images/esp_overzicht.png" style="width: 400px; height: auto;">

#### Verbinding:

| **BME280 Pin** | **ESP32-C3 GPIO** | **Naam op bord** | **Opmerking**               |
|----------------|-------------------|------------------|-----------------------------|
| VCC            | 3V3               | 3V3              | Enkel 3.3V gebruiken        |
| GND            | GND               | GND              | Aarde                       |
| SCL            | GPIO4             | I2C_SCL          | I²C Clock                   |
| SDA            | GPIO5             | I2C_SDA          | I²C Data                    |




#### 1) ESPHome installeren
Ga naar het tab **Instellingen** -> **Add-ons** -> **Add-on winkel** -> **Zoek naar ESPHome Home Device Builder** 
- Druk op "**Installeren**" en zet de optie "**Start bij opstarten aan**
- Herstart Home Assistant

#### 2) ESP32 toevoegen aan ESPHome
- Ga naar het tab genaamd "**ESPHomeBuilder**"
- Druk op "**+ New Device**"
- Geef het apparaat een naam en kies bij device type de "**ESP32-C3**"
- Kies de optie "**Plug into this computer**"
- **Verbind** je ESP32 via een usb kabel met je computer
- Druk op "**connect**"
- Nu is je "**ESP32**" verbonden

#### 3) Pas de ESP32 code aan 
- Druk op de knop **edit** bij ESPHome van je ESP32
- Pas je code aan voor je wifi netwerk en pas deze regel aan: 

 ````yaml
  wifi:
  ssid: JouwWifiNaam
  password: JouwWifiWacthwoord
  ````
-  
#### 4) Sensor toevoegen

- Voeg de code toe via de "**edit**" button
- Code :

  ````yaml
  i2c:
  sda: 5
  scl: 4
  scan: true

  sensor:
  - platform: bme280_i2c
    temperature:
      name: "BME280 Temperature"
    pressure:
      name: "BME280 Pressure"
    humidity:
      name: "BME280 Humidity"

  ````
- Nu kan je de sensor in je dashboard gebruiken, door het toe te voegen via de eniteiten 




### Installatie van de CO2-sensor MH-Z19

#### Benodigdheden:
- Microcontroller: **ESP32-C3-DevKitC-02**
- CO₂-sensor **MH-Z19**
- Arduino IDE  
- Jumper wires
- Google Chrome

---

**Pin-out van de microcontroller:**  

<img src="/images/esp_overzicht.png" style="width: 400px; height: auto;">




#### Verbinding:

| **MH-Z19 Pin** | **ESP32-C3 GPIO** | **Naam op bord** | **Opmerking**                         |
|----------------|-------------------|------------------|---------------------------------------|
| VCC            | 5V                | 5V               | Sensor werkt op 5V                    |
| GND            | GND               | GND              | Aarde                                 |
| TX             | GPIO7             | IO7              | Naar RX van ESP32 (seriële ingang)   |
| RX             | GPIO6             | IO6              | Naar TX van ESP32 (seriële uitgang)  |

---

#### 1) ESPHome installeren
Ga naar het tab **Instellingen** → **Add-ons** → **Add-on winkel** → **Zoek naar ESPHome Home Device Builder**  
- Druk op "**Installeren**" en zet de optie "**Start bij opstarten**" aan  
- Herstart Home Assistant

---

#### 2) ESP32 toevoegen aan ESPHome
- Ga naar het tab genaamd "**ESPHomeBuilder**"
- Druk op "**+ New Device**"
- Geef het apparaat een naam en kies bij device type de "**ESP32-C3**"
- Kies de optie "**Plug into this computer**"
- **Verbind** je ESP32 via een USB-kabel met je computer
- Druk op "**connect**"
- Nu is je "**ESP32**" verbonden

---

### 3) Pas de ESP32-code aan
- Druk op de knop **edit** bij ESPHome van je ESP32
- Pas je code aan voor je WiFi-netwerk en pas deze regel aan:  

```yaml
wifi:
  ssid: JouwWifiNaam
  password: JouwWifiWachtwoord
```

### 4) Sensor toevoegen
- Voeg de code toe via de "**edit**" button
- Code :

  ````yaml
  uart:
    tx_pin: 6
    rx_pin: 7
    baud_rate: 9600

  sensor:
    - platform: mhz19
      co2:
        name: MH-Z19 CO2 Value
      temperature:
        name: MH-Z19 Temperature
  ````
- Nu kan je de sensor in je dashboard gebruiken, door het toe te voegen via de eniteiten 

### Ultrasone Sensor

#### Benodigdheden:
- Microcontroller: **ESP32-C3-DevKitC-02**
- HC-SR04 Ultrasone sensor
- Arduino IDE  
- Jumper wires
- Google Chrome

---


**Pin-out van de microcontroller:** 


#### Verbinding:

| **HC-SR04**    | **ESP32-C3 GPIO** | **Naam op bord** | **Opmerking**               |
|----------------|-------------------|------------------|-----------------------------|
| VCC            | 5V                | 5V               | Enkel 5V gebruiken          |
| GND            | GND               | GND              | Aarde                       |
| Trig           | GPIO2             | D2               | GPIO pin                    |
| Echo           | GPIO5             | D5               | GPIO pin                    |

#### Arduino IDE

Ga naar arduino en download de laatste versie van de software.
Download ook de wifi.h en PubSubClient.h library.
Zet daarna deze code in en flash deze op de ESP32 C3.

````Cpp
#include <WiFi.h>
#include <PubSubClient.h>

const int trigPin = 5;
const int echoPin = 18;

const char* ssid = "Smart Greenhouse";
const char* password = "admin123";
const char* mqtt_server = "192.168.1.105";
const char* mqtt_user = "esp32";
const char* mqtt_pass = "esp32pass";

#define MAX_HEIGHT_CM 15.5
#define MAX_VOLUME_LITER 0.473

WiFiClient espClient;
PubSubClient client(espClient);

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Verbinden met ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi verbonden");
  Serial.println("IP adres: ");
  Serial.println(WiFi.localIP());
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Verbinding maken met MQTT...");
    if (client.connect("ESP32C6Client", mqtt_user, mqtt_pass)) {
      Serial.println("Verbonden met MQTT broker");
    } else {
      Serial.print("Mislukt, rc=");
      Serial.print(client.state());
      Serial.println(" opnieuw proberen over 5 seconden");
      delay(5000);
    }
  }
}

void setup() {
  Serial.begin(115200);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  setup_wifi();
  client.setServer(mqtt_server, 1883);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  // Ultrasoon meten
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  long duration = pulseIn(echoPin, HIGH);
  float distance = duration * 0.034 / 2;

  // Hoogte water = totale hoogte - gemeten afstand
  float waterHeight = MAX_HEIGHT_CM - distance;
  if (waterHeight < 0) waterHeight = 0;
  if (waterHeight > MAX_HEIGHT_CM) waterHeight = MAX_HEIGHT_CM;

  float liters = (waterHeight / MAX_HEIGHT_CM) * MAX_VOLUME_LITER;

  // Debug via serial monitor
  Serial.print("Gemeten afstand: ");
  Serial.print(distance);
  Serial.println(" cm");

  Serial.print("Inhoud vat: ");
  Serial.print(liters, 3);
  Serial.println(" liter");

  // Versturen via MQTT
  char msg_cm[16];
  dtostrf(distance, 6, 2, msg_cm);
  client.publish("sensor/afstand/cm", msg_cm);

  char msg_liter[16];
  dtostrf(liters, 6, 3, msg_liter);
  client.publish("sensor/vat/liter", msg_liter);

  delay(2000);
}
````



#### 1) ESPHome installeren
Ga naar het tab **Instellingen** -> **Add-ons** -> **Add-on winkel** -> **Zoek naar ESPHome Home Device Builder** 
- Druk op "**Installeren**" en zet de optie "**Start bij opstarten aan**
- Herstart Home Assistant

#### 2) ESP32 toevoegen aan ESPHome
- Ga naar het tab genaamd "**ESPHomeBuilder**"
- Druk op "**+ New Device**"
- Geef het apparaat een naam en kies bij device type de "**ESP32-C3**"
- Kies de optie "**Plug into this computer**"
- **Verbind** je ESP32 via een usb kabel met je computer
- Druk op "**connect**"
- Nu is je "**ESP32**" verbonden

#### 3) Pas de ESP32 code aan 
- Druk op de knop **edit** bij ESPHome van je ESP32
- Pas je code aan voor je wifi netwerk en pas deze regel aan: 

 ````yaml
  wifi:
  ssid: JouwWifiNaam
  password: JouwWifiWacthwoord
  ````
-  
#### 4) Sensor toevoegen

- Voeg de code toe via de "**edit**" button
- Code :


````yaml
sensor:
  - platform: ultrasonic
    trigger_pin: D1
    echo_pin: D2
    name: "Ultrasonic Distance"
    update_interval: 2s
````
- Nu kan je de sensor in je dashboard gebruiken, door het toe te voegen via de eniteiten 




# Home Assistant Dashboard Maken

## Stap 1: Home Assistant Downloaden
Als je Home Assistant nog niet hebt gedownload, doe dat op deze manier:
- **Raspberry Pi:** Gebruik Home Assistant OS.
- **Docker:** Gebruik Home Assistant Container-versie.
- **Windows/macOS/Linux:** Installeer via een virtuele machine.

Meer informatie: [Home Assistant Installatie](https://www.home-assistant.io/installation/)

---

## Stap 2: Inloggen en Basisconfiguratie
1. Open Home Assistant in je browser via:  
   `http://homeassistant.local:8123`
2. Log in met je Home Assistant-account.
3. Ga naar **Instellingen > Dashboard**.

---

## Stap 3: Een Nieuw Dashboard Toevoegen
1. Ga naar **Instellingen > Dashboard**.
2. Klik rechtsboven op de drie puntjes en kies **Dashboard bewerken**.
3. Klik op **+ Dashboard toevoegen**.
4. Voer een **naam** in voor het dashboard.
5. Kies een type dashboard:
   - **Beheerd** (Automatisch kaarten toevoegen).
   - **Handmatig** (Zelf controle over de lay-out).
6. Klik op **Opslaan**.

---

## Stap 4: Weergaven (Tabbladen) Maken
Een weergave is een pagina binnen je dashboard.
1. Klik op **+ Weergave toevoegen**.
2. Geef de weergave een **naam**.
3. Kies een **pictogram** (bijv. `mdi:home` voor een huis).
4. Kies een **indeling**:
   - **Raster** (kaarten worden netjes uitgelijnd).
   - **Muur** (vrij positioneren van kaarten).
   - **Paneel** (één grote kaart over de hele breedte).
5. Klik op **Opslaan**.
---

## Stap 5: Kaarten Toevoegen aan je Dashboard
Kaarten zijn de widgets van Home Assistant. Volg deze stappen om een kaart toe te voegen:
1. Open een weergave en klik op **+ Kaart toevoegen**.
2. Kies een **kaarttype**, bijvoorbeeld:
   - **Entiteitenkaart** (Lijst met apparaten en sensoren).
   - **Schakelaar** (Knop om apparaten aan/uit te zetten).
   - **Knop** (Voor snelle bediening van een enkel apparaat).
   - **Grafiek** (Voor temperatuur- of luchtvochtigheidsgegevens).
   - **Weerkaart** (Toont het weer van je locatie).
3. Selecteer de **entiteit** (bijvoorbeeld `sensor.binnen_temperatuur`).
4. Pas de instellingen aan en klik op **Opslaan**.

## YAML Voorbeeld: Knopkaart
```yaml
type: button
entity: switch.lamp_woonkamer
name: Woonkamerlamp
icon: mdi:lightbulb 
tap_action:
  action: toggle
```
---

## Stap 6: Automatiseringen en Scripts Toevoegen
Maak je dashboard interactiever met automatiseringen.
1. Ga naar **Instellingen > Automatiseringen & Scripts**.
2. Klik op **+ Nieuw Automatisering**.
3. Stel een **trigger** in.
4. Voeg een **actie** toe.
5. Klik op **Opslaan**.

### YAML Voorbeeld: Automatisering
```yaml
alias: "Verlichting aan bij zonsondergang"
trigger:
  - platform: sun
    event: sunset
action:
  - service: light.turn_on
    target:
      entity_id: light.woonkamer
```

---

## Stap 7: Dashboard Personaliseren

### Kleuren en Thema’s Wijzigen
1. Ga naar **Instellingen > Profiel**.
2. Kies een **thema**.
3. Klik op **Opslaan**.

### Achtergrondafbeelding Instellen
1. Ga naar je dashboard en klik op **Dashboard bewerken**.
2. Open **Thema-instellingen**.
3. Voeg een aangepaste YAML-code toe:
   ```yaml
   background: "center / cover no-repeat url('/local/background.jpg') fixed"
   ```
4. Upload je afbeelding naar `/config/www/` en herstart Home Assistant.

### Eigen CSS en JavaScript Gebruiken
- Gebruik **HACS** om custom cards toe te voegen.
- Voeg **Lovelace UI-Mode** toe voor geavanceerde aanpassingen.

---

## Stap 8: Dashboard Beschikbaar Maken op Externe Apparaten
Wil je jouw dashboard bekijken op je telefoon of tablet? 
- Installeer de **Home Assistant Companion App** (Android/iOS).
- Gebruik **Nabu Casa**, **DuckDNS** of **Tailscale** voor externe toegang.

## Ons Dashboard
## Onze configuration file

1. Ga naar instellingen.
2. Ga Add-ons.
3. Zoek de add on file editor en installeer deze.
4. Vink de optie weergeven in zijbalk aan.
5. Herstart Home Assistant.
6. Daarna zie je in de zijbalk file editor.
7. Gebruik dan deze [Configuratie code](/Software/configuration.yaml) en plak dit in de file editor.
   
## Onze raw configuration file

Wil je ons dashboard gebruiken op Home Assistant gebruik dan dit stappenplan.
Je doet dit door de raw configuration editor te gebruiken. 
1. Druk op het potlood rechtsboven.
2. Druk daarna op de 3 punten.
3. Selecteer raw configuration editor.
4. Plak dan deze code in de raw configuration editor.--> Code voor [Dashboard](/Software/dashboard.yaml)
5. Je zult wel de entiteiten moeten aanpassen.
6. Ga terug naar instellingen en druk op 3 puntjes rechtsboven.
7. Druk op herlaad YAML-configuratie.
7. Om het watervat werkend te krijgen zul je ook deze helpers moeten instellen.![image](https://github.com/user-attachments/assets/ee7db8eb-0936-46d6-b6c9-4d98619aee1a)

### Helpers aanmaken voor het vat
1. Ga naar Instellingen.
2. Ga naar apparaten en diensten.
3. Ga naar het tablad Helpers.
4. Druk op de blauwe knop helper aanmaken.

### De Balk breedte
1. Stel een minimumwaarde in van 0.
2. Stel een maximumwaarde in van 300.
3. Weergavemodus: invoerveld.
4. Stapgrootte: 0.1.
5. Meeteenheid: cm.

### Balk Vat lengte
1. Stel een minimumwaarde in van 0.
2. Stel een maximumwaarde in van 300.
3. Weergavemodus: invoerveld.
4. Stapgrootte 0.1.
5. Meeteenheid: cm.

### Diameter vat
1. Stel een minimumwaarde in van 0.
2. Stel een Maximumwaarde in van 300.
3. Weergavemodus: Invoerveld
4. Stapgrootte: 0.1
5. Meeteenheid: cm
   
### Hoogte vat
1. Stel een minimumwaarde in van 0.
2. Stel een Maximumwaarde in van 300.
3. Weergavemodus: Invoerveld
4. Stapgrootte: 0.1
5. Meeteenheid: cm

### Input_select
1. Opties: Cilinder
         : Balk

### Inhoud Vat Dynamisch
1. Stel meeteenheid L in
2. Precisie weergeven: standaard (0,0)
---
### ESP32-C6 Zigbee (Compatible firmware)
#### Uitleg
De ESP32-C6 is een uitstekende keuze voor Home Assistant, dankzij de ondersteuning voor Wi-Fi 6, Bluetooth 5 (LE), Zigbee en Thread. Hierdoor kan het werken als een sensor/aandrijving binnen je smart home.
Dit is een handleiding voor het toevoegen van nieuwe sensoren aan Home Assistant.

#### Benodigdheden
##### Algemene Benodigdheden
* ESP32-C6-DevKitM-1 – Het ontwikkelbord zelf
* USB-C kabel - Voor voeding en programmering
* Computer (Windows, macOs, of Linux) - Voor programeren
* ESPHome of ESP-IDF - Software om de ESP32-C6 te programmeren
* 5V voeding (optioneel) - Indien je het bord los van een PC wil voeden

- Voor Home Assistant (SmartHome)
* Home Assistant installatie - Op een Rasberry Pi, server of NAS
* ESPHome (geïnstalleerd in Home Assistant) - Voor makkelijke configuratie
* ZIgbee2MQTT (indien Zigbee-Coördinator) - Voor Zigbee-apparaten
* OpenThread Border Router (indien Thread gebruikt wordt)

#### Protocol
##### Stappenplan
- Stap 1: ESPHome installeren in Home Assistant
1. Open Home Assistant
2. Ga naar Instelllingen > Add-ons > Add-on Store
3. Zoek naar ESPHome en klik op Installeren
4. Na installatie, klik op Starten
5. Vink "Toon in zijbalk" aan voor snelle toegang

- Stap 2: ESP32-C6 flashen met ESPHome
1. Sluit de ESP32-C6 met een USB-C kabel aan op je computer
2. Open ESPHome in Home Assistant
3. Klik op + Nieuw Apparaat
4. Kies Handmatig en geef een naam (bijv. "ESP-C6-Zigbee")
5. Kies ESP32-C6 als bordtype
6. Klik op Installeren en kies "Verbinden via Serieel"
7. Selecteer de juiste poort (/dev/ttyUSB0 of COM3) en installeer de firmware

- Stap 3: Wi-Fi Instellen
1. Zodra de ESP32-C6 is geflasht, start hij in Wi-Fi-configuratiemodus
2. Ga op je telefoon/computer naar Wi-Fi-instellingen
3. Verbind met het netwerk ESPHome-xxxxxx
4. Open een browser en ga naar http://192.168.4.1
5. Selecteer je Wi-Fi-netwerk, voer het wachtwoord in en klik op Opslaan

- Stap 4: Zigbee-coördinator instellen met Zigbee2MQTT
De ESP32-C6 heeft een ingebouwde Zigbee-chip, maar je moet deze activeren als Zigbee-coördinator. Dit doe je met Zigbee2MQTT of ZHA.
Optie 1: Zigbee2MQTT installeren
1. Ga in Home Assistant naar Instellingen > Add-ons > Add-on Store
2. Zoek naar Zigbee2MQTT en klik op Installeren
3. Na installatie, open Zigbee2MQTT Configuratie
4. Voeg de ESP32-C6 als coördinator toe met deze instellingen:
```yaml
serial:
  port: tcp://IP-VAN-ESP32-C6:6638
  adapter: ezsp
```
(Vervang "IP-VAN-ESP32-C6" met het IP-adres van je ESP32-C6, dit kun je vinden in je router of ESPHome logboek).
5. Klik op Opslaan en Herstart Zigbee2MQTT

- Stap 5: Zigbee-apparaten toevoegen
1. Ga naar Zigbee2MQTT > Instellingen
2. Klik op Apparaat toevoegen (Pairing Mode)
3. Zet je Zigbee-apparaat in koppelmodus (volgens de handleiding van het apparaat)
4. Zodra het apparaat verschijnt in Zigbee2MQTT, klik op Opslaan

- Stap 6: Zigbee-apparaten integreren in Home Assistant
1. Ga naar Instellingen > Apparaten & Diensten
2. Zoek Zigbee2MQTT en klik op Configureren
3. Je Zigbee-apparaten zijn nu zichtbaar in Home Assistant!

