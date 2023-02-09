## Schedulazione dei processi
- Si fa utilizzo dello schedulatore di processi (process scheduler) che si occupa di trovare un processo disponibile all'esecuzione e inserirlo in un core;
- Ci sono processi con limite di I/O (I/O bound process) e di CPU (CPU bound process).

### Code di schedulazione
- Quando un processo arriva nel sistema viene collocato nella coda di processi pronti (ready queue), altrimenti se sta attendendo per un evento che accada allora viene posto nella coda di attesa (wait queue);
- Durante l'esecuzione di un processo questo può avere una richesta di I/O ed essere posto nella coda d'attesa, creare processi figli ed attendere nella coda d'attesa la terminazione di quest'ultimi, venire rimosso forzatamente (magari per via di un interrupt o se oltre il limite di tempo) e ricollocato alla coda dei processi pronti;
- La CPU scheduler deve allocare un core ad uno dei processi in ready queue;
- Altri sistemi operativi invece adottano la tecnica dello `swapping`, dove talvolta un processo può venire rimosso dalla memoria per poi venire reinserito in essa e riprendere la sua esecuzione;
- Quando accade un interrupt si fa ricorso al salvataggio dello stato del core della CPU, per poi la ripresa delle operazioni. Questo processo è chiamato Cambio di Contesto (Context Switch).

### Algoritmi preemptive e non preemptive
- Ci sono quattro circostanze nelle quali è necessario fare decisioni di CPU-scheduling;
- Quando un processo passa dallo stato di esecuzione a quello di attesa, da esecuzione a pronto per l'esecuzione, da stato di attesa a quello di pronto per l'esecuzione, alla sua terminazione;
- Nei casi primo ed ultimo lo schema di schedulazione è non-preemptive, o cooperativo. Negli altri due è preemptive;
- In una circonstanza non-preemptive la CPU è allocata ad un processo, il quale occuperà la CPU fino alla sua terminazione o passando allo stato di attesa. Questo schema non è compatibile con la computazione in tempo reale;
- Nella circostanza preemptive invece vi può essere il problema di dipendenza sui dati tra molteplici processi. I dati letti da un processo potrebbero essere inconsistenti. Perciò richiede meccanismi come mutex lock (mutex, mutual exclusion) per prevenire tali errori.

### Dispatcher
- Il dispatcher è il modulo che dà controllo della CPU al processo selezionato dalla CPU scheduler;
- Nel fare ciò deve cambiare contesto da un processo ad un altro, passsare alla modalità utente e saltare alla locazione corretta nel programma dell'utente per riesumare quel programma;
- La latenza del Dispatcher (Dispatcher latency) è la quantità di tempo necessaria per fermare un processo ed iniziarne un altro.

### Algoritmi di scheduling della CPU

#### Criteri
- `Utilizzo della CPU`, deve essere il più alto possibile, idealmente tra il 40% e il 90%;
- `Throughput`, il numero di processi completati per unità di tempo dovrebbe essere idealmente alto;
- `Tempo di consegna` (Turnaround time), il tempo necessario per eseguire un processo, cioè la somma dei tempi di attesa nella coda dei processi pronti, di esecuzione, e di operazioni I/O;
- `Tempo di attesa`, il tempo speso nella coda dei processi pronti;
- `Tempo di risposta`, il tempo di attesa fino alla prima risposta dall'inizio dell'invio della richiesta.

#### First-Come, First-Scheduled
- Il processo che fa richiesta per primo è quello che avrà la CPU per primo;
- Quando il processo entra nella coda dei processi pronti, il blocco di controllo del processo è collegato alla fine della coda (FIFO).
- Pur essendo un algoritmo semplice da scrivere e capire il principale svantaggio è il tempo di attesa medio, il quale può essere elevato.
- Si genera un effetto convoglio dovuto all'attesa di un processo molto lungo che termini, e quindi causa un basso utilizzo della CPU e del dispositivo.
- Questo è un algoritmo non-preemptive.

#### Shortest-Job-First Scheduling
- Per ogni processo vi è un'associazione della durata del consumo della CPU. Il processo che consuma meno tempo è quello che verrà eseguito per primo. Se due processi hanno stessa durata si userà l'algoritmo precedente.
- Il vantaggio è che si va ad abbassare il tempo medio di attesa, tuttavia non c'è un modo per conoscere in anticipo quanto tempo richiede un processo;
- Si passa ad una stima con la media esponenziale: τn+1 = α tn + (1 − α)τn; 
- Questo algoritmo può essere sia preemptive oppure non-preemptive, quindi in presenza di un processo più corto di quello corrente il primo lo metterà in pausa, il secondo consentirà al processo corrente di terminare.

#### Round-Robin Scheduling
- Assomiglia all'algoritmo First-Come-First-Scheduled, ma viene introdotto il quanto di tempo;
- Un processo può occupare la CPU per tutta la durata del quanto. Se il processo è più breve allora termina regolarmente. Altrimenti viene interrotto, causando un interruzione del sistema operativo, e verrà collocato alla fine della coda dei processi pronti. La CPU scheduler quindi selezionerà il prossimo processo pronto all'esecuzione;
- Le performance di questo algoritmo dipendono dalla dimensione del quanto di tempo. Uno troppo ampio causerebbe gli stessi problemi del FCFS. Uno troppo breve e troppi cambiamenti di contesto andrebbero eseguiti.

#### Priority Scheduling
- L'algoritmo SJF è un tipo di algoritmo basato sulle priorità. In questa tipologia ogni processo è associato ad una priorità;
- Il processo con la priorità più importante (che può essere la più alta o la più bassa a seconda del sistema) sarà il primo al quale verrà allocata la CPU;
- La priorità può essere interna e quindi basata su quantità misurabili come il numero di file aperti, la durata di un'operazione di I/O, i requisiti di memoria, ecc... oppure esterna, e quindi da criteri fuori dal sistema operativo, come la sua importanza;
- Tale classe di algoritmi può essere preemptive o non-preemptive. Il primo cambia processo da eseguire se ne arriva uno con priorità più importante, il secondo lo mette all'inizio della coda dei processi pronti.
- Un problema di questo algoritmo è la `starvation`, cioè un processo potrebbe dover attendere indefinitamente per via di altri processi con priorità maggiore;
- Una soluzione è data dall'`invecchiamento` dei processi, dove quindi la priorità del processo evolve nel tempo. Quindi con priorità bassa possono dopo una certa attesa guadagnare una priorità più elevata e venire eseguiti;
- Un'altra soluzione è una combinazione il Round-Robin con algoritmi di priorità cosicchè processi con la stessa priorità vengano eseguiti con l'ordine di RR.

#### Multilevel Queue Scheduling
- I processi vengono raggruppati in diverse code in base alla loro priorità, in modo tale da semplificare la ricerca del processo a priorità maggiore;
- Questa tecnica inoltre funziona bene in combinazione con la RR, che stabilisce quali processi eseguire quando questi hanno medesima priorità;
- Un utilizzo comune di questo algoritmo è nella divisione di processi in *foreground* e in *background*. Questi due gruppi hanno diversi requisiti di tempi di risposta e quindi di scheduling. Ciascuna coda può quindi avere il suo algoritmo.

#### Multilevel Feedback Queue Scheduling
- Simile al precedente ma reintroduce il concetto di invecchiamento. I processi possono essere spostati in diverse code anzichè attendere che tutti i processi della coda con priorità maggiore siano terminati.


### Scheduling in più processori
- Vi è il multiprocessing `asimmetrico`, dove un solo core accede alle strutture dati del sistema riducendo così la necessità per la condivisione dei dati;
- Un solo core quindi si occupa di tutte le decisioni di schedulazione, processione di I/O, ecc...;
- Lo svantaggio è in termini di performance, vi è un collo di bottiglia con questo approccio;
- Vi è il multiprocessing `simmetrico`, dove ogni processore fa auto-schedulazione. Vi è quindi uno scheduler per ogni processore che esamina quale thread è da eseguire;
- Ciò permette due possibilità, tutti i thread potrebbero stare nella stessa coda, oppure ogni processore può avere una propria coda di thread.