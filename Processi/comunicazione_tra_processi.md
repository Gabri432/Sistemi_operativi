## Comunicazione tra processi
- Un processo è indipendente se non condivide dati con altri processi in esecuzione, cooperante viceversa;
- I motivi per cui si favorisce la comunicazione tra processi sono la condivisione di informazioni (più applicazioni interessati alla stessa informazione), l'aumento di velocità di computazione (possibile solo con più core) e la modularità (suddivisione in thread e processi separati).

## Comunicazione con memoria condivisa (shared memory)
- I processi che vogliono comunicare stabiliscono una regione di memoria condivisa, la quale di solito risiede nell'indirizzo di spazio del processo che ha creato il segmento di memoria condiviso;
- I due processi quindi acconsentono all'accesso reciproco della memoria (di norma vietata);
- La forma dei dati e la locazione sono determinati da questi processi, non dal S.O.;
- Un processo `produttore` produce informazioni che vengono consumate dal processo `consumatore`, e affinchè possano funzionare in concorrenza si usa un buffer di oggetti che il produttore riempie e il consumatore svuota. Devono essere sincronizzati così che non si acceda ad un buffer vuoto;
- Il buffer può essere `limitato` o `illimitato`, quindi può nel primo caso il produttore non potrà inserire dati se il buffer è pieno.

## Comunicazione con passaggio di messaggio (Message-passing)
- Questo meccanismo consente scambio di dati senza condivisione dello stesso indirizzo di memoria;
- I messaggi possono avere capienza limitata o illimitata. La prima rende l'implementazione facile, la seconda facilita invece il lavoro del programmatore;
- Affinchè due processi comunichino tra di loro ci deve essere un collegamento di comunicazione (communication link);
- La comunicazione può essere `diretta` o `indiretta`, `sincrona` o `asincrona`, col buffering `automatico` o `esplicito`;

### Comunicazione diretta e indiretta
- Nella comunicazione diretta ogni processo deve esplicitamente nominare il mittente della comunicazione;
- In questo modello il collegamento è stabilito automaticamente tra coppie di processi che vogliono comunicare (serve l'identita dei processi), tale collegamento è associato ad esattamente due processi, i quali hanno esattamente un collegamento;
- Il limite è sulla modularità, in quanto cambiare identificatore ad un processo comporterebbe esaminare tutte le altre definizioni del processo;
- Nella comunicazione indiretta i messaggi sono inviati (e ricevuti) tramite mailbox, o porte, il quale ha una univoca identificazione. Quindi due processi possono comunicare solo se condividono la stessa mailbox;
- In questo caso un collegamento può essere associato a più di due processi, e ogni coppia di processi comunicanti può esistere un numero diverso di collegamenti, ciascuno corrispondente ad una mailbox;
- Una mailbox può essere posseduta sia dal processo che dal sistema operativo. Nel primo caso vi è un proprietario (che riceve soltanto) ed un utente (che invia soltanto), e quando la mailbox termina essa scompare, si notificheranno i processi che hanno mandato a questa mailbox. Nel secondo non vi è associazione ad alcun processo, la mailbox gode di esistenza propria;
- Il sistema operativo si occuperà della creazione, invio e ricezione, cancellazione di una mailbox.

### Sincronizzazione
- Blocco dell'invio, il processo mittente è bloccato fino a quando il messaggio non è stato ricevuto dal processo ricevente o dalla mailbox;
- Non-blocco invio, il mittente manda il messaggio e riesuma l'operazione;
- Blocco della ricezione, il ricevente è bloccato fino a quando non sarà disponibile un messaggio;
- Non-blocco ricezione, il ricevente riceve o un messaggio valido o uno nullo.

### Buffering
- Capacità nulla (Zero capacity), il collegamento non può ricevere alcun messaggio che attente al suo interno, il mittente blocca fino a quando il ricevente riceve il messaggio;
- Capacità limitata (Bounded capacity), il collegamento ha una capacità limitata e quindi una volta pieno il mittente dovrà bloccare prima che si liberi un po; 
- Capacità illimitata (Unbounded capacity), una quantità illimitata di messaggi possono essere in attesa in coda, il mittente non blocca mai.


### Canali ordinari (Ordinary pipes)
- Un canale ordinario consente a due processi di comunicare secondo il modello produttore-consumatore, ciascuno che sta ad un capo del tubo;
- Tale sistema è unidirezionale, per cui serviranno due canali per la comunicazione a due sensi;
- I canali possono essere acceduti dai processi con le chiamate `read()` e `write()`. Nessun processo fuori dal canale può accedervi;
- In genere un processo genitore usa un canale per comunicare coi figli.

### Canali con nome (Named pipes)
- Comunicazione bi-direzionale che non necessita del rapporto genitore-figli;
- Utilizzabile da diversi processi, quindi ci possono essere diversi produttori.

### [Vai a schedulazione dei processi](https://github.com/Gabri432/Sistemi_operativi/blob/master/Processi/schedulazione_dei_processi.md)