# Google Sheets

Questo package configura Home Assistant per salvare dei valori automaticamente in un file Google Spreadsheet

## Configurazione
- ** Configurare Google Sheets API
    Devi configurare l'API di Google Sheets e ottenere le credenziali:
    - Vai su Google Cloud Console
    - Crea un nuovo progetto e abilita l'API di Google Sheets e Google Drive
    - Crea una credenziale "Service Account" e scarica il file JSON delle credenziali
    - Condividi il tuo foglio Google con l'email della Service Account

- ** Installare il componente hass_google_sheets
    Puoi usare il componente custom hass_google_sheets:
    - Installa hass_google_sheets via HACS oppure manualmente
    - Copia il file JSON delle credenziali in /config/ e specifica il percorso della configurazione

- ** Creare il package
    Crea un file YAML in /config/packages

## Progetti realizzati grazie a questo package

### Storico Consumo Energetico
