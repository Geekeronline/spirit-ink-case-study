# Responsabilità sul progetto

## Mantenere il lavoro coordinato

Il progetto coinvolge diversi servizi collegati tra loro, quindi ho mantenuto una fonte di verità aggiornata sullo stato del sistema e verificato la configurazione live prima di apportare modifiche.

Questo è stato particolarmente importante quando il lavoro proseguiva in sessioni diverse o coinvolgeva più piattaforme. Se la documentazione e il sistema live non coincidevano, consideravo quella discrepanza come qualcosa da risolvere prima di cambiare altro.

## Apportare modifiche in sicurezza

Ho cercato di mantenere le modifiche live circoscritte e reversibili. Prima di intervenire su uno scenario Make, ne controllavo lo stato corrente e modificavo soltanto la parte coinvolta nel problema.

Per azioni a rischio più elevato, come la creazione di ordini presso i fornitori o la pubblicazione visibile ai clienti, il sistema utilizza controlli espliciti o una revisione umana invece di assumere che uno stato incerto sia sicuro.

## Lavorare con un cliente non tecnico

Ho tradotto gli stati tecnici in azioni successive chiare.

Per esempio, invece di descrivere un'opzione prodotto come "fail-closed pending authoritative data", avrei spiegato che non poteva ancora essere venduta perché il fornitore non aveva confermato l'identificativo richiesto, indicando poi con precisione quale informazione fosse necessaria.

Ho usato lo stesso approccio per gli aggiornamenti di stato. Costruito, distribuito, testato e dimostrato tramite un ordine reale sono fasi diverse, quindi ho mantenuto chiare queste distinzioni nel comunicare l'avanzamento.

## Uso dell'AI durante l'implementazione

Ho utilizzato ampiamente agenti AI per il coding durante il processo di implementazione. Sono rimasto responsabile dei requisiti, delle decisioni architetturali, dei vincoli forniti agli agenti, della validazione rispetto al sistema live e dell'accettazione finale delle modifiche.

Questo significava verificare le modifiche generate rispetto al sistema reale, invece di considerare un output plausibile come prova che qualcosa funzionasse.

## Handoff

Ho mantenuto aggiornata la documentazione del progetto durante tutto il lavoro e preparato un handoff finale che descrive l'architettura live, i punti ancora da dimostrare e i vincoli operativi senza dipendere da vecchie chat o note di debugging.
