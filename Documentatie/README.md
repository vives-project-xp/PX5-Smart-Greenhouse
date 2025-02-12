# ESP32

# Sensoren
Een iets ruimere uitleg over de sensoren die worden gebruikt in dit project:

## Temperatuur, vochtigheids & luchtdruk
## CO2 sensor
- **Sensor**: MH-Z19


![Co₂ sensor](./Info%20componenten/images/MH-Z19.jpg)
- **Uitleg**: Deze sensor maakt gebruik van NDIR (Non-Dispersive Infrared) technologie om de CO₂-concentratie te meten. Een infrarood lichtbron zendt licht uit in een gaskamer, waar CO₂-moleculen specifieke golflengtes absorberen. Een fotodetector meet hoeveel licht er passeert en de sensor berekent de CO₂-niveaus.
- **Waarom deze sensor?**: We kiezen voor deze sensor omdat deze gezien wordt als één van de beste CO₂ sensors binnen een budget. Deze sensor is ook zeer goed gedocumenteerd waardoor het integreren net wat gemakkelijker is.
## Lichtintensiteitssensor
## Bodemvochtigheidssensor
## Luchtvochtigheidssensor
![otronic-3-in-1-sensor-temperature-humidity-and-air (1)](https://github.com/user-attachments/assets/e4e60abc-08a6-46e7-9678-48090e1f7f54)

-**Uitleg**: OT711-D64 (BME280) kan Temperatuur van  -40°C tot 85°C meten, luchtvochtigheid van 0% tot 100% RH  en luchtdruk van 300 hPa tot 1100 hPa meten. Deze sensor werkt op een voeding van 3,3V en kan communiceren via I2C en SPI. De sensor is zeer gevoelig voor veranderingen in de omgevingsomstandigheden, en biedt daardoor nauwkeurige metingen. Met de prijs van €6,85 is dit een goede prijs/kwaliteit verhouding.
## Uitgebreide Luchtvochtigheidssensor
![bme680-module-gas-temperature-pressure-and-humidity-sensor-with-level-converter-600x600 (1)](https://github.com/user-attachments/assets/362c3337-7cb9-4ab1-9d78-e8174fd1574c)

-**Uitleg**: Ik heb ook nog een sensor gevonden die naast Temperatuur, Luchtvochtig en luchtdruk ook nog CO2 kan meten. kan Temperatuur van  -40°C tot 85°C meten, luchtvochtigheid van 0% tot 100% RH  en luchtdruk van 300 hPa tot 1100 hPa meten. Deze sensor werkt op een voeding van 3,3V en kan communiceren via I2C en SPI.
De prijs van deze sensor is €11

