## Struttura della directory
- Nella progettazione di una directory si deve tener conto delle diverse operazioni su di essa effettuate;
- Queste operazioni sono: la creazione e la cancellazione del file, l'elencazione dei file in una directory, rinominazione di un file, e attraversamento del file system (necessario per effettuare il back up di tutti i file in caso di fallimento del sistema).

### Directory a livello singolo
- Tutti i file sono contenunti nella stessa directory, che è facile da supportare e comprendere;
- Vi sono però diverse limitazioni, legate soprattutto all'incremento del numero dei file;
- In particolare si deve garantire che tutti i file abbiano nomi univoci, ed è difficile che anche in un sistema con un solo utente sia possibile ricordarseli tutti.

### Directory a due livelli
- Ogni utente ha la propria directory dei file dello user (User File Directory, o UFD), ognuna quindi contenente un elenco dei file di un singolo utente;
- Quando un utente cerca un file lo si cerca nella sua UFD, per cui è sufficiente avere nomi univoci all'interno di essa. Più user possono quindi avere file con lo stesso nome;
- Creazione e cancellazione dei file di un utente avvengono dentro la stessa UFD;
- Anche questo sistema presenta svantaggi. Mentre permette di avere indipendenza completa tra utenti non consente invece la cooperazione tra più utenti. Cioè un utente non può accedere ai file di altri utenti;
- L'unica possibilità è quella che l'utente conosca il nome del file di un altro utente, quindi deve avere sia l'utente associato alla UFD dove risiede il file che il nome dello stesso file. Questi due dati identificheranno il path name del file desiderato.

### Directory a struttura d'albero
- Questa struttura consente all'utente di creare delle sotto-directory alle directory principali, dove poter organizzare i file nei vari modi più appropriati;
- Una directory quindi contiene sia file che altre directory. Alcune implementazioni fanno si che le stesse directory siano dei file particolari. Un bit d'ingresso alla directory identifica tale ingresso come file o (sub-)directory;
- I path possono essere assoluti (ed iniziare dalla radice), oppure relativi (a partire dalla directory in cui ci si trova)
- Con questa struttura un utente ha accesso sia ai propri file che a quelli di altri utenti.

### Directory a grafo aciclico
- Con questa struttura è possibile condividere file e directory con altri utenti. Per cui modifiche effettuate da un utente saranno visibili anche agli altri;
- Un modo per implementare tale struttura è tramite l'utilizzo di un link, cioè un puntatore ad un file o ad una subdirectory, e tale link può essere un implementato come un path assoluto o relativo. Un link è facilmente identificabile dal format nell'ingresso della directory;
- Un altro approccio è la duplicazione delle informazioni dei file in condivisione in entrambe le directory condivise. Dunque entrambi gli ingressi sono identici e indistinguibili, ma si possono avere problemi di inconsistenza dei file una volta modificatone uno;
- I grafi aciclici sono quindi più flessibili ma complessi. Ora un file può avere più nomi di path assoluti, quindi diversi nomi di file possono riferirsi al medesimo;
- Un altro problema riguarda quando effettuare la cancellazione dei file. In alcuni casi i link vengono lasciati e l'utente dovrà in autonomia capire che il file è stato cancellato;
- Un altro approccio prevede invece di preservare il file fino a quando tutti i riferimenti ad esso non sono cancellati, ma un file potrebbe avere tanti riferimenti.
- Alcuni sistemi semplicemente non consentono la condivisione.

### Directory a grafo generale
- In un grafo generale si fa utilizzo di un garbage collector per determinare quando l'ultimo riferimento ad un file è stato cancellato;
- La garbage collection attraversa tutti i file mappando tutti i possibili accessi. Dunque tutto il resto è segnato come spazio libero, ma tale procedimento è lento per un file system basato sul disco;
- La garbage collection è necessaria per evitare i cicli con l'aggiunta di nuovi link alla struttura del grafo aciclico.