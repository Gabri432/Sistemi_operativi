## Prepaginazione
- Una proprietà della paginazione su domanda è che causa molti page-fault all'avvio di un processo, poichè si fa richiesta di pagine non presenti in memoria. Per prevenire ciò si fa ricorso alla `prepaginazione (prepaging)`, con la quale vengono caricate in memoria una parte, o la totalità, delle pagine necessarie in un singolo momento. L'unica domanda è se risulta più conveniente tale caricamento oppure servire i page-fault corrispondenti;
- Si potrebbero caricare in memoria molte pagine che però non verranno più utilizzate. Inoltre la prepaginazione di un programma eseguibile è difficile, essendo poco chiaro quali pagine siano da portare in memoria. Per i file è più semplice perchè spesso l'accesso a questi è sequenziale.

### Dimensione di una pagina (Page size)
- Se le pagine sono più piccole allora ce ne possono stare di più in una tabella delle pagine. Dato che ogni processo attivo deve avere una copia propria della tabella delle pagine avere pagine più grandi è ideale. Tuttavia la memoria viene utilizzata meglio se le pagine sono più piccole;
- Un altro aspetto è il tempo richiesto per leggere e scrivere in una pagina. Il tempo di trasferimento è direttamente proporzionale alla dimensione della pagina;
- Minimizzare il tempo di I/O comprende aumentare la dimensione della pagina, ma pagine più piccole il totale I/O dovrebbe venire ridotto, visto che si migliora la località. Pagine piccole consentono ai programmi di avere una più precisa corrispondenza tra località del programma e pagina;
- Pagine più piccole migliorano la risoluzione, riducono il numero di I/O e di memoria allocata totale;
- Pagine più grandi riducono il numero di page-fault. I sistemi moderni tendono a questo approccio.

### Tabelle dalle pagine invertite
- Con questo metodo si riduce la quantità di memoria fisica necessaria per tenere traccia delle traduzioni di indirizzi da virtuali a fisici. Tuttavia non contengono più informazioni riguardanti lo spazio di indirizzamento logico di un processo.

### Struttura del programma
- Le performance di sistema possono migliorare se l'utente prende coscienza dell'attivazione della paginazione su richiesta. Una più attenta scelta delle strutture dati e delle strutture di programmazione può migliorare la località e quindi ridurre la frequenza di page-fault e il numero di pagine nel working-set.