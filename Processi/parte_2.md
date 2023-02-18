## Creazione dei processi
- Un processo può creare processi figli, creando figli a loro volta, formando così un albero di processi;
- Durante la creazione si possono verificare due casi: il genitore si esegue in concorrenza con i figli, oppure il genitore attende che alcuni o tutti i suoi figli abbiano terminato;
- Il nuovo processo potrebbe essere un duplicato del genitore (stesso nome e dati), oppure avere un nuovo programma caricato al suo interno;
- La creazione del processo avviene con la chiamata di sistema `fork()`, tale processo consiste in una copia dello spazio d'indirizzo del processo originale, così da facilitare la comunicazione genitore-figlio;
- In seguito uno dei due processi fa la chiamata di sistema `exec()` con la quale si carica il nuovo programma al posto della memoria del processo;
- Il genitore potrebbe quindi creare altri figli oppure fare una chiamata di sistema `wait()` ed attendere la terminazione dei suoi figli;
- Se il figlio non effettua la chiamata `exec()` allora genitore e figlio proseguirebbero concorrentemente come due processi aventi stesso codice di istruzioni;

## Terminazione dei processi
- Un processo si conclude quando la sua ultima operazione viene eseguita e chiede al sistema operativo la sua cancellazione con la chiamata di sistema `exit()`;
- Tutte le risorse del processo vengono deallocati (come i file aperti, memoria virtuale e fisica, ...) e reclamati dal sistema operativo;
- Il genitore può causare la terminazione del figlio per diversi motivi: il figlio potrebbe aver superato l'utilizzo massimo delle sue risorse, il compito assegnato al figlio non è più necessario, il genitore sta terminando ed il sistema operativo non lascia processi orfani;
- La terminazione a cascata (Cascading termination) consiste nella eliminazione di tutti i processi figli alla conclusione del genitore;
- Se un processo è concluso ma il suo genitore non ha chiamato wait() questo processo diventa `zombie`, cioè i processi figli sono orfani.

### [Vai a comunicazione tra processi](https://github.com/Gabri432/Sistemi_operativi/blob/master/Processi/comunicazione_tra_processi.md)