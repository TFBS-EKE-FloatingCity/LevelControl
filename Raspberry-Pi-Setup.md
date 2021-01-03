# Schritte

1. Raspbian Buster Lite auf SD Card flashen
2. WLAN und SSH konfigurieren (siehe Links)
3. Über SSH verbinden (Putty)
4. Pakete mit `sudo apt update && sudo apt upgrade`
5. SPI aktivieren (`sudo raspi-config`)
6. Git installieren (`sudo apt install git`)
7. NodeJS installieren (siehe Notiz)
8. Repository clonen (siehe Notiz)
9. cd Raspberry (die Repository betreten)
10. Dependencies installieren (`npm install`)
11. Projekt mit `npm start` starten

# Notizen

## Image
https://www.raspberrypi.org/downloads/raspbian/

## SSH
https://www.raspberrypi.org/documentation/remote-access/ssh/

## WLAN
https://www.raspberrypi-spy.co.uk/2017/04/manually-setting-up-pi-wifi-using-wpa_supplicant-conf/

## NodeJS installieren
`curl -sL https://deb.nodesource.com/setup_13.x | sudo -E bash -`
`sudo apt-get install -y nodejs`

## Repository clonen
`git clone https://bSdSchule@dev.azure.com/bSdSchule/FloatingCity/_git/Raspberry`

User: raspi1

Passwort: `twayh3ay5x3kv5i6tiwkbrsswepth7674runzbwm23odvoohpcvq`


# Alte Anleitung

* Raspbian Buster Lite geflashed
* WLAN Settings hinterlegt (wpa_supplicant.conf & ssh)
* SPI aktiviert (raspi-config)
* apt Update (Store aktualisieren)
* apt-get upgrade (System aktualisieren)
* sudo apt install nodejs (Node.js Framework installieren)
* sudo apt install git
* git clone (Git Repo klonen)
* sudo apt install npm (Package Manager)

###Git Zugangsdaten

User: raspi1

Passwort: twayh3ay5x3kv5i6tiwkbrsswepth7674runzbwm23odvoohpcvq

https://bSdSchule@dev.azure.com/bSdSchule/FloatingCity/_git/Raspberry