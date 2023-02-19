### Copiatura durante scrittura (Copy-on-write)
- Tradizionalmente la chiamata fork() di un processo creava una copia dello spazio di indirizzamento del processo genitore per il figlio, duplicando le pagine associate/appartenenti al genitore. Tuttavia dato che molti processi figli chiameranno exec() subito dopo la creazione, tale copiatura non è necessaria;
- Si può in alternativa ricorrere alla `Copiatura durante scrittura (Copy-on-write)`. Questa tecnica prevede che il processo genitore e quelli figli condividano inizialmente le stesse pagine. Quando il processo figlio vuole modificare una pagina il sistema operativo otterrà un frame dalla lista dei frame liberi e creerà una nuova copia di quella pagina, per poi mapparla nello spazio di indirizzamento del processo figlio. Il processo figlio modificherà quindi tale copia;
- Con questo metodo solo le pagine modificate vengono copiate, tutte le altre vengono messe in condivisione tra processi genitore e figli.

### Rimpiazzamento della pagina (Page replacement)
- Con la demand-paging i processi che vengono eseguiti ricevono le pagine di cui hanno bisogno. Si ha quindi una `sovra-allocazione (over-allocation)`, dove si ottiene un maggiore throughput e utilizzo della CPU perchè si è in grado di eseguire più processi di quelli eseguibili se a ciascuno di essi venissero caricate tutte le pagine ad esso associate;
- Tuttavia alcuni processi potrebbero in seguito richiedere altre pagine che però non sono disponibili (essendo state caricate solo quelle richieste all'inizio e il resto dello spazio potrebbe essere stato allocato alle pagine di altri processi);
- Questo succede quando durante l'esecuzione di un processo avviene un page fault e il sistema operativo, dopo aver determinato che la pagina desiderata risiede nell'archiviazione secondaria, *NON* trova frame liberi nella lista dei frame liberi. Ergo tutta la memoria è in utilizzo;
- Il sistema operativo potrebbe decidere quindi di terminare il processo, tuttavia l'utilizzo del demand-paging è proprio un tentativo del s.o. di incrementare il throughput e l'utilizzo della CPU. Idealmente gli utenti dovrebbero essere inconsci dell'applicazione di tale tecnica, per cui è un'opzione non conveniente;
- Quindi può fare ricorso allo swapping del processo e muovere il processo fuori dalla memoria, liberando così i frame ad esso associati. Tuttavia molti sistemi operativi non ricorrono più a questa tecnica per la complicazione legata alla copiatura di interi processi tra memoria e spazio di swap;
- Molti sistemi operativi ricorrono quindi al `rimpiazzamanto della pagina`.

#### Rimpiazzamento base (Base page replacement)
- Quando nessun frame è libero si cerca un frame non utilizzato al momento e lo si libera. Ciò avviene scrivendo il contenuto di tale frame nello spazio di swap e cambiando la tabella della pagina per indicare che tale pagina non è più in memoria;
- Dunque si ha ora a dispozione un frame da allocare al processo che ha causato un page-fault. Si va allora a modificare la procedura di servizio del page-fault aggiungendo il rimpiazzamento della pagina;
- Sarà perciò necessario utilizzare un algoritmo di rimpiazzamento della pagina per selezionare il frame vittima `victim frame`;
- Da notare che se nessun frame è disponibile saranno necessari *DUE* trasferimenti di pagina (uno fuori e uno dentro). Questo incrementa il tempo di accesso effettivo;
- Si può fare utilizzo di un `bit di modifica (modify bit oppure dirty bit)`. Ogni pagina o frame viene associato ad uno di esso. Quando una pagina è selezionata per il rimpiazzamento ne si esamina il bit di modifica;
- Se il bit è impostato allora si sa che tale pagina è stata modificata da quando è stata letta in archiviazione secondaria. In tal caso la pagina deve essere scritta in archiviazione;
- Se il bit non è stato impostato, allora la pagina *NON* è stata modificata da quando è stata letta dalla memoria. Per cui non si dovrà scrivere la pagina di memoria nell'archiviazione, perchè è già lì;
- Questo schema permette di ridurre notevolmente il tempo richiesto per servire un page-fault, dato che dimezza il tempo di I/O *SE* la pagina non è stata modificata.

### Osservazioni
- Sarà necessario sviluppare algoritmi per la locazione dei frame e per il rimpiazzamento delle pagine. Si dovrà infatti decidere quanti frame allocare a ciascun processo e quando il rimpiazzamento di pagina è necessario;
- La progettazione di algoritmi appropriati è importante poichè le operazione di I/O nell'archiviazione secondaria sono costose. Anche miglioramenti minimi possono generare risultati migliori a livello di sistema;

#### FIFO Page Replacement
- Il più semplice degli algoritmi associa ciascuna pagina al tempo in cui essa è stata caricata in memoria. Quando una pagina deve essere rimpiazzata si sceglie la più vecchia (quella in `memoria da più tempo`). La pagina più vecchia si troverà in cima alla coda fifo, e quando una pagina viene caricata in memoria verrà inserita sul fondo di tale coda;
- Questo algoritmo è semplice da capire e progettare, ma le performance non sono sempre ottimali. Da una parte la pagina rimpiazzata potrebbe essere un modulo di inizializzazione usato molto tempo prima e mai più usato, dall'altra potrebbe contenere una variabile inizializzata in principio e costantemente in uso;
- Il rimpiazzamento di una pagina in uso tuttavia non affetterebbe nulla. Un page fault accadrebbe quasi subito e tale pagina verrebbe riacquisita. Il problema è che altre pagine dovranno venir rimpiazzate per consentire il reinserimento in memoria di quella attiva;
- Dunque una pessima scelta nel rimpiazzamento incrementa la frequenza dei page-fault rallentando l'esecuzione del processo, pur non causando una esecuzione errata.

#### Optimal Page Replacement
- Questo algoritmo rimpiazza la pagina che `non verrà utilizzata per il maggiore tempo`, inoltre garantisce la più bassa frequenza possibile di page-fault per un numero fisso di frame;
- Tuttavia è un algoritmo difficile da implementare, infatti viene spesso utilizzata negli studi di confronto.

#### LRU Page Replacement (Least Recently Used)
- Questo algoritmo rimpiazza la pagina che `non è stata utilizzata per il maggiore tempo`;
- Ogni pagina è associata al tempo dell'ultimo utilizzo, e quando una pagina deve essere rimpiazzata quella che non è stata utilizzata da più tempo verrà selezionata;
- Il problema sta nel modo in cui implementare tale algoritmo poichè necessiterebbe di un sostanziale supporto hardware. La difficoltà sta nel determinare un ordine per i frame definiti dal tempo dell'ultimo utilizzo;
- Una implementazione prevede l'utilizzo di un `contatore` o di un `clock logico`. Si associa ad ogni entrata della tabella delle pagine un campo `tempo di utilizzo` e si aggiunge alla CPU un contatore o un clock. Tale contatore verrà incrementato ad ogni referenza di memoria. Quando una referenza ad una pagina viene fatta il contenuto del registro del clock viene copiato al campo "tempo di utilizzo" nella tabella delle pagine di quella pagina. Così facendo si ha il tempo di ultima referenza di ogni pagina;
- Tale implementazione richiede una ricerca della tabella delle pagine per trovare la pagina meno usata di recente e una scrittura in memoria per aggiornare il campo. I tempi devono essere mantenuti quando le tabelle delle pagine vengono cambiate. Inoltre può verificarsi un overflow del clock;
- Un'altra implementazione utilizza lo `stack` dei numeri delle pagine. Quando una pagina viene referenziata viene rimossa dallo stack e reinserita in cima. Quindi la pagina più usata di recente è in cima, quella meno usata è sul fondo. Si ricorre ad una lista linkata doppia con un puntatore head e tail. Ogni aggiornamento è un po' più costoso ma non è necessaria la ricerca per il rimpiazzamento, il tail pointer punta al fondo dello stack.

#### LRU approssimato 
- In assenza di supporto hardware alcuni sistemi ricorrono al `bit di referenza (reference bit)`. Tale bit viene impostato dall'hardware ad ogni referenza di pagina ed è associato ad ogni entrata nella tabella delle pagine;
- Inizialmente tutti i bit sono impostati a zero dal sistema operativo, per poi passare ad 1 quando se la relativa pagina viene referenziata;
- Risulta quindi possibile conoscere quali pagine sono state usate e quali no, ma non l'ordine di utilizzo.

#### Counting-based Page Replacement (rimpiazzamento basato su conteggio)
- L'algoritmo `Usato meno volte (LFU o Least frequently used)` rimpiazza la pagina che conta meno referenze ad essa associate. Tuttavia una pagina potrebbe venire usata molto spesso all'inizio e mai più utilizzata in seguito, in tal caso non verrebbe rimpiazzata. Una soluzione è lo shift a destra di un bit ad intervalli regolari così che il conteggio dell'utilizzo medio decada esponenzialmente;
- L'algoritmo `Usato più volte (MFU o Most frequently used)` funziona esattamente all'opposto.

#### Page-Buffering 
- Alcuni sistemi mantengono una riserva di frame liberi. La scelta della vittima accade come prima, ma la pagina desiderata è letta in uno di questi frame. Questo metodo consente al processo di riavviarsi il prima possibile. Selezionata la vittima il suo frame viene aggiunto alla riserva.

### [prossimo](https://github.com/Gabri432/Sistemi_operativi/blob/master/Memoria_Virtuale/parte_3.md)