## Cos'è un sistema operativo?
- Un sistema operativo è un programma responsabile per la `gestione` dei componenti hardware dei computer. Ossia agisce come intermediario tra le risorse hardware (CPU, memoria, dispositivi I/O, ecc...) e i programmi applicativi (browser, compilatori, ecc...). Nello specifico il sistema operativo ha il compito di stabilire quali programmi hanno accesso a quali risorse hardware, con l'obiettivo di permettere all'utente di utilizzare il computer nel modo più efficiente.


## Che cosa fa un sistema operativo?
### Gestione dei processi
- Creazione e cancellazione dei processi dell'utente e di sistema;
- Schedulazione dei processi (e dei thread) nella CPU;
- Sospensione e riavvio dei processi;
- Gestione della comunicazione e sincronizzazione tra processi.

### Gestione della memoria
- Tracciamento delle porzioni di memoria in utizzo e da quali processi;
- Allocazione e deallocazione dello spazio di memoria a seconda della richiesta;
- Decisione di quali processi e dati debbano essere mossi da e nella memoria.

### Gestione del file-system
- Creazione e cancellazione dei file;
- Creazione e cancellazione delle directories (cartelle) per organizzare i file;
- Supporto di modalità per la manipolazione di file e cartelle;
- Mappatura dei file nella memoria di massa;
- Back up dei file in dispositivi di archivio.

### Gestione dell'archiviazione
- Caricamento e scaricamento;
- Gestione dello spazio libero;
- Allocazione dello spazio;
- Schedulazione del disco;
- Partizione dello spazio;
- Protezione.

### Gestione della cache
- Selezione della dimensione della cache;
- Selezione di una policy di aggiornamento dei dati (Replacement Policy);
- Trasferimento dei dati da disco a memoria.

### Gestione I/O
- Nascondere specifiche hardware dall'utente;
- Gestione dei sottosistemi di I/O.

### Protezione e sicurezza
#### Protezione
- Controllo dell'accesso dei processi o degli utenti alle risorse;
- Rilevamento degli errori di interfaccia tra componenti dei sottosistemi;
#### Sicurezza
- Difesa da attacchi esterni o interni al sistema.



## Servizi del sistema operativo
- Interfaccia utente, che può essere grafica (GUI), touch-screen oppure una linea di comando;
- Esecuzione dei programmi, dunque il loro caricamento e avviamento in memoria, così come la loro terminazione;
- Operazioni I/O, devono essere forniti gli strumenti per effettuare operazioni di input e output, normalmente non direttamente controllabili dall'utente per ragioni di sicurezza;
- Manipolazione del file-system, cioè sia gestione dei file medesimi che dei permessi a loro (o alle directories) associati.
- Comunicazione tra processi, quindi via la condivisione della memoria o passaggio di un messaggio;
- Rilevamento (e correzione) degli errori, sia a livello hardware, sia in I/O, sia tra i programmi dell'utente;
- Allocazione delle risorse;
- Registro delle attività, cioè dell'utilizzo delle risorse da parte di quali processi (o utenti);
- Protezione e sicurezza.

### [prosegui](https://github.com/Gabri432/Sistemi_operativi/blob/master/Architettura/parte_2.md)
