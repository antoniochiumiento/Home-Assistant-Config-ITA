🖥️ **Installazione di NUT in un container Proxmox**

Per semplificare l'installazione di NUT, utilizzeremo uno script automatico che installerà anche una webgui PeaNUT.
Utilizzeremo uno degli ottimi script del sito https://community-scripts.github.io/ProxmoxVE/

1️⃣ **Creare un Container LXC su Proxmox**

Accedi al terminale di Proxmox ed esegui il seguente comando:
```
bash -c "$(wget -qLO - https://community-scripts.github.io/ProxmoxVE/scripts?id=peanut)"
```
Lo script configurerà automaticamente il container con NUT installato e pronto all'uso.
PeaNUT sarà raggiungibile all'indirizzo: http://IP-CONTAINER:3000

2️⃣ **Abilitare l'Accesso USB al Container (se necessario)**

Se l'UPS è collegato via USB, abilita il passthrough USB:

Trova l'ID USB del tuo UPS con:
```
lsusb
```
Aggiungi il dispositivo USB al container:
```
pct set <ID_CONTAINER> -usb0 host=051D:0002
```
(Sostituisci 051D:0002 con il valore corretto del tuo UPS.)

Riavvia il container:
```
pct stop <ID_CONTAINER>
pct start <ID_CONTAINER>
```
3️⃣ **Testare la Connessione**

Dopo l'installazione, puoi verificare che NUT funzioni correttamente con il comando:
```
upsc nutdev1@localhost
```
Se vuoi accedere da un altro dispositivo:
```
upsc nutdev1@<IP_DEL_CONTAINER>
```
Se ricevi i dati dell'UPS, la configurazione è riuscita! 🎉

🎯 **Conclusione**

Ora hai NUT installato in un container Proxmox utilizzando uno script automatizzato. Puoi usarlo per monitorare l'UPS o integrarlo con Home Assistant. 🚀