## Sottosistema del kernel
- Il kernel fornisce diversi servizi relativi al I/O. La schedulazione, il buffering, il caching, lo spooling, prenotazione dei dispositivi e gestione degli errori;
- Inoltre deve proteggere se stesso da processi erranti o utenti malintenzionati.

### Schedulazione I/O
- Si deve determinare un buon ordine in cui eseguire le richieste di I/O. La schedulazione permette di condividere dispositivi tra più processi e permette di ridurre il tempo medio d'attesa per il completamento di una richiesta;
- Viene mantenuta una coda delle richieste per ogni dispositivo e lo scheduler della I/O riordina tale coda per migliorare l'efficienza del sistema e il tempo di risposta;

### Buffering 
- Il buffer è una regione di memoria che immagazina i dati che vengono trasferiti tra due dispositivi o tra un dispositivo e un'applicazione;
- Si ricorre al buffering per `compensare la differenza di velocità tra produttore e consumatore di stream di dati`, per `fornire adattazioni per dispositivi che hanno diverse dimensioni di trasferimento di dati`;
- Per `copiare la semantica delle applicazioni I/O`, cosicchè la versione dei dati scritti nel disco sia garantita di essere la versione dei dati al tempo della chiamata di sistema dell'applicazione, indipendente da seguenti cambiamenti nel buffer dell'applicazione.

### Caching
- La cache è una `regione di memoria veloce che trattiene le copie dei dati`. L'accesso a tali copie è più efficiente che ai dati originali;
- Si distingue dal buffer perchè questo potrebbe trattenere l'unica copia di un oggetto. La cache trattiene la copia di un oggetto che può trovarsi altrove;
- Una regione di memoria può però essere usata per entrambi gli scopi. Si usa un buffer in memoria per tenere i dati di disco per preservare la copia delle semantiche e avere una schedulazione efficiente. Viene poi usato anche come cache per migliorare l'efficienza I/O per i file condivisi da applicazioni o che vengono scritte e ri-lette rapidamente.

### Spooling e Prenotazione del dispositivo
- Lo spool è un buffer che `trattiene l'output per un dispositivo che non può accettare stream di dati provenienti da più processi`;
- Il sistema di spooling copia i file nella coda di spool uno alla volta al dispositivo;
- Lo spooling è un metodo con cui il sistema operativo può coordinare l'output concorrente.

### Gestione degli errori e Protezione
- I dispositivi e i trasferimenti di I/O possono fallire in diversi modi, per cause temporanee, come il sovraccaricamento della rete, oppure per ragione più a lungo termine, come un difetto nel controller del disco;
- Una regola generale è che la chiamata di sistema I/O ritorni un `bit di informazione riguardo lo stato della chiamata`, significando successo o fallimento;
- Per prevenire che gli utenti performino I/O illegale si va a definire tutte le istruzioni I/O come `istruzioni privilegiate`, quindi chiamabili solo attraverso il sistema operativo;
- Per fare ciò il programma utente effettua una chiamata di sistema per chiedere al sistema operativo di performare una I/O in sue veci. Il sistema operativo verifica la legalità della richiesta, e se positiva allora effettua la richiesta per poi ritornare dall'utente.

### Gestione della corrente
- I sistemi operativi gestiscono l'utilizzo della corrente. Se necessario i core della CPU possono essere sospesi se il sistema non li richiede e li richiama quando il carico di lavoro cresce;
- Lo stato dei core, se sospesi, deve essere salvato e ripristiato al riavvio.

### Performance
- La gestione di I/O è un fattore importante nelle performance di sistema. Si va a piazzare molta richiesta alla CPU per eseguire il codice del device-driver da eseguire e per schedulare i processi in modo equo ed efficiente;
- I cambi di contesto stressano la CPU e la cache, la I/O inoltre espone le inefficienze nei meccanismi del kernel di gestione degli interrupt;
- Per migliorare l'efficienza della I/O si può `ridurre il numero di cambi di contesto`, `ridurre il numero di volte che i dati devono essere copiati in memoria` mentre passati tra dispositivi e applicazione;
- Si può `ridurre la frequenza degli interrupt` usando trasferimenti larghi, controller più intelligenti e il polling (minimizzando busy waiting), `incrementare la concorrenza`;
- Si può `bilancaire la CPU`, il sottosistema della memoria, il bus e le performace I/O perche il sovraccaricamento di un'area causa stato di attesa in altre.