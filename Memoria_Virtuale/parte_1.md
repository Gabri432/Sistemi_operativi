## Che cos'è?
- La memoria virtuale è una tecnica con la quale diventa possibile eseguire processi più grandi della memoria disponibile;
- Dunque i programmi possono essere più grandi della memoria fisica, inoltre fornisce una separazione in memoria logica e fisica. In questo modo agli sviluppatori viene offerta una memoria molto più larga di quella effettiva;
- Questa tecnica consente ai processi di condividere file e librerie, e di implementare la memoria condivisa;
- Fornisce un efficiente modo di creare processi;
- Non è tuttavia facile da implementare, in più c'è una penalizzazione in termini di performance se tale tecnica è usata senza misura. 

### Benefici dei programmi parzialmente in memoria
- I programmi non sono più limitati dalle dimensioni della memoria;
- Dato che ciascun programma occupa meno spazio, più programmi possono venire eseguiti, così da aumentare l'utilizzo della CPU e il throughput senza incremento dei tempi di risposta;
- Vi saranno meno operazioni di I/O per il caricamento o swapping di porzioni di programmi in memoria, risultando in programmi che si eseguono più velocemente.

### Altri benefici della memoria virtuale
- Le librerie di sistema possono venire condivise da molteplici processi grazie alla mappatura dell'oggetto condiviso nell'indirizzo di memoria virtuale;
- I processi possono condividere la memoria. Un processo può quindi creare una regione di memoria condivisibile con altri processi;
- Le pagine possono essere condivise durante la creazione dei processi con la chiamata fork(), quindi velocizzando tale creazione.

### Paginazione su richiesta (Demand Paging)
- Con questa tecnica le pagine vengono caricate solo quando vengono richieste in fase di esecuzione del programma. Le pagine che non vengono mai accedute non vengono mai caricate in memoria fisica. Perciò la memoria viene usata più efficientemente;
- Per distinguere le pagine che saranno caricate in memoria dalle altre si fa utilizzo del `bit di validità (valid-invalid bit)`;
- Quando il bit è su "valido" la pagina associata è legale (cioè nello spazio di indirizzamento logico) e in memoria. Viceversa se è su "invalido" la pagina o è invalida (quindi non è nello spazio di indirizzamento logico) oppure è valido ma non è in memoria;

#### Page fault
- Se un processo cercasse di accedere ad una pagina invalida causerebbe un  `page fault`. Cioè l'hardware avrà inviato un segnale di `trap` al sistema operativo, e questo trap è il risultato del fallimento del sistema operativo nel fornire la pagina richiesta in memoria;
- Per gestire tale situazione si dovrà cercare una tabella interna per il processo coinvolto per determinare se la referenza fosse valida o invalida;
- Se invalida allora il processo viene terminato. Viceversa se valida allora ma la pagina non è ancora stata inserita, verrà inserita;
- Si trova un frame libero e si schedula una operazione in archiviazione secondaria per leggere la pagina desiderata nel frame nuovamente allocato;
- Quando la lettura è completata si modifica la tabella interna tenuta con il processo e la tabella della pagina per indicare che la pagina è ora in memoria;
- Si riavvia l'istruzione precedentemente interrotta dalla trap, così che ora il processo possa accedere alla pagina.

### Supporto hardware
- `Tabella della pagina`, che è in grado di segnare un'entrata come invalida attraverso il bit di validità;
- `Memoria secondaria`, che contiene le pagine non presenti nella memoria principale. Viene conosciuto anche come `swap device` e la sezione di archiviazione usata per tale scopo è detta `swap space`.

### Lista dei frame liberi (Free-frame list)
- Nella risoluzione di un page fault molti sistemi operativi mantengono una `lista dei frame liberi`, cioè una riserva di frame liberi per soddisfare la richiesta di pagine dalla memoria secondaria a quella principale;
- Per allocare i frame liberi si usa la tecnica del `zero-fill-on-deman` (riempimento zero su richiesta). I frame prima della loro allocazione vengono ripuliti del loro contenuto. All' avviamento del sistema tutta la memoria disponibile viene piazzata nella lista dei frame liberi;
- Alla richiesta di questi frame tale lista si accorcia. Quindi può esaurirsi completamente o comunque scendere sotto una certa dimensione, e a tal punto va ripopolata.

### Performance della paginazione su richiesta
- Per calcolare il tempo di accesso ad una pagina su richiesta si tiene conto di quanto tempo è necessario per servire un page fault. Tale page fault causa una serie di eventi;
- Il segnale di trap arriva al sistema operativo, si salvano i registri e lo stato del processo, si determina che l'interrupt era un page fault, si verifica la legalità della referenza di pagina e si determina la locazione della pagina nella archiviazione secondaria;
- Si avvia un'operazione di lettura dall'archiviazione al frame libero. Mentre si attende che tale richiesta venga servita si alloca il core della CPU ad altri processi. Si riceve un interrupt dal sottosistema I/O di archiviazione, si salvano i registri e gli stati degli altri processi (ammesso che il core sia stato allocato), si determina che l'interrupt provenisse dall'archiviazione secondaria;
- Si corregge la tabella della pagina e altre tabelle per mostrare che ora la pagina richiesta è in memoria, si attende che il core della CPU venga nuovamente allocato al processo interrotto, si ripristinano i registri e lo stato del processo, una nuova tabella della pagina e l'istruzione interrotta;
- Pur non essendo tutti passaggi necessari vi sono almeno tre componenti principali: servire l'interrupt del page fault, lettura nella pagina, riavvio del processo.

#### Problemi
- Il tempo di accesso effettivo è direttamente proporzionale alla frequenza dei page fault, quindi tale frequenza deve essere la più bassa possibile. Un altro aspetto è la gestione e utilizzo dello spazio di swap;
- Operare I/O allo spazio di swap è più veloce che al file system. Una opzione per il sistema di guadagnare throughput è di copiare l'intero file nello spazio di swap all'inizio del processo ed eseguire la paginazione su richiesta dallo swap space. Tale copiatura è ovviamente svantaggiosa;
- Una seconda opzione è quella di eseguire il demang-paging dal file system inizialmente ma poi di scrivere le pagine nello spazio di swap nel momento in cui vengono rimpiazzate. Tale approccio si assicura che solo le pagine necessarie siano lette dal file system ma tutta la paginazione successiva avviene dallo spazio di swap;