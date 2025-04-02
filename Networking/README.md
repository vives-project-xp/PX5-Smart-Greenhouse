# :wireless: network-configuration

Deze pagina bevat een uitleg over hoe we ons netwerk opzetten in de serre. In deze uitleg volgen we het scenario die uitgetekend staat hier: [Schema](/Schema's/scenario's/Scenario-Final.drawio.png)

## Benodigdheden
- Raspberry PI
- WiFi verbinding
- Ethernet kabel

## Raspberry PI OS Installeren
De eerste stap is het installeren van Raspberry PI OS(lite). Dit doen we via de [officiële-documentatie](https://www.raspberrypi.com/software/) van raspberry pi. Eens dit is gebeurt ben je klaar om van de raspberry pi een Pi-Router te maken.

## 📦 Installeren dependencies
```bash
sudo apt update
sudo apt install dnsmasq netfilter-persistent iptables-persistent -y
