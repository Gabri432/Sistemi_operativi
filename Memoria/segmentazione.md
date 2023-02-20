## Segmentazione
- Con questa tecnica il sistema operativo suddivide la memoria in segmenti o sezioni non necessariamente delle stesse dimensioni;
- Una referenza ad una locazione di memoria consiste in un valore che identifica un segmento ed un offset all'interno di quel segmento;
- I segmenti corrispondono a divisioni naturali del programma, come routine individuali o tabelle di dati. In generale quindi la segmentazione è più visibile al programmatore della paginazione;
- Il segmento base è associato ad ogni segmento e ne indica la posizione in memoria;
- Quando un programma fa una referenza ad una locazione di memoria l'offset è aggiunto al segmento di base per generare l'indirizzo fisico; 

### Implementazione hardware
- Gli indirizzi di memoria si compongono da un id del segmento e un offest all'interno del segmento. La traduzione da indirizzo logico a fisico avviene allo stesso modo della paginazione;
- Ogni segmento ha una lunghezza e un insieme di permessi ad esso associati. Un processo può referenziare nel segmento solo se il tipo di referenza è ammesso dai permessi, e se l'offset nel segmento è dentro il range specificato dalla lunghezza del segmento;
- La segmentazione può essere utilizzata per implementare la memoria virtuale. Quindi ogni segmento è associato ad un flag che ne indica la presenza o meno in memoria. Tale implementazione può però causare frammentazione della memoria poichè è necessario muovere interi segmenti dentro e fuori la memoria;

### Segmentazione con paginazione
- Anzichè avere il segmento di memoria, l'informazione nel segmento include l'indirizzo di una tabella delle pagine per il segmento. Quando un programma fa una referenza alla locazione di memoria l'offset viene tradotto ad un indirizzo di memoria usando questa tabella; 
- Un segmento può essere esteso allocando un'altra pagina di memoria e aggiungendola alla tabella delle pagine del segmento;
- Una implementazione della memoria virtuale con la segmentazione con paginazione di solito muove solo singole pagine (anzichè interi segmenti) dalla memoria principale all'archiviazione secondaria. Le pagine di un segmento possono essere situate ovunque nella memoria principale e non hanno bisogno di essere contigue.
- Questo risulta in una riduzione della quantità di I/O tra memoria principale e archiviazione secondaria e in una riduzione della frammentazione della memoria.

### Riepilogo

#### Vantaggi
- La segmentazione non soffre di frammentazione interna;
- Le tabelle dei segmenti richiedono meno spazio delle tabelle delle pagine nella paginazione;
- Gli utenti possono suddividere i programmi in moduli tramite segmentazione, che non sono altro che la separazione dei codici dei processi;
- La dimensione del segmento è specificata dall'utente, quella della pagina invece dall'hardware;
- La segmentazione permette di separare i dati da operazioni di sicurezza;

#### Svantaggi
- La segmentazione soffre di frammentazione esterna perchè la memoria libera viene distribuita in porzioni minori man mano che i processi vengono caricati e rimossi dalla memoria;
- Vi è una complicazione nel mantenere una tabella dei segmenti per ogni attività;
- Il tempo per accedere all'istruzione aumenta poichè è necessario effettuare due accessi in memoria, uno per la tabella dei segmenti e uno per la memoria principale.