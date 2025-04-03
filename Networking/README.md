# :wireless: network-configuration

Deze pagina bevat een uitleg over hoe we ons netwerk opzetten in de serre. In deze uitleg volgen we het scenario die uitgetekend staat hier: [Schema](/Schema's/scenario's/Scenario-Final.drawio.png)

## Benodigdheden
- Raspberry PI
- WiFi verbinding
- Ethernet kabel

## Raspberry PI OS Installeren
De eerste stap is het installeren van Raspberry PI OS(lite). Dit doen we via de [officiële-documentatie](https://www.raspberrypi.com/software/) van Raspberry PI. Eens dit is gebeurt ben je klaar om van de Raspberry PI een Pi-Router te maken. 
## :wireless: Aanzetten WiFi Raspberry PI
Om te verbinden met een WPA2-Enterprise netwerk via Raspberry PI OS met GUI is dit niet moeilijk je doet dit gewoon bijne zoals op windows gewoon met een andere GUI

Als we gebruik maken van Raspberry PI OS lite vereist dit wel een paar stappen aangezien we geen GUI hebben.
Als eerste controleren we of WiFi is uitgeschakeld op de Raspberry PI. Deze is standaard meestal uitgeschakeld door rfkill.
```bash
nmcli device status
```
Hier zien we volgende line als de wifi is uitgeschakeld. Als dit connected/disconnected is dan hoef je volgende stappen niet uitvoeren
```bash
wlan0   wifi      unavailable
```
Eerst controleren we of er wel een wifi-module beschikbaar is (als je 100% zeker bent dat er een wifi module is dan is dit overbodig.)
```bash
iw dev
```
We verwachten volgende output:
```bash
phy#0
        Interface wlan0
                ifindex 3
                wdev 0x1
                addr xx:xx:xx:xx:xx:xx
                type managed
                channel 34 (5170 MHz), width: 20 MHz, center1: 5170 MHz
```
Als je hier geen output hebt dan werkt je WiFi chip niet of is er geen chip aanwezig

Daarna controleren we of rfkill de WiFi chip blokkeert
```bash
rfkill list all
```
Hier zien we:
```bash
1: phy0: Wireless LAN
        Soft blocked: yes
        Hard blocked: no
``` 
Als hier NO staat dan is WiFi niet geblokkeerd door rfkill.

Eens we dit bekeken hebben zullen we WiFi unblocken
```bash
sudo rfkill unblock wifi
```
Dan zetten ze wlan0 aan
```bash
sudo ip link set wlan0 up
```
Eens dit is gebeurd zullen de network-manager rebooten
```bash
sudo systemctl restart NetworkManager
```
Als laatste zetten we de WiFi radio aan:
```bash
sudo nmcli radio wifi on
```
Eens dit is gebeurd kan je verbinden met een WiFi netwerk we controleren dit eerst met:
```bash
nmcli device status
```
We verwachten hier volgende output:
```bash
wlan0          wifi      disconnected            
```
Daarna restarten we nog eens de Raspberry PI
```bash
sudo reboot        
```
Eens dit is gebeurd staat de wifi module aan en kunnen we verbinden met een netwerk. 
## :wireless: Verbinden met een WPA2-Enterprise WiFi netwerk
Voor een WPA2- Enteprise netwerk gebruiken we volgend commando:
```bash
nmcli con add type wifi ifname wlan0 ssid "YOUR_SSID" \
802-1x.identity "USERNAME" \
802-1x.password "PASSWORD" \
802-1x.eap peap \
802-1x.phase2-auth mschapv2 \
wifi-sec.key-mgmt wpa-eap
```
Vervang "YOUR_SSID" door de naam van het netwerk waarmee je wil verbinden "USERNAME" door je username die je hebt gekregen en als laatste "PASSWORD" door het wachtwoord dat je hebt ingesteld.
## 🍓 Instellen PI-Router
We zullen de Raspberry PI gebruiken als een router. We doen dit omdat een WPA2-Enterprise netwerk niet kan verbinden met ESP's indien dit nodig is een ook niet met Home Assistant OS.
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