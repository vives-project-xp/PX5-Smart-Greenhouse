# ESP32

# Sensoren
Een iets ruimere uitleg over de sensoren die worden gebruikt in dit project:

## Temperatuur, vochtigheids & luchtdruk
- **Sensor**: BME280


![BME280](./images/BME280.jpg)

-**Uitleg**: Deze sensor kan Temperatuur van  -40°C tot 85°C meten, luchtvochtigheid van 0% tot 100% RH  en luchtdruk van 300 hPa tot 1100 hPa meten. Deze sensor werkt op een voeding van 3,3V en kan communiceren via I2C en SPI. De sensor is zeer gevoelig voor veranderingen in de omgevingsomstandigheden, en biedt daardoor nauwkeurige metingen. Met de prijs van €6,85 is dit een goede prijs/kwaliteit verhouding.
- **Waarom deze senor?**:
## CO2 sensor
- **Sensor**: MH-Z19


![Co₂ sensor](./images/MH-Z19.jpg)
- **Uitleg**: Deze sensor maakt gebruik van NDIR (Non-Dispersive Infrared) technologie om de CO₂-concentratie te meten. Een infrarood lichtbron zendt licht uit in een gaskamer, waar CO₂-moleculen specifieke golflengtes absorberen. Een fotodetector meet hoeveel licht er passeert en de sensor berekent de CO₂-niveaus.
- **Waarom deze sensor?**: We kiezen voor deze sensor omdat deze gezien wordt als één van de beste CO₂ sensors binnen een budget. Deze sensor is ook zeer goed gedocumenteerd waardoor het integreren net wat gemakkelijker is.
## Lichtintensiteitssensor
- **Sensor**: BH1750 16-bit I2C


![Lichtintensiteitssensor](./images/bh1750-16-bit-i2c.webp)
- **Uitleg**: Dit is een low-power sensor die ambient light meet door gebruik te maken van een photodiode. Deze sensor heeft een bereik van 1-65535 LUX, dit bereik is goed voor on maar als je moet meten in extreem felle omstandigheden kan dit meetbereik een probleem zijn. Maar voor dit project is dit niet van toepassing. 
- **Waarom deze sensor?**: Deze sensor is goed gedocumenteerd waardoor deze makkelijk te integreren is. De sensor geeft ook direct een LUX waarde zonder dat je extra berekeningen moet uitvoeren zoals bij sommige andere lichtintensiteitssensoren.

## Bodemvochtigheidssensor
![Bodemvochtigheidssensor](./images/Bodemvochtigheidssensor.jpg)

- **Uitleg:** We hebben gekozen voor een sensor van Otronic, namelijk de [OT2019-D69](https://www.otronic.nl/nl/bodemvochtsensor-met-voltage-regulator.html?source=googlebase&gad_source=1). 
Deze sensor werkt op zowel 3,3V als 5V en heeft een uitgangsspanning van 0-3V. De prijs van de sensor is €4,35 exclusief verzendkosten.  

- **Waarom deze sensor?** We hebben voor deze sensor gekozen omdat deze gebruik maakt van de TL555I-timer en de 662-voltageregulator. Deze componenten zorgen voor een nauwkeurige meting, want er zijn andere sensoren in de omploop die deze componenten niet gebruiken en waarbij de sensoren niet zo goed werken. Daarnaast is deze sensor corrosiebestendig, wat betekent dat hij niet snel zal roesten.  

- **Andere bodemvochtigheidssensoren:** Er waren ook andere interessante sensoren, zoals de [Joy-it SEN-Moisture](https://www.conrad.be/nl/p/joy-it-sen-moisture-sensormodule-geschikt-voor-serie-raspberry-pi-pcduino-bbc-micro-bit-calliope-banana-pi-arduin-2176923.html) van Conrad. Deze sensor was echter niet corrosiebestendig, waardoor we deze niet hebben gekozen.  Tot slot vonden we nog een sensor van Digikey, namelijk de [101020614](https://www.digikey.be/en/products/detail/seeed-technology-co-ltd/101020614/10451856). Deze sensor was wel corrosiebestendig, maar de levertijd en de verzendkosten waren te hoog.  