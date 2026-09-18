# Architettura

## Una fonte autorevole per ogni tipo di dato

Il sistema utilizza diversi servizi, quindi ogni tipo di dato ha una fonte di verità ben definita. Gli altri sistemi possono conservarne riferimenti o copie operative, ma non devono diventare fonti concorrenti.

| Sistema | Responsabilità principale |
| --- | --- |
| **Servizio backend** (Cloud Run) | Identità di prodotti, colori e composizioni; geometria di stampa; contratti di produzione |
| **Shopify** | Prodotti e varianti, checkout, dati cliente, ordini e record di fulfilment |
| **Airtable** | Mapping dei fornitori, configurazione runtime, code e stato degli ordini |
| **Make** | Orchestrazione dei workflow, transizioni di stato e controlli di sicurezza |

Airtable conserva i dati operativi, ma non decide quali prodotti possano esistere. I workflow Make evitano inoltre di codificare direttamente nomi prodotto, colori, taglie o opzioni dei fornitori.

## Flusso commerciale

Il percorso di un ordine pagato è suddiviso in cinque fasi:

1. Creare il prodotto Shopify e salvare il mapping del fornitore.
2. Portare le righe dell'ordine Shopify pagato nello stato operativo.
3. Costruire il pacchetto di produzione con artwork e dati di posizionamento.
4. Inviare ogni riga al fornitore corretto dopo il superamento dei controlli richiesti.
5. Riportare il tracking del fornitore nel fulfilment Shopify.

Le fasi sono separate perché possono fallire in modi diversi. Un timeout temporaneo del fornitore può essere ritentato. Dati di posizionamento stampa non validi richiedono una correzione. Se non è certo che un ordine al fornitore sia stato creato, serve una riconciliazione manuale prima di inviare qualsiasi nuova richiesta.

## Identificativi di prodotto e fornitore

Lo stesso capo può avere diversi identificativi:

1. Uno SKU Shopify interno, usato come riferimento all'interno dello store.
2. Uno SKU variante del fornitore.
3. Un identificativo prodotto usato dall'API del fornitore quando viene creato l'ordine.

Possono sembrare simili, ma hanno funzioni diverse.

L'esatto **ProductVariant GID** di Shopify viene usato per identificare la variante Shopify selezionata nel flusso downstream. Gli identificativi del fornitore provengono esclusivamente da dati confermati dal fornitore. Se manca il mapping esatto, la riga viene bloccata invece di tentare di dedurlo.

Vedi [Affidabilità](reliability.md) per sapere come vengono gestiti dati mancanti o incerti dei fornitori.

## Configurazione dei prodotti

Nomi prodotto, ID, colori, taglie, prezzi e numero di varianti restano fuori dalla logica dei workflow Make. Aggiungere una nuova famiglia di prodotto richiede principalmente una modifica al catalogo e al contratto di produzione, invece di un nuovo ramo hard-coded nell'automazione.

Il catalogo consegnato copre nove famiglie di prodotto. Sette supportano attualmente la stampa sul fronte, sul retro o su entrambi tramite il registro di produzione.

Il catalogo completo e il registro di stampa fronte/retro sono configurazioni separate, quindi non è previsto che i due numeri coincidano.

## Store scolastici

Tutti gli store scolastici vivono all'interno dello stesso store Shopify. Ogni scuola può avere nomi e descrizioni personalizzati senza modificare il prodotto Shopify condiviso.

Questo è importante perché lo stesso prodotto può comparire in più store scolastici. Modificare il nome del prodotto Shopify condiviso per una singola scuola influenzerebbe tutte le altre scuole che lo utilizzano.

La gestione dello store è collegata al cliente Shopify autenticato tramite il flusso App Proxy. Prima di rinominare o eliminare una scuola in bozza, il sistema verifica l'identità del cliente, l'ID della scuola e lo stato corrente della scuola.
