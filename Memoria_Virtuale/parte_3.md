## Algoritmi di allocazione dei frame

### Numero minimo di frame
- Si deve allocare almeno il minor numero di frame necessari per questioni di performance. Il punto è che la frequenza dei page-fault aumenta con il diminuire del numero di frame associati ad un processo, rallentandone così l'esecuzione;
- Inoltre se un page-fault si verifica prima del completamento dell'esecuzione di una istruzione questa deve essere riavviata;
- Il minimo numero di frame è definito dall'architettura del computer, mentre il loro massimo numero è definito dalla quantità disponibile di memoria fisica.

#### Allocazione paritaria (Equal Allocation)
- Il metodo più semplice è quello di distribuire in modo equo i frame tra i vari processi. I frame avanzati possono essere raccolti in un buffer di frame liberi. 

#### Allocazione proporzionale (Proportional allocation)
- Con questo metodo si tiene invece conto della dimensione dei processi e si distribuiscono i frame in modo proporzionato;
- Da notare che sia nella Equal Allocation che nella Proportional Allocation i processi godono dello stesso livello di priorità. In scenari ideali però alcuni processi potrebbero avere maggiore importanza (e quindi priorità) rispetto ad altri.

### Allocazione Globale vs Locale (Global vs Local Allocation)
- Nell'allocazione dei frame anche il rimpiazzamento della pagina è un fattore da dover tenere conto. Si va infatti a classificare gli algoritmi di `rimpiazzamento globale (global replacement)` e quelli `rimpiazzamento locale (local replacement)`;
- Nel rimpiazzamento globale un processo può selezionare un frame di rimpiazzamento nell'insieme di tutti i frame, anche se alcuni di quei frame sono stati già allocati ad altri processi;
- Nel rimpiazzamento locale la selezione è possibile solo tra i frame allocati allo stesso processo selezionante;
- Un problema col rimpiazzamento globale è che l'insieme di pagine in memoria dipende non solo dal tipo di paginazione del processo, ma anche dal tipo di paginazione degli altri processi;
- Il rimpiazzamento locale non soffre di questo problema, ma può ostacolare un processo non fornendogli altre pagine meno usate. Motivo per cui il rimpiazzamento globale è più utilizzato, inoltre vanta maggiore throughtput;
- Un altro accorgimento è quello di attivare il rimpiazzamento di pagina superato un certo valore limite anzichè attendere il completo esaurimento dei frame. In modo tale da lasciare sufficiente memoria libera per gestire nuove richieste.
- Si ricorre a delle kernel routine dette `reaper (raccoglitore, lett. "mietitore")`;

### Thrashing
- Quando un processo non ha abbastanza frame e tutte le pagine sono attive, e quindi il rimpiazzamento di una di queste causerebbe un page-fault che la ripristinerebbe in seguito, si incappa in una situazione di `thrashing`. Un processo è in thrashing se spende più tempo in paginazione che in esecuzione;
- La frequenza dei page-fault può aumentare tremendamente. Il thrashing causa un decremento dell'utilizzo della CPU, la CPU scheduler per compensare inserisce nuovi processi in esecuzione. Questi processi faranno richiesta di altre pagine, potenzialmente causando altri page-fault;
- Un modo per limitare tale situazione è l'utilizzo del rimpiazzamento locale. Quando un processo va in thrashing non può più chiedere frame da altri processi. Tuttavia i processi in thrashing andranno nella coda del dispositivo di paginazione, e aumenterà il tempo di servizio del page-fault;

#### Modello di località (locality model)
- Una tecnica per prevenire il thrashing è usando il `modello di località (locality model)` dell'esecuzione di un processo. Una località è un insieme di pagine attivamente usate insieme, e i processi in esecuzione si muovono tra più località; 
- Queste località sono definite dalla struttura del programma e le sue strutture dati. Un processo va quindi in thrashing quando non ci sono abbastanza frame per accomodare la sua località corrente;

#### Modello di set di lavoro (Working-set model)
- Questo modello si basa sull'assunzione di località e definisce una `finestra di set di lavoro (working-set window)`;
- Si esaminano le referenze di pagine più recenti. Il set di pagine nelle referenze di pagine più recenti è il working-set. Quindi una pagina attiva si troverà nel working-set, se non viene più usata uscirà dal working-set;
- La proprietà più importante del working-set è la sua dimensione e la somma dei working-set di tutti i processi nel sistema compone la domanda totale di frame. Se la domanda totale supera il numero di frame disponibili allora vi sarà thrashing;
- Il vantaggio di questo modello è che previene il thrashing mantenendo alto il grado di multiprogrammazione, quindi ottimizza l'utilizzo della CPU, ma la difficoltà sta nel tenere traccia di tale working-set;
- La working-set window varia, poichè ad ogni referenza di memoria una nuova referenza viene aggiunta e quella vecchia viene rimossa. Una pagina è nel working-set se viene referenziata ovunque nel working-set window;
- Si può approssimare il working-set con un bit di referenza ed un interrupt che funge da timer a intervallo fissato.  

### Frequenza del page-fault
- Il working-set model pur fornendo conoscenza del working-set complica la gestione del thrashing. Un approccio diretto è quello della `frequenza del page-fault (page-fault frequency)`;
- Si stabiliscono un limite superiore ed inferiore di frequenza dei page-fault (rispettivamente per troppi pochi e troppi frame allocati a ciascun processo). Superato il limite superiore si allocano nuovi frame al processo, superato quello inferiore si rimuovono frame al processo;
- Così facendo si può controllare e misurare direttamente la frequenza dei page-fault per prevenire il thrashing. Mentre con il working-set model si doveva fare ricorso allo swapping. 