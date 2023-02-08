## Creazione dei processi
- Un processo può creare processi figli, creando figli a loro volta, formando così un albero di processi;
- Durante la creazione si possono verificare due casi: il genitore si esegue in concorrenza con i figli, oppure il genitore attende che alcuni o tutti i suoi figli abbiano terminato;
- Il nuovo processo potrebbe essere un duplicato del genitore (stesso nome e dati), oppure avere un nuovo programma caricato al suo interno;
- La creazione del processo avviene con la chiamata di sistema `fork()`, tale processo consiste in una copia dello spazio d'indirizzo del processo originale, così da facilitare la comunicazione genitore-figlio;
- In seguito uno dei due processi fa la chiamata di sistema `exec()` con la quale si carica il nuovo programma al posto della memoria del processo;

## Terminazione dei processi