## Sistemi di I/O
- Il ruolo del sistema operativo è quello di gestire e controllare operazioni e dispositivi di I/O;
- I componenti hardware di base come porte, bus e device controller accomodano una varietà di dispositivi di I/O;
- I driver dei dispositivi presentano una uniforme interfaccia d'accesso ai sottosistemi di I/O;

### Componenti hardware
- Il `port`, è un punto di connessione che consente la comunicazione tra dispositivo e la macchina;
- Il `bus`, è un insieme di cavi in condivisione tra più dispositivi, e ha un protocollo definito rigidamente che specifica l'insieme di messaggi mandabili in quei cavi; 
- Il `controller`, è un insieme di componenti che può operare su porte, bus o dispositivi.

### Registri per il controllo I/O
- Il `registro dei dati in ingresso`, è letto dall'host per ottenere l'input;
- Il `registro dei dati in uscita`, è usato dall'host per mandare l'output;
- Il `registro di stato`, contenente bit per indicare lo stato, come capire se il comando corrente è stato completato, o vi è un errore nel dispositivo;
- Il `registro di controllo`, dove l'host può scrivere per iniziare un comando o cambiare modalità di un dispositivo.

### Polling (Busy Waiting)
- L'interazione tra host e controller avviene attraverso l'`handshaking`. Il controller indica il suo stato di disponibilità col `busy bit` nel registro di stato;
- L'host segnala il comando tramite il bit `command-ready` nel registro di controllo;
- L'host continua a leggere il busy bit fino a quando non è zero. L'host dunque stabilisce un comando col bit command-ready;
- Il controller vede il segnale ed esegue il comando. L'handshaking si conclude risettando tutti i bit a zero;
- Una soluzione più efficiente è l'uso degli interrupt.

### Interrupt
- La CPU controlla un cavo detto `linea di richiesta-interrupt` dopo ogni esecuzione di un istruzione;
- Quando avverte il segnale di un controller allora la CPU effettua un salvataggio dello stato e salta alla procedura di gestione dell'interrupt. Viene quindi stabilita la causa, effettuate le operazione di ripristino e la CPU riprende l'esecuzione da dove si è interrotta;
- In sistemi moderni si vuole inoltre ritardare la gestione degli interrupt in processioni critiche;
- Oltre ad assegnare in modo efficiente corretto gestore dell'interrupt senza prima andare in polling con tutti i dispositivi;
- Avere interrupt a più livelli di priorità;
- Ottenere l'attenzione del sistema operativo direttamente per certi errori;
- Alcune CPU hanno due linee di richieste di interrupt, mascherabili e non.

### Accesso diretto alla memoria
- Per un dispositivo che effettua grandi spostamenti di dati è preferibile non usare un sistema che dipenda da un bit. Quindi si ricorre al controller del DMA;
- Per iniziare un trasferimento in questa modalità  l'host scrive un blocco di comando DMA in memoria, il quale contiene un puntatore alla destinazione del trasferimento e un conteggio del numero di byte da trasferire;
- La CPU scrive l'indirizzo del blocco al controller del DMA e si incarica di altro lavoro;
- Il DMA controller opera direttamente sul bus, piazzando gli indirizzi in esso per effettuare trasferimenti senza l'aiuto della CPU;
- Una seconda operazione è necessaria per ottenere i dati trasferiti dal DMA allo spazio utente per l'accesso dei thread. Si genera un doppio buffering inefficiente.

### [prossimo](https://github.com/Gabri432/Sistemi_operativi/blob/master/Input_output/parte_2.md)