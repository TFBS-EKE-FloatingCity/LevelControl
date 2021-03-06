# RaspberryPi einrichten 

1. Raspbian Lite auf SD Card flashen
2. WLAN, SSH und SPI konfigurieren (`sudo raspi-config` & siehe unten)
3. Über SSH verbinden (Putty)
4. Pakete mit `sudo apt update && sudo apt upgrade`
5. Git installieren (`sudo apt install git`)


## Image/Flash Software
https://www.raspberrypi.org/software/

OS Wahl:
<br>
Raspberry Pi OS (other) **=>** Raspberry Pi OS lite (32-bit) 

SD-Card auswählen und schreiben/write drücken

## Der erste Start von Raspbian Lite: 
https://www.dennis-henss.de/2018/03/28/installation-und-konfiguration-raspbian-lite-auf-raspberry-pi-3/

## Wichtige Punkte (Bei Problemen siehe Artikel oben):
- Tastatur auf Deutsch stellen / Zeitzone anpassen
- Bitte Passwort in z.b. "floatingCity1!" ändern (Davor Tastatur auf Deutsch stellen)
- WLAN
- SSH
- SPI
- Reboot nach Einstellungsänderungen

## WLAN
System Options / Wireless LAN

## SSH
Interfacing Options / SSH => Hier bestätigt man die Frage mit "Yes/Ja"

## SPI
Interfacing Options / SPI => Hier bestätigt man die Frage mit "Yes/Ja"

SPI1 aktivieren:

``` shell
sudo nano /boot/config.txt
```
am Ende der Datei muss diese Zeile hinzugefügt werden:

``` shell
dtoverlay=spi1-3cs
```

Um alle aktivierten interfaces anzuzeigen:

``` shell
> ls /dev/*spi*
/dev/spidev0.0 /dev/spidev0.1 /dev/spidev1.0 / dev/spidev1.1 /dev/spidev1.2
```

Dann muss der Raspberry-pi neu gestartet werden:

``` shell
sudo reboot
```

# FloatingCity RaspberryPi (Master) Node Server

### Requirements

-   Node(14.15.1) + NPM/Yarn
-   Python3
-   g++

### Installation (RaspberryPi / Linux)

-   Install NodeJS

```shell
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash - && sudo apt-get install -y nodejs
```

-   Install Python

```bash
 sudo apt-get update  && sudo apt-get install python3
```

-   Install Build-Essentials (for the c++ compiler)

```bash
 sudo apt-get update  && sudo apt-get install build-essential
```

-   Install Yarn (after NodeJS)

```shell
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```

```shell
sudo apt update && sudo apt install yarn
```


### Installation (Windows)


-   Install NodeJS


Install Node-Verion-Manager with setup: https://github.com/coreybutler/nvm-windows/releases/tag/1.1.7
```shell
nvm install 14.15.1

nvm use 14.15.1
```


-   Install Yarn

Install: https://classic.yarnpkg.com/en/docs/install/#windows-stable
<br>
Restart all Commandline Tools (Including IDE)


-   Install Windows Build Tools

Method 1.
```shell
yarn global add windows-build-tools
```

Method 2.
<br>
Install C++ Tools in Visual Studio



### Checkout and run server

-   Clone repository

```shell
git clone https://bSdSchule@dev.azure.com/bSdSchule/FloatingCity/_git/Raspberry
```

-   Install Node Modules

```shell
cd **projectfolder**
yarn
```

### Run Server

-   Install Node Modules

```shell
yarn start
```



### Git Zugangsdaten

User: raspi1

Passwort (Muss eventuell neu generiert werden (Repos => clone => Credentials)): znwneoqxc3b5a52uw76ag7t7xqby5pkq5625lrc2t2wwezdqv6uq

https://bSdSchule@dev.azure.com/bSdSchule/FloatingCity/_git/Raspberry

