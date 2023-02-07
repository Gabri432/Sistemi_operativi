## Strutture di un sistema operativo
- I sistemi operativi moderni sono sistemi assai larghi e complessi, per cui si adotta un approccio comune che è quello di suddividere le funzionalità in componenti più piccoli, o moduli. Quindi ciascun modulo avrà delle specifiche funzioni e interfacce.

### Struttura monolitica (Monolithic Structure)
- Questa è la più semplice delle strutture, ma anche uno degli approcci più comuni;
- Il kernel è contenuto in un singolo file binario statico che viene avviato in un singolo indirizzo di spazio di memoria;
- Un esempio di tale struttura è l'originale sistema UNIX, costituito da due parti: il kernel e i programmi dell'utente;
- Similmente anche molti sistemi operativi basati sul kernel di Linux;
- Questo tipo di sistema è difficile da implementare ed estendere, ma risultano particolarmente efficienti e veloci. Nello specifico le comunicazioni all'interno del kernel sono rapide e l'interfaccia della chiamata di sistema è semplice.

### Struttura a layer (Layered Approach)
- L'idea di base è quella di suddividere un sistema in moduli più piccoli aventi funzionalità specifiche e limitate.
- Il vantaggio di un approccio modulare è che le modifiche che vengono apportate ad una componente non influenzano le altre, così da consentire una maggiore libertà di cambiamento;
- Nello specifico dell'approccio a layer si definisce un componente zero (Layer 0) che verrà associato all'hardware, mentre il componente più alto in numero (Layer N) verrà associato all'interfaccia utente;
- Ciascun layer consiste in un insieme di strutture dati e funzioni invocabili da altri layer di livello maggiore. Ossia, dato un layer M questo fornirà le sue funzionalità a tutti i layer M+1, M+2,... e potrà a sua volta chiamarne delle altre da layer inferiori.
- Grazie a questo approccio il debugging e la verificazione del sistema vengono semplificati. Se un errore viene trovato in un layer, tale errore riguarderà solo quel layer poichè quelli precedenti saranno gia stati debuggati a loro volta (partendo dal layer 1 che usa solo hardware e quindi si assume che sia corretto);
- Questo sistema è ampiamente usato nelle applicazioni web e nei computer networks, ma rara per sistemi operativi;
- Uno dei problemi sta nella definizione delle funzionalità di ciascun layer, inoltre si hanno scarse performance a causa del fatto che un programma dell'utente potrebbe dover attraversare diversi layer per ottenere uno specifico servizio del sistema operativo.


### Microkernel
- In questa struttura tutte le componenti non essenziali vengono rimosse dal kernel e implementate al livello di programmi utenti che risiedono in indirizzi di memoria separati, dando così origine ad un kernel più piccolo;
- In genere un microkernel contiene servizi minimali per la gestione dei processi e della memoria, e qualche servizio di comunicazione;
- La principale funzionalità di un microkernel è quella di fornire comunicazione tra il programma cliente e i vari servizi che sono avviati nello spazio utente, e tale comunicazione è fornita attraverso il passaggio di un messaggio;
- Un vantaggio dei microkernel è la facilità con cui si può estendere il sistema operativo, poichè ogni nuovo servizio è aggiunto allo spazio utente e quindi non necessita di modificare il kernel. Anche in caso di modifica in genere si tratta di una modifica piccola, essendo appunto un kernel più piccolo;
- Un altro vantaggio di ciò è il sistema operativo risultante è più facile da portare tra un design hardware ad un altro;
- Inoltre il microkernel fornisce più sicurezza e affidabilità, poichè molti servizi funzionano come processi dell'utente, quindi il fallimento di un servizio non influenza il resto del sistema operativo;
- Tuttavia l'incremento di funzionalità comporta un peggioramento delle performance. Due servizi a livello utente per poter comunicare hanno bisogno che i messaggi vengano copiati tra i vari servizi, i quali risiedono in indirizzi di memoria separati;
- In aggiunta un sistema operativo potrebbe dover cambiare da un processo all'altro per favorire lo scambio dei messaggi. 

### Moduli
- Uno dei metodi usati è il Caricamento dei Moduli del Kernel (Loadable Kernel Modules), dove il kernel ha un insieme di componenti fondamentali (cioè strettamente essenziali) e può collegarsi servizi addizionali per mezzo dei moduli, sia all'avvio del computer che a runtime (ad esecuzione del kernel);
- Collegare i servizi, anzichè aggiungerli direttamente al kernel, permette di evitare la ricompilazione dello stesso ad ogni cambiamento;
- Il risultato è più flessibile dell'approccio a layer perchè ogni modulo può chiamare ogni altro modulo, ed è più efficiente del microkernel perchè i moduli non necessitano del passaggio dei messaggi per la comunicazione;
- Tale approccio è usato in Linux, Windows, MacOs e Solaris.

### Sistemi Ibridi
- In generale questo è l'approccio più comune dai sistemi operativi, cioè l'utilizzo di una combinazione di differenti strutture per fronteggiare i diversi requisiti di performance, sicurezza e usabilità.

### Macchine Virtuali
- Con questo approccio si vuole creare un'astrazione dell'hardware in modo tale da dare ad un processo l'illusione di avere una macchina tutta per se.
- I vantaggi sono che ciascun processo gode di una protezione completa, permette di sviluppare e testare funzionalità in diversi sistemi operativi, permette inoltre di risolvere problemi di compatibilità tra sistema operativo e macchina;
- Dal punto di vista del sistema operativo originale queste macchine virtuali non sono altro che processi utente;
- Lo svantaggio è relativo alle performance, poichè ogni chiamata dal sistema della macchina virtuale affronta un viaggio più lungo, dallo stadio virtuale a quello fisico.


## Avviamento di un sistema operativo
- Il caricamento del Kernel è chiamato Booting del sistema;
- Il programma di bootstrap o boot loader localizza il kernel;
- Il kernel viene caricato in memoria e avviato;
- Il kernel inizializza l'hardware,
- Il file-system root viene montato (mounting).