# Decisioni

Queste sono le principali scelte tecniche che hanno definito il sistema e i compromessi che hanno comportato.

## 1. Non dedurre gli identificativi dei fornitori

Gli identificativi prodotto dei fornitori provengono esclusivamente da dati confermati dal fornitore. Se manca un valore esatto, quel mapping rimane incompleto e la riga d'ordine non può essere inviata in produzione.

Gli SKU interni sono leggibili e i codici dei fornitori possono sembrare simili, quindi sarebbe facile essere tentati di dedurre il valore mancante. Un identificativo apparentemente plausibile può comunque puntare al prodotto sbagliato.

**Compromesso:** qualsiasi opzione di prodotto priva di un identificativo fornitore confermato rimane non disponibile finché non viene verificata.

## 2. Tenere separate le fasi del flusso commerciale

Il flusso dell'ordine pagato è suddiviso in cinque scenari Make invece di un unico workflow lungo.

Ogni fase ha un percorso di recupero diverso. Un timeout del fornitore può essere sicuro da ritentare. Dati di produzione non validi richiedono una correzione. Un risultato incerto nella creazione dell'ordine richiede una riconciliazione prima di inviare un'altra richiesta.

**Compromesso:** ci sono più workflow da mantenere e lo stato deve persistere tra uno e l'altro.

## 3. Usare Airtable per lo stato operativo

Airtable contiene i mapping dei fornitori, la configurazione runtime, le code e lo stato degli ordini. La disponibilità dei prodotti viene controllata altrove.

Questo evita che una tabella operativa comoda da usare si trasformi involontariamente in una allowlist dei prodotti. Un prodotto valido non dovrebbe smettere di funzionare soltanto perché qualcuno ha dimenticato di aggiungere una riga in Airtable.

**Compromesso:** alcune modifiche richiedono un aggiornamento esplicito del backend o del contratto invece di una rapida modifica alla tabella.

## 4. Mantenere l'approvazione umana prima della pubblicazione

L'automazione può preparare prodotti e store scolastici per la revisione, ma la pubblicazione visibile ai clienti viene comunque approvata da una persona.

Questo punto di revisione è utile perché artwork e presentazione sono più facili da valutare per una persona che per un'automazione. Limita inoltre l'impatto di un'ipotesi errata prima che possa diventare visibile ai clienti.

**Compromesso:** il processo include un breve passaggio manuale.

## 5. Leggere lo scenario Make live prima di modificarlo

Prima di modificare uno scenario live, ne leggo lo stato corrente e intervengo soltanto sui moduli coinvolti nel problema. I vecchi blueprint esportati vengono conservati come materiale di riferimento.

Reimportare un vecchio blueprint può sovrascrivere modifiche live più recenti oppure alterare parti dello scenario che non dovevano essere toccate.

**Compromesso:** è un approccio più lento rispetto all'applicazione di modifiche ampie partendo da un vecchio export.

## 6. Trattare fronte e retro come posizionamenti separati

Un prodotto può essere solo Fronte, solo Retro oppure Fronte + Retro. Ogni lato ha il proprio artwork, la propria geometria, offset, scala e template.

Questo è necessario quando fronte e retro utilizzano artwork differenti o aree fisiche di stampa diverse. Un semplice flag fronte/retro non conterrebbe abbastanza informazioni per la produzione.

**Compromesso:** il modello di composizione e il pacchetto di produzione sono più complessi rispetto a una semplice impostazione booleana.

## Cosa migliorerei in seguito

La coda di approvazione potrebbe essere più facile da monitorare. Il cliente può già vedere cosa è in attesa di revisione, ma indicatori sul tempo trascorso e notifiche renderebbero più evidente quando qualcosa resta fermo troppo a lungo.
