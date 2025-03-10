🖥️ **Installazione di Raspberry Pi OS**

Prima di configurare NUT, è necessario installare il sistema operativo sul Raspberry Pi. Segui questi passaggi:

Scarica Raspberry Pi Imager dal sito ufficiale: https://www.raspberrypi.com/software/

Inserisci una scheda microSD nel tuo computer e avvia Raspberry Pi Imager.

Seleziona il sistema operativo Raspberry Pi OS (64-bit) o Raspberry Pi OS Lite (senza interfaccia grafica, consigliato per server).

Seleziona la scheda microSD come destinazione e avvia la scrittura.

Una volta completata, inserisci la microSD nel Raspberry Pi e accendilo.

Se stai utilizzando Raspberry Pi OS Lite, abilita SSH con:
```
sudo raspi-config
```
Vai su Interfacing Options > SSH e abilitalo.

Connettiti via SSH dal tuo computer con:
```
ssh pi@raspberrypi.local
```
(La password predefinita è raspberry se non l'hai cambiata durante l'installazione.)

Ora sei pronto per installare e configurare NUT.

1️⃣ **Installazione di NUT**

Per installare NUT sul Raspberry Pi, eseguire il seguente comando:
```
sudo apt update && sudo apt install nut nut-client nut-server -y
```
2️⃣ **Verificare la connessione dell'UPS**
```
sudo nut-scanner
```
Se l'UPS viene rilevato correttamente, vedrai un output simile:
```
[nutdev1]
    driver = "usbhid-ups"
    port = "auto"
    vendorid = "051D"
    productid = "0002"
    product = "Back-UPS BX1200MI"
    serial = "9B2438A11074"
    vendor = "American Power Conversion"
    bus = "001"
```
3️⃣ **Configurare il driver dell'UPS**

Modifica il file di configurazione:
```
sudo nano /etc/nut/ups.conf
```
Aggiungi o modifica:
```
[nutdev1]
    driver = usbhid-ups
    port = auto
    vendorid = 051D
    productid = 0002
    desc = "APC Back-UPS BX1200MI"
```
Salva con CTRL+X, poi Y e INVIO.

4️⃣ **Configurare il servizio UPSD**

Modifica il file:
```
sudo nano /etc/nut/upsd.conf
```
Aggiungi:
```
LISTEN 127.0.0.1 3493
```
Se vuoi accedere da altri dispositivi in rete:
```
LISTEN 0.0.0.0 3493
```
5️⃣ **Creare un utente per l'accesso**

Modifica il file degli utenti:
```
sudo nano /etc/nut/upsd.users
```
Aggiungi:
```
[admin]
    password = tua_password
    actions = SET
    instcmds = ALL
    upsmon master
```
Se vuoi un utente solo per monitoraggio:
```
[monitor]
    password = monitor_password
    upsmon slave
```
6️⃣ **Configurare upsmon**

Modifica il file:
```
sudo nano /etc/nut/upsmon.conf
```
Aggiungi questa riga:
```
MONITOR nutdev1@localhost 1 admin tua_password master
```
7️⃣ **Abilitare NUT come sistema standalone**

Modifica il file:
```
sudo nano /etc/nut/nut.conf
```
Assicurati che sia presente:
```
MODE=standalone
```
8️⃣ **Riavviare i servizi e testare**

Riavvia i servizi di NUT:
```
sudo systemctl restart nut-server nut-client
sudo systemctl enable nut-server nut-client
```
Verifica lo stato dell'UPS:
```
sudo upsc nutdev1@localhost
```
Se tutto è corretto, vedrai informazioni sulla batteria, carica e stato dell'UPS.