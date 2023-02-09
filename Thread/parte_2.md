## Problemi coi thread

### Fork() e exec()
- Cosa succede quando un thread effettua una fork()?
- Alcuni sistemi UNIX hanno deciso di avere due versioni di fork(), una che duplica tutti i thread e un'altra che duplica solo il thread che ha fatto la chiamata;
- Se exec() è chiamato subito dopo fork() non si duplicheranno tutti i thread, viceversa se non viene chiamato exec() allora vi è  duplicazione.

### Segnali
- Tutti i segnali sono generati dall'occorrenza di un evento particolare, vengono mandati ad un processo, e una volta ciò devono essere gestiti;
- Un segnale può essere gestito da un handler di default oppure definito dall'utente;
- Nel multithreading ci si deve chiedere dove mandare il segnale. Si può mandare il segnale al thread cui il segnale verte, oppure ad ogni thread del processo, o a certi thread del processo, o assegnare un thread specifico per ricevere tutti i segnali per il processo.

### Cancellazione del thread
- La cancellazione del thread implica la sua eliminazione prima del suo completamento. Tal thread è il `target thread`;
- La cancellazione avviene in modo `asincrono`, un thread elimina il target thread, oppure `differita` (deferred cancellation), il target periodicamente verifica se deve essere eliminato, così da poter terminare ordinatamente;
- Diventa problematica se vi sono risorse associate a thread cancellati oppure se un thread viene cancellato mentre sta aggiornando dati che condivide con altri thread;
- In modo asincrono alcune risorse potrebbero non venire reclamate dal sistema operativo. Con la differita si può avere una cancellazione sicura.

### Attivazioni dello scheduler
- La comunicazione tra kernel e libreria di thread (che può essere richiesta dai modelli Many-To-Many o Two-Level) permette di aggiustare dinamicamente il numero di thread per assicurare la migliore performance;
- Un processo di peso leggero `Lightweight process` viene usato come intermediario tra kernel e libreria. Per la libreria agisce come un processore virtuale dove si può schedulare un thread da avviare;
- Ogni LWP è attaccato ad un kernel thread. Se questo si blocca allora anche LWP si bloccherà, così come il thread a livello user attaccato a LWP;
- Uno schema usato per tale comunicazione è l'attivazione dello scheduler. Il kernel provvede a fornire una serie di processori virtuali (LWP) ad una applicazione. Il kernel avvisa l'applicazione di certi eventi, tramite una `upcall`. Questa può venire provocata da un thread applicativo che sta per bloccarsi. Quindi il kernel alloca un altro processore virtuale all'applicazione. Tale applicazione fa una upcall sul nuovo processore, che salva lo stato del thread bloccante, e rilascia il processore su qui il thread bloccante sta procedendo.
- Quindi l'upcall handler schedula un altro thread per girare sul nuovo processore.   
- Quando il thread bloccante ha finito di attendere un evento il kernel avvisa l'applicazione che esso è di nuovo disponibile.