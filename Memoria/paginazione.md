## Paginazione
- La paginazione consente allo spazio d'indirizzamento fisico di un processo di non essere contiguo. Questo procedimento evita la frammentazione esterna e la necessità di compattazione;
- Serve una cooperazione tra sistema operativo e hardware.

### Metodo di base
- La memoria fisica viene suddivisa in blocchi di grandezza fissa detti `frame` e la memoria logica è divisa in blocchi della stessa grandezza detti `pagine`;
- All'esecuzione di un processo le sue pagine sono caricate nei frame di memoria disponibili dalla loro sorgente, dal file system o l'archivio di supporto;
- Il file system o l'archivio di supporto viene diviso in blocchi delle stesse dimensioni dei frame. Così facendo lo spazio d'indirizzamento logico è completamente separato da quello fisico;
- Ogni indirizzo della CPU è separato in un `numero di pagina` e in un `offset di pagina`;
- Il numero di pagina viene usato come indice della tabella delle pagine per processo. Tale tabella contiene l'indirizzo di base di ciascun frame nella memoria fisica e l'offset è la locazione nel frame che viene referenziata;
- **L'indirizzo di base del frame è combinato con l'offset della pagina per definire l'indirizzo della memoria fisica**;
- Per tradurre l'indirizzo logico generato dalla CPU in uno fisico la MMU deve: estrarre il numero di pagina e usarlo come indice della tabella delle pagine, estrarre il corrispondente numero di frame dalla tabella, rimpiazzare il numero di pagina nell'indirizzo logico con il numero di frame;
- Il numero di frame e l'offset comprendono ora l'indirizzo fisico.

### Osservazioni
- L'impaginazione è una forma di rilocazione dinamica. Ogni indirizzo logico è legato a qualche indirizzo fisico dall'hardware di impaginazione;
- Pur non soffrendo di segmentazione esterna vi è possibile segmentazione interna. Se i requisiti di memoria di un processo non coincidono con i limiti della pagina l'ultimo frame allocato potrebbe non essere pieno.

### Supporto hardware
- L'implementazione hardware della tabella delle pagine può essere fatta in diversi modi;
- Il caso più semplice prevede di utilizzare un insieme di registri dedicati ad alta velocità, cosicchè la traduzione pagina-indirizzo risulti efficiente. Ma vi è un incremento del tempo di cambio del contesto, inoltre è soddisfacente per tabelle piccole.
- Per tabelle più grandi si fa ricorso al registro di base della tabella delle pagine, che punta alla tabella. Cambiare tabelle richiede di cambiare solo questo registro, riducendo il tempo di cambio del contesto. La tabella è tenuta in memoria principale.

### Protezione
- L'utilizzo di bit di protezione associati a ciascun frame consente la protezione della memoria in un ambiente impaginato;
- Un bit può definire le operazioni consentite in una pagina (solo lettura, solo scrittura, ambedue);
- Si viene ad aggiungere un bit di `validità-invalidità`. Quando è valido la pagina associata è nello spazio di indirizzamento logico del processo e quindi è una pagina legale. Altrimenti non si trova in tale spazio ed è illegale.