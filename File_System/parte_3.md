## Protezione
- Vi è la necessità di dover proteggere le informazioni sia da problemi fisici (danno hardware) che da problemi di accesso;
- Un file system può essere danneggiato da problemi di hardware, mancanza o salti di corrente, crash, polvere, temperature estreme e atti di vandalismo;
- I file possono essere cancellati accidentalmente, oppure si può verificare un bug del file-system.

### Tipi di accesso
- La protezione dei file è legata alla capacità di accedervi. Dunque i meccanismi di protezione limitano il tipo di accesso che si può effettuare ad un file. Si applica un controllo sui diversi tipi di operazione.
- `Lettura`, dal file;
- `Scrittura`, o sovrascrittura del file;
- `Esecuzione`, caricamento del file in memoria ed esecuzione;
- `Aggiunta`, di informazioni alla fine di un file
- `Cancellazione`, del file e liberazione dello spazio per riuso;
- `Elencazione`, del nome e degli attributi del file;
- `Cambio di attributo`, Cambiamento degli attributi del file.

### Meccanismi di protezione

#### Controllo dell'accesso
- L'accesso dipende dall'identità dello user. Ogni file e directory è associata ad una lista del controllo degli accessi (`Access Control List`, ACL), la quale specifica il nome degli utenti e i tipi d'accesso concessi a ciascuno di essi.
- Quando un utente fa una richiesta per un file il sistema operativo verifica che l'utente sia nella lista di quelli aventi diritto ad accedervi. Se non lo è gli viene negato l'accesso;
- Questo metodo consente di abilitare metodologie più complesse, ma ha un problema relativo alla lunghezza della lista;
- La costruzione di questa lista può essere difficile e lunga se soprattutto non si conosce in anticipo la lista degli utenti nel sistema. La gestione dello spazio è più complessa dovendo permette ingressi di directory di dimensione variabile;

#### Versione condensata
- Una versione più semplice è quella che classifica solo tre tipi di utenti;
- `Proprietario` (Owner), l'utente che ha creato il file;
- `Gruppo`, un insieme di utenti che condividono il file e necessitano di simili accessi;
- `Altro` (Other), altri utenti nel sistema;
- Si cerca di combinare questa versione con le ACL.


### Altri meccanismi di protezione
- Un approccio è di associare ciascun file con una password, ma l'utente avrebbe bisogno di ricordarsi un numero di password potenzialmente alto. Inoltre se si utilizza una password per tutti i file allora una volta scoperta tutti i file sono accessibili;
- In alcuni sistemi lo stesso meccanismo viene adottato a livello di directory, ma rimane un problema di gestione delle password;
