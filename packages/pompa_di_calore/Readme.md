# Pompa di Calore
Questo progetto descrive come integrare la pompa di calore utilizzando **Espaltherma** e visualizzarne i dati su **Home Assistant** tramite una pagina Lovelace personalizzata.

## Prerequisiti

- **Home Assistant** configurato e funzionante.
- **Espaltherma** integrato con MQTT per inviare i dati della pompa di calore.
- Conoscenza di base della configurazione YAML di Home Assistant.

## Configurazione di Espaltherma
Espaltherma è configurato per inviare i dati operativi della pompa di calore al broker MQTT configurato su Home Assistant. Esempio di configurazione MQTT:

![Screenshot Pompa di Calore](/packages/pompa_di_calore/screenshot/Pompa_di_Calore.PNG)
![Screenshot Dettaglio Pompa di Calore](/packages/pompa_di_calore/screenshot/Pompa_di_Calore_2.PNG)