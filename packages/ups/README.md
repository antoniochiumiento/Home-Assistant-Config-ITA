# UPS
Gestire le interruzioni di corrente non è mai una priorità assoluta quando si pensa alla smarthome, a implementare un server casalingo o a spostare alcuni dei propri servizi in self-hosted .... fino a quando non sarà l'unica priorità assoluta dopo che un blackout bloccherà tutti i nostri servizi e server in casa mentre noi siamo lontani da essa o peggio ancora quando il NAS casalingo si danneggerà e perderemo una parte dei nostri dati.

Per fortuna per tutto ciò ci viene in aiuto **NUT**.

**Network UPS Tools** (**NUT**) è un software open-source che consente di monitorare e gestire gruppi di continuità (UPS). 
Grazie a NUT, il sistema può:
- Monitorare lo stato dell'UPS (carica della batteria, tensione, capacità residua, ecc.).
- Eseguire azioni automatiche in caso di interruzione di corrente, come spegnere in sicurezza il Raspberry Pi.
- Fornire dati a dispositivi in rete per una gestione centralizzata dell'energia.

Questa guida per installare NUT è suddivisa in due versioni:
- [Installare e Configurare NUT su un Raspberry PI](https://github.com/antoniochiumiento/Home-Assistant-Config-ITA/blob/main/packages/ups/NUT_Rasp.md)
- [Installare e configurare NUT in un container Proxmox](https://github.com/antoniochiumiento/Home-Assistant-Config-ITA/blob/main/packages/ups/NUT_Proxmox.md)

## Integrazione di NUT in Home Assistant

**Introduzione**
Se hai configurato NUT (Network UPS Tools) sul tuo Raspberry Pi o su un altro server, puoi integrare le informazioni del tuo UPS in Home Assistant per monitorarlo direttamente dalla tua dashboard.

Questa guida spiega come integrare NUT in Home Assistant e creare una card personalizzata con le informazioni più importanti dell'UPS.

1️⃣ **Abilitare l'accesso remoto a NUT**
Se NUT è installato su un altro dispositivo rispetto alla nostra installazione Home Assistant (come nel nostro caso un Raspberry Pi o un container), devi assicurarti che Home Assistant possa accedervi.

Modifica il file di configurazione di NUT:
```
sudo nano /etc/nut/upsd.conf
```
Aggiungi questa riga per permettere connessioni da Home Assistant:
```
LISTEN 0.0.0.0 3493
```
Poi, modifica il file degli utenti:
```
sudo nano /etc/nut/upsd.users
```

Riavvia i servizi di NUT:
```
sudo systemctl restart nut-server nut-client
```
2️⃣ **Aggiungere l'integrazione NUT in Home Assistant**
Apri Home Assistant.

Vai su Impostazioni > Dispositivi e Servizi.

Clicca su Aggiungi integrazione e cerca NUT.

Inserisci i seguenti dati:
```
Host: IP del Raspberry Pi o server con NUT (es. 192.168.1.100)

Porta: 3493

Username: monitor

Password: monitor_password
```
Conferma e Home Assistant rileverà automaticamente il tuo UPS.

3️⃣ **Creare una card personalizzata nella dashboard**
```
  - type: vertical-stack
    title: UPS 1
    cards:
      - type: history-graph
        entities:
          - entity: sensor.ups_stato
            name: Stato UPS 1
          - entity: sensor.ups_carica_batterie
            name: Carica Batteria
          - entity: sensor.ups_tensione_di_ingresso
            name: Tensione Ingresso
          - entity: sensor.ups_carico
            name: Carico UPS
          - type: entity
            entity: sensor.ups_autonomia_della_batteria
            name: Autonomia (minuti)
            icon: mdi:battery-clock
            state: "{{ (states['sensor.ups_autonomia_della_batteria'] | int / 60) | round(1) }} min"
        hours_to_show: 4
      - type: gauge
        entity: sensor.ups_carica_batterie
        name: Carica Batteria UPS 1
        severity:
          green: 50
          yellow: 20
          red: 0
```


Possiamo ovviamente inserire nello stesso Home Assistant più di un UPS e monitorarne qualsiasi suo stato facilmente configurando più NUT nelle impostazioni e moltiplicando la configurazione nel lovelace.