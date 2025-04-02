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
## 📡 Instellen dnsmasq voor DHCP
Het wordt aangeraden een backup te maken van dit bestandje indien er iets fout gaat.
```bash
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
```
Dan maken we een nieuw bestandje en deze zullen we bewerken.
```bash
sudo nano /etc/dnsmasq.conf
```
In dit bestandje voegen we het volgende toe.
```bash
interface=eth0
dhcp-range=192.168.xxx.yyy,192.168.xxx.zzz,255.255.255.0,24h
```
De dhcp range zullen de IP's zijn die je via de ethernet verbinding zal uitsturen.
- xxx: Op deze plek vullen we het subnet in die we in de vorige stap gekozen hebben.
- yyy: Hier vullen we het eerste IP van de range in dit is een getal tussen 2-255
- zzz: Hier vullen we het laatste IP van de range in dit is opnieuw een getal tussen 2-255
Voorbeeld:
```bash
interface=eth0
dhcp-range=192.168.101.15,192.168.101.20,255.255.255.0,24h
```
In dit voorbeeld kunnen clients een IP krijgen tussen 192.168.101.15 tot 192.168.101.20. We kunnen dus maximaal 6 clients een ip geven.

## 🌐 Aanzetten IP forwarding
```bash
sudo nano /etc/sysctl.conf
```
Voeg volgende lijn toe of uncomment deze als dit er al stond.
```bash
net.ipv4.ip_forward=1
```
Als laatste passen we dit toe.
```bash
sudo sysctl -p
```
## 📡 Opzetten NAT
Voer uit:
```bash
sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
sudo netfilter-persistent save
```
## 🔄 Restart alles.
```bash
sudo systemctl restart dnsmasq
sudo reboot
```