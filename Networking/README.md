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
```
## 🌐 Statisch IP zetten
Als je meerdere ethernet interfaces hebt moet je eerst kijken welke je wil gebruiken als gateway voor je netwerk maar aangezien er maar één netwerkpoort is aan aan raspberry PI kunnen we enkel deze gebruiken.
```bash
sudo nano /etc/dhcpcd.conf
```
Je voegt het volgende toe aan dit bestandje. Vul op xxx een getal tussen 0-255 in. Het is best niet 0 of 1 in te vullen omdat dit meestal het subnet is van je thuisnetwerk en dit voor problemen kan zorgen. Wij kiezen voor 101 dus (192.168.101.1/24)
```bash
interface eth0
    static ip_address=192.168.xxx.1/24
```
