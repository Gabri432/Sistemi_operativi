## Interfacce d'applicazione
- L'utilizzo di un'interfaccia consente di standardizzare un insieme di funzioni per ogni diverso dispositivo;
- Le differenze sono incapsulate nei moduli del kernel chiamati `device driver`, i quali internamente sono specifici del dispositivo ma esternamente forniscono una interfaccia standard;
- L'obiettivo del device driver è di nascondere le differenze tra i controller dei dispositivi dal sottosistema I/O del kernel. L'indipendenza del sottosistema dalle specifiche hardware semplifica il lavoro degli sviluppatori di sistemi operativi;
- Essi possono o sviluppare dispositivi compatibili con l'interfaccia del controller esistente oppure scrivere loro stessi i device driver per interfacciarsi all'hardware. Tuttavia ciascun sistema operativo ha i propri standard di interfacce dei device driver; 

### Differenze nascoste dal sistema operativo
- `Trasferimento di dati per via di blocchi o flusso di caratteri`, quindi o blocchi di byte o un byte alla volta;
- `Accesso sequenziale o casuale (random)`;
- `Accesso sincrono o asincrono`;
- `Dispositivo condivisibile o dedicato`, quindi supporta o meno un uso concorrente da più processi o thread;
- `Velocità di operazione`, da pochi byte a diversi gigabyte;
- `Operazioni eseguibili`, lettura o scrittura, opppure ambedue.

### Interfaccia per dispositivi a blocco e a caratteri
- Tale interfaccia racchiude tuti gli aspetti necessari per l'accesso i drive di disco e altri dispositivi orientati ai blocco;
- L'accesso a dispositivi a blocco può essere effettuato come un accesso ad un array linerare di blocchi, detto anche `accesso grezzo I/O`;
- L'accesso ai file mappati su memoria può essere stratificato sopra i device-driver a blocco. Tale interfaccia fornisce accesso al disco tramite un array di byte in memoria;
- Le tastiere, le stampanti e le casse audio sono esempi di dispositivi che sono acceduti tramite interfaccia di tipo flusso di caratteri.

### Clock e timer
- Vengono usati per segnare il tempo (orario) attuale, il tempo trascorso e il timer per impostare un'operazione in un dato tempo;
- Queste ultime due operazioni sono implementante a livello hardware tramite un `timer ad intervallo programmabile (programmable interval timer)`;

### I/O bloccante e non
- Quando un'applicazione invia un chiamata di blocco al sistema l'esecuzione del thread chiamante viene sospesa e tale thread viene spostato nella coda di attesa. Infine verrà ripristinato alla terminazione della chiamata;
- Alcuni processi utente necessitano di chiamate non bloccanti. Ad esempio l'interfaccia utente che riceve l'input dal mouse e dalla tastiera e mostra i dati sullo schermo;

### [prossimo](https://github.com/Gabri432/Sistemi_operativi/blob/master/Input_output/parte_3.md)