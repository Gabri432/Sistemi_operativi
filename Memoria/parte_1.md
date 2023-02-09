## A cosa serve?
- Per migliorare l'utilizzo della CPU e la velocità di risposta del computer è necessario che vengano caricati molti processi in memoria. Per ottimizzare questa operazione è necessario sviluppare tecniche per la gestione della memoria.

### Collegamento dell'indirizzo
- Quando un processo viene eseguito questo accede alle istruzioni e ai dati dalla memoria. Al termine la memoria usata viene reclamata;
- Molti sistemi consentono al processo utente di risiedere in ogni parte della memoria fisica, e prima di essere eseguito affronta diversi passi;
- In genere il collegamento delle istruzioni e dei dati negli indirizzi di memoria può essere svolto in ogni momento tra i seguenti momenti;
- `Durante la compilazione`, sapendo dove a tempo di compilazione risiede il processo in memoria allora il codice assoluto viene generato;
- `Tempo di caricamento`, se non è noto a tempo di compilazione il compilatore dovrà generare del codice rilocabile e il collegamento finale è ritardato fino al caricamento;
- `Tempo di esecuzione`, se il processo può essere mosso da un segmento di memoria all'altro durante la sua esecuzione allora il collegamento deve essere ritardato fino al tempo di esecuzione.

### Spazio di indirizzo logico vs fisico
- Un indirizzo generato dalla CPU è chiamato `indirizzo logico`, mentre se è visto dall'unità di memoria è detto `indirizzo fisico`;
- Il collegamento a tempo di compilazione genera indirizzi fisici e logici identici, diversamente dal tempo di esecuzione. In questo secondo caso l'indirizzo logico è detto `indirizzo virtuale`. Dunque l'insieme degli indirizzi logici generati da un programma è detto `spazio degli indirizzi logici`. Mentre l'insieme degli indirizzi fisici associati è detto `spazio degli indirizzi fisici`;
- L'`unità di gestione della memoria` si occupa del mappamento a run-time dall'indirizzo virtuale a quello fisico, e perciò il programma dell'utente non accederà mai all'indirizzo fisico reale. Può solo generare e usare quelli logici.

### Caricamento dinamico
- Fino ad ora la memoria fisica limitava la dimensione massima dei processi;
- Per migliorare l'utilizzo della memoria si fa ricorso al caricamento dinamico cosicchè una procedura venga caricata solo in caso di necessità;
- Diventa utile anche per gestire grandi quantità di codice poichè la porzione in uso (quindi caricato) può essere più piccola;
- Inoltre il caricamento dinamico non necessità del supporto del sistema operativo. Chi programma si incarica di sfruttare questo metodo al meglio.

### Collegamento dinamico e librerie condivise
- Le librerie dinamicamente collegate sono librerie che si collegano al programma dell'utente in fase di esecuzione;
- Alcuni sistemi operativi supportano solo il collegamento statico dove le librerie sono trattate come ogni altro modulo e combinate dal caricatore in una immagine di un programma binario;
- Il collegamento dinamico è postposto all'esecuzione, riducendo lo spreco di memoria. In più queste librerie possono essere condivise tra più processi, motivo per cui sono conosciute anche come librerie condivise;
- Le librerie per il collegamento dinamico possono essere estese ad aggiornamenti di libreria. Senza collegamento dinamico tutti i programmi dovrebbero ricollegarsi per ottenere accesso alla nuova libreria;
- Generalmente queste librerie necessitano assistenza del sistema operativo. Se due processi sono protetti tra di loro solo il sistema operativo allora può verificare che la procedura necessaria si trovi nello spazio di memoria di un altro processo.


### Allocazione di memoria contigua
- La memoria deve essere allocata nel modo più efficiente possibile, così da poter accomodare il sistema operativo e i vari processi dell'utente. La memoria è quindi suddivisa in due parti, una per il s.o., l'altra per i processi utente;
- Nella memoria contigua ciascun processo è contenuto in una singola sezione della memoria che è contigua alla sezione di memoria del prossimo processo;

#### Protezione
- Per prevenire che un processo acceda ad uno spazio di memoria non consentito si può utilizzare il registro di rilocazione insieme al registro limite. Il primo contiene il valore del più piccolo indirizzo fisico. Il secondo contiene il range degli indirizzi logici;
- Ogni indirizzo logico deve essere contenuto nel range specificato dal registro limite. La MMU mappa dinamicamente gli indirizzi logici aggiungendo il valore nel registro di rilocazione. L'indirizzo mappato è passato alla memoria;
- Quando la CPU scheduler seleziona un processo per l'esecuzione il dispatcher carica i due registri con i corretti valori come parte del cambio di contesto;
- Dato che ogni indirizzo generato dalla CPU è verificato rispetto a questi registri si può proteggere il sistema operativo e gli altri programmi e dati dell'utente dal processo in esecuzione.

### Allocazione della memoria
- Uno degli schemi più semplici è quello di assegnare i processi a porzioni di memoria di grandezza variabile. Con questo schema il sistema operativo tiene traccia di una tabella dove sono segnate quali parti della memoria sono libere od occupate;
- Il sistema operativo alloca memoria al processo in esecuzione e la reclama in terminazione;
- Quando la memoria è esaurita si può o rifiutare il processo e inviare un messaggio d'errore oppure inserire il processo in una coda d'attesa fino a sufficiente liberamento di memoria;
- All'atto di allocazione della memoria, se lo spazio dato è troppo ampio questo viene diviso in due parti, una per i processi in arrivo, l'altra reinserita nell'insieme di memoria libero;
- Questa procedura è un'istanza del `Problema di allocazione dello spazio dinamico`.

### Soluzioni
- `First fit`, si alloca il primo spazio di memoria libero grande a sufficienza;
- `Best fit`, si alloca lo spazio più piccolo di memoria grande a sufficienza;
- `Worst fit`, si alloca lo spazio più grande possibile.

### Frammentazione
- I primi due metodi soffrono di frammentazione `esterna`. Come i processi vengono caricati e scaricati in e dalla memoria vi può esserci abbastanza spazio, ma non contiguo, per soddisfare una richiesta. Nel peggiore dei casi vi è spazio sprecato tra ogni coppia di processi;
- La frammentazione può essere anche `interna`. Cioè vi è memoria non usata interna ad una partizione, poichè la memoria allocata ad un processo può essere maggiore di quella richiesta;
- La frammentazione esterna si risolve rimettendo insieme gli spazi di memoria liberi in un unico blocco, ma non è sempre possibile. Può avvenire solo con la rilocazione dinamica ed è effettuata a run-time. Inoltre è un procedimento costoso;
- Una seconda soluzione è consentire l'esistenza di spazio di indirizzi logici non contiguo, cioè di allocare memoria fisica ad un processo quando questa è disponibile. Questa strategia è detta `paginazione`.