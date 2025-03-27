# Installatie Homeassitant:
Door gebruik te maken van de OpenSource oplatfrom Home-Assitant kunnen we alles makelijk automatiseren en verbinden met elkaar. Hieronder vind je de nodig stappen en onderdelen dat je nodig zal hebben om thuis of op werk je eigen domotica systeem op te stellen.

# Documentatie Sensoren

**Studiegebied:** Technology, Bachelor in Elektronica-ICT, Campus Brugge  
**Academiejaar:** 2024-2025  

---

## Inhoudstafel

1. [Wat?](#wat)
2. [Benodigdheden](#benodigdheden)
3. [Protocollen](#protocollen)
    - [Zigbee: Stappenplan](#zigbee-stappenplan)
    - [Wi-Fi: Stappenplan](#wifi-stappenplan)
    - [Z-Wave: Stappenplan](#z-wave-stappenplan)
    - [Bluetooth: Stappenplan](#bluetooth-stappenplan)
    - [Thread: Stappenplan](#thread-stappenplan)

---

## Wat?

Dit is een handleiding voor het toevoegen van nieuwe sensoren aan Home Assistant. We tonen stap voor stap met tekst en afbeeldingen hoe je dit eenvoudig doet.

---

## Benodigdheden

- Home Assistant
- Home Assistant URL
- Adapter (Zigbee, Z-Wave, ...)
- Computer
- Internetverbinding
- Sensoren
- Gezond verstand

---

## Protocollen

### Zigbee: Stappenplan

1. Download de Zigbee2MQTT add-on.
2. Open Home Assistant via `http://homeassistant.local:8123`.
3. Ga naar de [Github-pagina van Koenkk](https://github.com/zigbee2mqtt/hassio-zigbee2mqtt#installation).
4. Voeg de repository toe via "Add add-on repository to my home assistant".
5. Ga naar **Instellingen > Add-On Store** en installeer Zigbee2MQTT.
6. Activeer "Start bij opstarten" en "Weergeven in zijbalk".
7. Open **Zigbee2MQTT Web UI** en zet "Aanmelden toestaan" op "alles".
8. Zet je sensor in paring-modus en deze zal verschijnen in de interface.

---

### Wi-Fi: Stappenplan

#### Benodigdheden
- Home Assistant (op Raspberry Pi, mini-pc of NAS)
- 2.4GHz Wi-Fi-router
- Wi-Fi-compatibele smarthome-apparaten (bv. Shelly, Sonoff, TP-Link)

#### Installatie

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

### Z-Wave: Stappenplan

1. Sluit de Z-Wave dongle aan op je apparaat.
2. Ga naar [Home Assistant Z-Wave integratie](https://www.home-assistant.io/integrations/zwave_js/).
3. Open Home Assistant en ga naar **Instellingen > Apparaten & Diensten**.
4. Voeg de **Z-Wave JS integratie** toe en volg de instructies.
5. Voeg een nieuw apparaat toe via **Apparaat toevoegen**.
6. Volg de instructies op het scherm en voltooi de configuratie.

---

### Bluetooth: Stappenplan

**Methodes:**
1. Bluetooth Proxy (ESP32)
2. Lokale Bluetooth-adapter

#### Installatie met ESPHome

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

### Thread: Stappenplan

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

Dit document biedt een duidelijke handleiding voor het toevoegen van verschillende soorten sensoren aan Home Assistant!

[Sensoren Toevoegen (pdf).pdf](https://github.com/user-attachments/files/19482208/Sensoren.Toevoegen.pdf.pdf)

