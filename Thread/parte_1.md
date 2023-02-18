## Cosa sono?
- Il thread è l'unità base dell'utilizzo della CPU, è composto da un ID, il program counter, l'insieme dei registri e uno stack;
- Condivide con altri thread appartenenti allo stesso processo la sezione del codice, dei dati, e altre risorse come file aperti e segnali;

## Programmazione multithread
- Responsività, parti di una applicazione possono procedere anche se altre sono bloccate o eseguono operazioni lunghe;
- Condivisione di risorse, i thread condividono risorse associate allo stesso processo;
- Economia, la creazione ed il cambio di contesto dei thread è più economica poichè questi condividono le stesse risorse;
- Scalabilità, nelle architetture multiprocessori i thread possono procedere in parallelo.

## Difficoltà
- Bisogna identificare quali thread sono idonei alla parallelizzazione;
- Bisogna assicurarsi che tali thread effettuino una quantità di lavoro equa;
- Bisogna capire come suddividere i dati;
- Bisogna capire quali thread dipendono dagli stessi dati;
- Testare e debuggare programmi che procedono in concorrenza risulta più difficile.

## Parallelismi
- Dei dati, distribuzione dei dati su più core di computazione e eseguire la stessa operazione su più core;
- Dei task, distribuzione dei thread su più core.

## Modelli di Multithread 
- User thread, supporto dei thread a livello utente, o thread del kernel, supportati e gestiti dal sistema operativo.

### Many to One Model
- Il modello `molti ad uno` mappa diversi user thread ad uno kernel. La gestione dei thread è effettuata da una libreria nello spazio utente, così da essere efficiente;
- Un problema è che se un thread effettua una chiamata di blocco tutto il processo si arresta, inoltre molteplici thread non potrebbero procedere in concorrenza dato che solo uno per volta può accedere al kernel.

### One to One Model
- Il modello `uno ad uno` mappa ciascun user thread ad un kernel thread. Tale sistema permette la concorrenza e non causa arresto del processo in caso di chiamata di blocco da parte di un thread;
- Il problema è che la creazione di molti user thread comporta la creazione di molti kernel thread, il che può peggiorare le performance di un sistema.

### Many to Many Model
- Il modello `molti a molti` mappa diversi user thread ad un gruppo di kernel thread di numerosità minore o uguale;
- Non soffre di nessuno dei due problemi precedentemente menzionati. Un utente può creare tutti gli user thread che vuole. In caso di chiamata di blocco il sistema può programmare un altro thread per l'esecuzione.
- Risulta però difficile da implementare, e con l'aumento dei core il limite di kernel thread è un problema minore.

### Two level Model
- Il modell `a due livelli` funziona similmente al modello molti a molti, ma permette anche ad un singolo user thread di essere associato ad un singolo kernel thread.

## Thread implicito (Implicit threading)
- Il design di applicazioni aventi tantissimi thread è difficoltoso, così si decide di trasferire la creazione e gestione dei thread ai compilatori e librerie run-time piuttosto che agli sviluppatori;

### Thread pools
- Si creano un gruppo di thread all'inizio, i quali attenderanno per un compito. Quando un servizio effettua una richiesta questa viene inserita nel thread pool e servita immediatamente se un thread è disponibile, altrimenti viene messa in una coda d'attesa;
- Offre quindi i vantaggi di: essere più veloce della creazione di un nuovo thread, sono possibili diverse strategie per l'esecuzione di un thread (come l'esecuzione posticipata, o periodica), limita il numero di thread che esistono per volta.

### Fork-join
- Una libreria è responsabile per il numero di thread che sono creati e per l'assegnamento dei taska quest'ultimi;
- Funziona come una versione sicrona dei thread pool.


### [prosegui](https://github.com/Gabri432/Sistemi_operativi/blob/master/Thread/parte_2.md)