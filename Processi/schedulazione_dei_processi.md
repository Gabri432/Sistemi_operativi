## Schedulazione dei processi
- Si fa utilizzo dello schedulatore di processi (process scheduler) che si occupa di trovare un processo disponibile all'esecuzione e inserirlo in un core;
- Ci sono processi con limite di I/O (I/O bound process) e di CPU (CPU bound process).

### Code di schedulazione
- Quando un processo arriva nel sistema viene collocato nella coda di processi pronti (ready queue), altrimenti se sta attendendo per un evento che accada allora viene posto nella coda di attesa (wait queue);
- Durante l'esecuzione di un processo questo può avere una richesta di I/O ed essere posto nella coda d'attesa, creare processi figli ed attendere nella coda d'attesa la terminazione di quest'ultimi, venire rimosso forzatamente (magari per via di un interrupt o se oltre il limite di tempo) e ricollocato alla coda dei processi pronti;
- La CPU scheduler deve allocare un core ad uno dei processi in ready queue;
- Quando accade un interrupt si fa ricorso al salvataggio dello stato del core della CPU, per poi la ripresa delle operazioni. Questo processo è chiamato Cambio di Contesto (Context Switch).