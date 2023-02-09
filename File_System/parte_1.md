## Cosa sono?
- Un file è una collezione di informazioni definite dal suo creatore. Un file è mappato dal sistema operativo sui dispositivi fisici di memoria di massa;
- Un file può rappresentare programmi oppure dati. I dati possono essere numerici, alfabetici, alfa-numerici o binari;
- Un file è una sequenza di bit, byte, linee o record. Può essere un file eseguibile, di testo o sorgente.

### Attributi di un file
- `Nome`, in una forma leggibile da un essere umano;
- `Identificativo`, un tag univoco, spesso numerico, che identifica il file nel file system, in forma non leggibile per un umano;
- `Tipo`, informazione necessaria per sistemi che supportano diversi tipi di file;
- `Locazione`, un puntatore ad un dispositivo e ad una locazione del file nel dispositivo;
- `Dimensione`, l'attuale dimensione del file, e potenzialmente la sua dimensione massima;
- `Protezione`, informazione sul controllo dell'accesso, chi può leggervi, scriverci, eseguirlo, ecc...;
- `Data e identificativo utente`, informazione che può essere trattenuta per creazione, ultima modifica o uso, e la data per misure di protezione, sicurezza o monitoraggio.

### Operazioni su un file
- `Creazione di un file`, ricerca di uno spazio per la sua creazione e creazione di un ingresso ad esso nella cartella;
- `Apertura`, essenziale per ogni operazione di lettura, scrittura, riposizionamento e troncamento, rilascia un gestore del file se avviene con successo (file handle);
- `Scrittura`, il sistema tiene traccia di un puntatore di scrittura (write pointer) alla locazione nel file dove eseguire la prossima scrittura. Tale puntatore deve rimanere aggiornato;
- `Lettura del file`, il sistema tiene traccia di un puntatore di lettura (read pointer) alla locazione nel file dove eseguire la prossima lettura. Una volta effettuata la lettura il puntatore deve essere aggiornato. Talvolta si usa un puntatore alla posizione corrente nel file (current-file-positio pointer) poichè spesso lettura e scrittura sono richieste e cosi si usa un solo puntatore per entrambe, risparmiando tempo e spazio; 
- `Riposizionamento all'interno del file`, il puntatore alla posizione corrente viene riposizionato ad un certo valore;
- `Cancellazione di un file`, dopo aver cercato la directory del file si rilascia tutto lo spazio del file, cosicchè possa venire utilizzato per altri file;
- `Troncamento di un file`, cancellazione del contenuto del file ma preservandone gli attributi.

### Informazioni legate ad un file aperto
- Il sistema operativo organizza una tabella contenente informazioni riguardo tutti i file aperti cosi da evitare ripetute ricerche. Alla cancellazione del file si cancella anche il suo ingresso nella tabella;
- `Puntatore al file`, il sistema tiene traccia della posizione del puntatore alla posizione corrente;
- `Contatore del file aperto`, che traccia il numero di aperture e chiusure e arriva a zero all'ultima chiusura;
- `Locazione del file`, l'informazione necessaria per localizzare il file è tenuta in memoria così da non dover leggere dalla struttura della cartella ogni volta;
- `Diritti d'accesso`, l'informazione della modalità d'accesso del processo è contenuta nella tabella per-processo così che il sistema può consentire o negare successive richieste I/O.

### Metodi d'accesso

#### Accesso sequenziale
- Il metodo più semplice è l'accesso sequenziale, dove le informazioni nel file sono processate in ordine, un record dopo l'altro;
- La lettura è effettuata nella successiva porzione del file e fa avanzare in automatico il puntatore al file, che traccia la locazione I/O;
- La scrittura prosegue dalla fine del file e avanza dalla parte finale del nuovo materiale inserito;
- Si basa su un modello di file a nastro e lavora bene in dispositivi ad accesso sequenziale e ad accesso casuale.

#### Accesso diretto
- Il file in questo caso è composto da dei record logici di lunghezza fissa. La lettura e la scrittura nei record sono consentite rapidamente in qualunque ordine;
- Si basa su un modello di file a disco, dato che i dischi consentono l'accesso casuale ad ogni file del blocco;
- Tali tipi di file sono comodi per l'accesso immediato a grandi quantità di dati, infatti spesso utilizzati nei database.

### Altri metodi d'accesso

#### Metodo per indice
- L'indice contiene i puntatori a vari blocchi. Per cercare un record di un file si cerca prima l'indice e poi si usa un puntatore per accedere direttamente al file e cercare il record desiderato;
- Per file larghi l'indice può diventare troppo largo per essere tenuto in memoria. Una soluzione è la creazione di un indice per il file dell'indice. Il primo file indice contiene puntatori a file indici secondari, che punteranno ai dati effettivi. 