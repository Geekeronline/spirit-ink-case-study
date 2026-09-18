# Affidabilità

Questo sistema può spendere denaro reale e creare ordini fisici presso i fornitori, quindi dati mancanti e risultati incerti delle API devono essere gestiti con attenzione.

## Bloccare il flusso quando mancano dati del fornitore

Se il sistema non dispone dei dati esatti del fornitore di cui ha bisogno, la riga d'ordine viene bloccata prima dell'invio. Il blocco registra ciò che manca, così il problema può essere risolto senza fare supposizioni.

Qualsiasi mapping privo di un identificativo fornitore confermato rimane non disponibile finché non viene verificato.

## Evitare ordini duplicati ai fornitori

Prima di creare un ordine presso il fornitore, il sistema verifica se quella riga d'ordine possiede già un ID ordine del fornitore. Se esiste, non viene creato un nuovo ordine.

Un caso più complesso si verifica quando la richiesta è stata inviata ma la risposta non è mai tornata. Il fornitore potrebbe aver creato l'ordine anche se l'automazione non riesce a confermarlo. Questi casi vengono inviati a revisione manuale prima di qualsiasi retry, perché inviare nuovamente lo stesso ordine potrebbe creare un duplicato e addebitare il cliente due volte.

## Mantenere la pubblicazione sotto revisione umana

L'automazione prepara store scolastici e prodotti fino a uno stato pronto per la revisione. Una persona li approva prima che diventino visibili ai clienti.

La pagina di revisione mostra l'artwork renderizzato e la presentazione lato cliente, non soltanto i campi del database, così chi approva può vedere ciò che vedranno i clienti.

## Regole di sicurezza

1. Il routing verso i fornitori utilizza esclusivamente mapping esatti e confermati.
2. Prodotti e store scolastici vengono revisionati prima della pubblicazione visibile ai clienti.
3. Credenziali, URL dei webhook, token di approvazione e identificativi dell'infrastruttura restano fuori dal version control.
4. I controlli su firma, identità, mapping dei fornitori e ordini duplicati bloccano il flusso quando falliscono.
5. Una creazione incerta dell'ordine presso il fornitore passa attraverso una riconciliazione manuale prima di qualsiasi retry.
6. Quando documentazione e sistema live non coincidono, il sistema live viene verificato nuovamente prima di effettuare modifiche.
7. Non vengono creati ordini reali presso i fornitori soltanto per produrre un risultato di test end-to-end.

## Evidenza in produzione

Il percorso del fornitore principale ha completato un ordine reale e pagato passando per produzione, spedizione, tracking del corriere ed evasione in Shopify.

Il percorso del secondo fornitore è attivo per i mapping confermati, con routing e integrazione del fornitore verificati. Il primo ordine naturale e pagato su quel percorso non si è ancora verificato.
