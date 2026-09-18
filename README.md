<a href="https://spirit-ink.co.uk/">
  <img src="assets/spirit-ink-banner.png" alt="Spirit Ink" width="100%">
</a>

# Caso studio sull'automazione di Spirit Ink

Spirit Ink è un progetto di automazione e-commerce per abbigliamento scolastico personalizzato.

I clienti creano un design direttamente nel browser e acquistano tramite Shopify. Dopo il pagamento, l'automazione prepara l'ordine per la produzione, lo invia al fornitore di stampa corretto e riporta il tracking in Shopify.

Sul piano tecnico, sono stato responsabile della progettazione del sistema, delle integrazioni tra i servizi, delle misure di sicurezza per la produzione, del troubleshooting tecnico e dell'handoff del progetto. Lo scope concordato è stato completato il **15 settembre 2026**.

## Il problema

Spirit Ink lavora con scuole e associazioni di genitori che vogliono vendere abbigliamento personalizzato con il proprio branding.

Senza automazione, il lavoro comprende la creazione della grafica, la raccolta dei file, la configurazione dei prodotti, la preparazione dei file di stampa, la gestione delle varianti dei fornitori, l'invio degli ordini al fornitore corretto e l'aggiornamento di Shopify quando vengono spediti. Il carico operativo cresce con ogni scuola e con ogni nuova opzione di prodotto.

L'obiettivo era automatizzare il più possibile questo processo, mantenendo però dei blocchi di sicurezza quando mancavano dati importanti del fornitore o quando le informazioni non erano sufficientemente certe.

## Cosa fa il sistema

```mermaid
flowchart LR
  A[Crea un design] --> B[Crea il prodotto in Shopify]
  B --> C[Il cliente completa il checkout]
  C --> D[Verifica il mapping del fornitore]
  D --> E[Prepara i file di stampa]
  E --> F[Invia l'ordine al fornitore]
  F --> G[Riceve il tracking]
  G --> H[Aggiorna Shopify]

  S[Account scuola] --> T[Crea o gestisce lo store scolastico]
  T --> A
```

Il cliente rimane all'interno del flusso Spirit Ink e Shopify. Una volta pagato l'ordine, l'automazione gestisce in background i passaggi necessari alla produzione.

Le scuole hanno anche un flusso separato per la gestione del proprio store. Possono costruire una gamma di prodotti, salvarla come bozza, inviarla in revisione e gestirla successivamente dallo stesso account cliente.

Il sistema copre nove famiglie di prodotto distribuite su due fornitori di stampa. Sette supportano attualmente la stampa sul fronte, sul retro o su entrambi i lati. Il flusso del fornitore principale ha completato un ordine reale e pagato, dal checkout Shopify fino a spedizione, tracking ed evasione dell'ordine. Il secondo fornitore è attivo per i mapping che sono stati verificati completamente.

## Il mio ruolo

Ho definito come i servizi dovessero comunicare tra loro e dove dovesse risiedere ogni tipo di dato.

Sono stato responsabile delle integrazioni tra Shopify, Make, Airtable, Google Cloud Run, Cloudinary e le API dei due fornitori di stampa.

Ho analizzato problemi che coinvolgevano più sistemi contemporaneamente, inclusi la gestione delle richieste Shopify, il comportamento dei workflow Make, i dati dei fornitori e i casi limite legati all'evasione degli ordini.

Per le parti del flusso a rischio più elevato, ho aggiunto controlli per evitare ordini duplicati ai fornitori, routing errato dei prodotti e pubblicazioni involontarie visibili ai clienti.

Ho inoltre mantenuto aggiornata la documentazione del progetto e preparato l'handoff finale, in modo che un'altra persona potesse comprendere la configurazione live senza dover ricostruire il sistema partendo da vecchi test o note di incidenti.

## Approfondimenti

| Documento | Contenuto |
| --- | --- |
| [Architettura](docs/architecture.md) | Come si collegano i componenti principali e dove risiede ogni tipo di dato |
| [Decisioni](docs/decisions.md) | Le principali decisioni tecniche e i compromessi che le hanno motivate |
| [Troubleshooting tecnico](docs/troubleshooting.md) | Esempi di problemi cross-system che ho analizzato |
| [Affidabilità](docs/reliability.md) | Controlli su routing dei fornitori, azioni duplicate e retry non sicuri |
| [Responsabilità sul progetto](docs/project-ownership.md) | Come ho coordinato il lavoro, validato le modifiche e gestito l'handoff |

## Stack tecnologico

Shopify, Make, Airtable, Google Cloud Run, Cloudinary, GitHub e le API di due fornitori di stampa on-demand.
