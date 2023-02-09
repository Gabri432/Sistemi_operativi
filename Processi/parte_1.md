## Cosa sono i processi?
- Un processo è un programma in esecuzione ed è l'unità di lavoro in un computer moderno;
- Un processo è un'entità attiva, mentre il programma è un'entità passiva;
- Un programma diventa un processo quando un file eseguibilie viene caricato in memoria;
- Due processi, anche se associati allo stesso programma, sono due sequenze separate;
- Un processo può generare altri processi.

### Layout della memoria di un processo
- `Sezione di testo`, contenente il codice eseguibile;
- `Sezione dei dati`, contenente le variabili globali;
- `Sezione heap`, cioè la memoria allocata dinamicamente durante l'esecuzione di un programma;
- `Sezione stack`, archivio dei dati temporanei all'atto d'invocazione delle funzioni (parametri di funzione, indirizzi di ritorno e variabili locali).


## Stati di un processo
- L'esecuzione di un processo può modificarne lo stato, che è in parte definito dall'attività corrente dello stesso;
- Può essere `Nuovo`, cioè un processo è in fase di creazione;
- `In esecuzione`, cioè quando le istruzioni vengono eseguite;
- `In attesa`, cioè attende l'avvenire di un evento (ricezione di un segnale o un'operazione di I/O per esempio);
- `Pronto`, è in attesa dell'assegnamento all'esecuzione;
- `Terminato`, ha concluso la sua esecuzione.

## Blocco di controllo del processo (Process Control Block)
- Ciascun processo è rappresentato nel sistema operativo come Blocco di controllo del processo, il quale contiene informazioni sul processo stesso;
- `Stato del processo`;
- `Program counter`, il quale indica l'indirizzo della prossima istruzione da eseguire per il processo;
- `Registri della CPU`, che devono essere salvati in caso di occorrenza di un interrupt per consentire la corretta ripresa;
- `Informazioni sulla schedulazione della CPU`, che include la priorità di un processo, puntatori alle code di schedulazione e altri parametri di schedulazione;
- `Informazione sulla gestione della memoria`;
- `Informazioni di conteggio` (accounting = contabilità), che include la quantità di CPU utilizzata, e altri numeri;
- `Informazioni sullo stato di I/O`, che include la lista di dispositivi I/O allocati al processo, la lista di file aperti, ecc...