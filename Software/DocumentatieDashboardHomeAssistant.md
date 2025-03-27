### Home Assistant Dashboard Documentatie

Hier is het stappenplan hoe je een dashboard maakt op Home Assistant.

---

## 📌 Stap 1: Home Assistant Downloaden
Als je Home Assistant nog niet hebt gedownload, doe dat op deze manier:
- **Raspberry Pi:** Gebruik Home Assistant OS.
- **Docker:** Gebruik Home Assistant Container-versie.
- **Windows/macOS/Linux:** Installeer via een virtuele machine.

Meer informatie: [Home Assistant Installatie](https://www.home-assistant.io/installation/)

---

## 🔹 Stap 2: Inloggen en Basisconfiguratie
1. Open Home Assistant in je browser via:  
   `http://homeassistant.local:8123`
2. Log in met je Home Assistant-account.
3. Ga naar **Instellingen > Dashboard**.

---

## 🔹 Stap 3: Een Nieuw Dashboard Toevoegen
1. Ga naar **Instellingen > Dashboard**.
2. Klik rechtsboven op de drie puntjes en kies **Dashboard bewerken**.
3. Klik op **+ Dashboard toevoegen**.
4. Voer een **naam** in voor het dashboard.
5. Kies een type dashboard:
   - **Beheerd** (Automatisch kaarten toevoegen).
   - **Handmatig** (Zelf controle over de lay-out).
6. Klik op **Opslaan**.

---

## 🔹 Stap 4: Weergaven (Tabbladen) Maken
Een weergave is een pagina binnen je dashboard.
1. Klik op **+ Weergave toevoegen**.
2. Geef de weergave een **naam**.
3. Kies een **pictogram** (bijv. `mdi:home` voor een huis).
4. Kies een **indeling**:
   - **Raster** (kaarten worden netjes uitgelijnd).
   - **Muur** (vrij positioneren van kaarten).
   - **Paneel** (één grote kaart over de hele breedte).
5. Klik op **Opslaan**.

🔹 **Extra optie:** Gebruik rechtenbeheer om de weergave te verbergen voor bepaalde gebruikers.

---

## 🔹 Stap 5: Kaarten Toevoegen aan je Dashboard
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

### YAML Voorbeeld: Knopkaart
```yaml
type: button
entity: switch.lamp_woonkamer
name: Woonkamerlamp
icon: mdi:lightbulb 
tap_action:
  action: toggle
```

💡 **Tip!** Gebruik **HACS (Home Assistant Community Store)** om extra kaarten toe te voegen.

---

## 🔹 Stap 6: Automatiseringen en Scripts Toevoegen
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

## 🔹 Stap 7: Dashboard Personaliseren

### 🎨 Kleuren en Thema’s Wijzigen
1. Ga naar **Instellingen > Profiel**.
2. Kies een **thema**.
3. Klik op **Opslaan**.

### 🌆 Achtergrondafbeelding Instellen
1. Ga naar je dashboard en klik op **Dashboard bewerken**.
2. Open **Thema-instellingen**.
3. Voeg een aangepaste YAML-code toe:
   ```yaml
   background: "center / cover no-repeat url('/local/background.jpg') fixed"
   ```
4. Upload je afbeelding naar `/config/www/` en herstart Home Assistant.

### 🔠 Eigen CSS en JavaScript Gebruiken
- Gebruik **HACS** om custom cards toe te voegen.
- Voeg **Lovelace UI-Mode** toe voor geavanceerde aanpassingen.

---

## 🔹 Stap 8: Dashboard Beschikbaar Maken op Externe Apparaten
Wil je jouw dashboard bekijken op je telefoon of tablet? 
- Installeer de **Home Assistant Companion App** (Android/iOS).
- Gebruik **Nabu Casa** of **DuckDNS** voor externe toegang.

---

## 🎯 Conclusie
Je hebt nu een volledig functioneel dashboard in Home Assistant! 🚀
Wil je extra functies zoals Zigbee-apparaten, ESPHome-sensoren of Node-RED-integraties toevoegen? Laat het weten! 😊
