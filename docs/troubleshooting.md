# Troubleshooting tecnico

Questi sono quattro problemi di integrazione che ho analizzato durante il progetto. In ogni caso, il problema visibile compariva in una parte del sistema mentre la causa si trovava altrove nel flusso.

## 1. Una fase successiva del workflow non vedeva un aggiornamento di stato

Un record doveva attraversare due fasi in sequenza: generare il mockup, quindi costruire il prodotto dello store. La prima fase si completava e il database mostrava il nuovo stato, ma la seconda fase continuava a non partire.

Ho controllato prima il filtro e la scrittura sul database. Entrambi erano corretti. Il problema era nel modo in cui il router di Make gestiva i dati all'interno della stessa esecuzione: entrambe le route valutavano la versione del record acquisita all'inizio dell'esecuzione.

Ho separato le due fasi in esecuzioni differenti e usato il passaggio di approvazione già esistente come confine tra le due. In questo modo la seconda fase poteva leggere normalmente lo stato aggiornato.

## 2. Un'espressione non supportata produceva un output vuoto

Le card di revisione admin apparivano vuote o mostravano soltanto una parte del riepilogo previsto, anche se i dati sottostanti erano presenti.

I dati erano corretti. Due builder di testo utilizzavano `concat()`, che non era supportato in quel contesto di espressione Make. Poiché il problema non emergeva come un errore utile al momento del salvataggio, sembrava che mancassero dati più avanti nel flusso.

Ho sostituito l'espressione con l'interpolazione nativa di Make e ricontrollato il workflow live per assicurarmi che fosse ancora attivo e che non ci fossero esecuzioni incomplete in attesa.

## 3. Shopify aggiungeva parametri rifiutati dall'endpoint immagini

Le immagini di anteprima dei prodotti scomparivano durante una parte del flusso di revisione dello School Store, mentre le stesse immagini continuavano a essere renderizzate correttamente quando richiamate direttamente.

Confrontando i due percorsi di richiesta, ho visto che le richieste che passavano attraverso Shopify includevano parametri di trasporto aggiuntivi. L'endpoint immagini downstream rifiutava i parametri che non si aspettava.

Il proxy ora valida la richiesta in ingresso e inoltra soltanto i parametri necessari all'endpoint immagini. Questo ha ripristinato le anteprime senza indebolire i controlli sulla richiesta.

## 4. L'API di un fornitore rifiutava le richieste firmate

Le richieste d'ordine al fornitore continuavano a restituire un errore di firma non valida, anche se payload e secret erano già stati verificati.

Ho confrontato la costruzione della richiesta con la documentazione del fornitore e ho scoperto che l'implementazione non corrispondeva al metodo di firma previsto dall'API.

Dopo aver corretto la richiesta in modo che rispettasse il metodo documentato, il fornitore l'ha accettata e ha restituito un identificativo ordine.

## Cosa ho ricavato da questi casi

I controlli più utili erano quasi sempre ai confini tra i sistemi: quando Make legge lo stato, cosa Shopify aggiunge a una richiesta o cosa un fornitore si aspetta esattamente da una chiamata API. Verificare direttamente queste assunzioni si è dimostrato più efficace che modificare il componente più vicino al sintomo visibile.
