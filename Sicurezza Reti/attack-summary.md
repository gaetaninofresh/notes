## 1. Race Condition Vulnerability

* **Architettura Bersaglio:** Sistemi operativi che eseguono programmi con privilegi elevati (es. programmi Set-UID di proprietà di root).
* **Elementi Coinvolti:** Processi paralleli , directory con bit "sticky" come `/tmp` , file protetti come `/etc/passwd` , e chiamate di sistema C (`access()`, `fopen()`, `symlink()`, `unlink()`, `renameat2()`).

### Processo di Exploiting (TOCTTOU)
Questo attacco sfrutta la finestra temporale tra il momento in cui un programma controlla le autorizzazioni (Time of Check) e il momento in cui utilizza la risorsa (Time of Use).
1. **Identificazione:** L'attaccante individua un programma Set-UID che verifica i permessi di un file (tramite `access()`) prima di aprirlo (tramite `fopen()`).
2. **Esecuzione Parallela:** L'attaccante esegue in parallelo un programma per "gareggiare" (race) contro il programma privilegiato.
3. **Scambio del File:** Durante la finestra temporale tra `access` e `fopen`, l'attaccante usa un collegamento simbolico per reindirizzare il file target (es. `/tmp/XYZ`) verso un file di sistema protetto come `/etc/passwd`.
4. **Miglioramento dell'Attacco:** Per evitare vulnerabilità di race condition nello stesso script di attacco, si utilizza la chiamata di sistema `renameat2()` per scambiare due collegamenti simbolici in modo atomico.
5. **Risultato:** Il programma vulnerabile aggiunge l'input dell'utente (ad esempio un nuovo account con privilegi di root) al file delle password.


## 2. Format String Attack

* **Architettura Bersaglio:** Programmi C compilati come binari a 32-bit o 64-bit.
* **Elementi Coinvolti:** Funzioni di formattazione come `printf()`, `sprintf()`, e `fprintf()` , layout della memoria stack , stringhe di formattazione (es. `%x`, `%n`, `%hn`) , indirizzi di ritorno e shellcode.



### Processo di Exploiting
L'attacco avviene quando l'input dell'utente viene passato a una funzione di formattazione senza essere sanificato.
1. **Lettura della Memoria (Memory Leak):** L'attaccante utilizza gli specificatori di formato `%x` per costringere il programma a stampare i dati residenti nello stack.
2. **Modifica della Memoria:** Sfruttando gli specificatori `%n` o `%hn`, l'attaccante può scrivere un valore in uno spazio di memoria a 4 byte o 2 byte. Il valore scritto corrisponde al numero totale di caratteri stampati fino a quel momento.
3. **Esecuzione di Codice Maligno:** L'obiettivo è modificare il campo dell'indirizzo di ritorno di una funzione. Quando la funzione termina, salta all'indirizzo modificato, eseguendo il codice malevolo (shellcode) iniettato dall'attaccante.


## 3. Meltdown Attack

* **Architettura Bersaglio:** Processori moderni, inclusi quelli di Intel e ARM, e meccanismi di isolamento della memoria del kernel.
* **Elementi Coinvolti:** Cache della CPU , Esecuzione fuori ordine (Out-of-order execution) , gestione delle eccezioni (`SIGSEGV`) in C , e la tecnica FLUSH+RELOAD.



### Processo di Exploiting
L'attacco Meltdown elude l'isolamento hardware per leggere dati segreti all'interno della memoria del kernel.
1. **Inizializzazione (FLUSH):** L'attaccante espelle un "probing array" dalla memoria cache della CPU.
2. **Accesso e CPU Race:** Il programma a livello utente tenta di accedere a un dato protetto del kernel. In parallelo al controllo dei permessi (che richiederà tempo), la CPU avvia speculativamente l'esecuzione delle istruzioni successive (Out-of-order execution).
3. **Modifica della Cache:** Durante l'esecuzione fuori ordine, la CPU carica il dato segreto in un registro e lo memorizza nella cache. Il dato segreto viene usato come indice per accedere al "probing array".
4. **Gestione dell'Eccezione:** Il controllo dei permessi fallisce e la CPU scarta i risultati dell'esecuzione, sollevando un'eccezione `SIGSEGV`. L'attaccante utilizza un signal handler per evitare il crash del programma.
5. **Recupero via Side-Channel (RELOAD):** Poiché l'elemento dell'array indicizzato dal dato segreto è rimasto nella cache, l'attaccante misura il tempo di ricaricamento dell'array. L'elemento con il tempo di accesso più veloce rivela il valore del segreto.



## 4. Spectre Attack

* **Architettura Bersaglio:** Processori moderni (Intel, AMD, ARM). Colpisce l'isolamento hardware inter-processo e i meccanismi software intra-processo (es. sandbox dei browser).
* **Elementi Coinvolti:** Predizione dei salti (Branch Prediction), Esecuzione Speculativa , Cache della CPU e attacchi Side Channel (FLUSH+RELOAD).



### Processo di Exploiting
Spectre sfrutta i meccanismi di previsione della CPU per eludere i controlli delle condizioni logiche (es. vincoli di un array).
1. **Addestramento del Predittore (Training):** L'attaccante fornisce ripetutamente valori validi a una funzione vittima, addestrando la CPU a prendere sempre il ramo "vero" di un'istruzione condizionale (es. `if (x < size)`).
2. **Svuotamento (FLUSH):** L'attaccante rimuove la variabile di condizione (`size`) e l'array di analisi dalla cache.
3. **Esecuzione Speculativa:** L'attaccante passa un input fuori limite (`x` maggiore di `size`) che punta a un indirizzo segreto. Poiché `size` deve essere recuperato dalla memoria principale, la CPU procede con l'esecuzione speculativa assumendo (in base all'addestramento) che la condizione sia vera.
4. **Effetto sulla Cache:** Il valore segreto viene letto e utilizzato per indicizzare un elemento dell'array.
5. **Rilevamento:** Quando la CPU si rende conto dell'errore, scarta i risultati, ma l'impatto sulla cache rimane. Usando FLUSH+RELOAD, l'attaccante misura i tempi di accesso per capire quale cella di memoria è in cache, estraendo così il segreto.


## 5. Local DNS Attack

* **Architettura Bersaglio:** Ambiente di rete locale (LAN) in cui vittima, attaccante e Server DNS Locale condividono la rete.
* **Elementi Coinvolti:** Processo di risoluzione DNS, Cache del Server DNS , Sniffing di pacchetti e sezioni DNS (Answer, Authority, Additional).



### Processo di Exploiting
L'obiettivo è avvelenare la cache del Server DNS locale per reindirizzare le richieste dell'utente a indirizzi IP malevoli.
1. **Sniffing:** Poiché si trova sulla stessa LAN, l'attaccante intercetta i messaggi di richiesta DNS generati dal bersaglio.
2. **Spoofing e Poisoning:** L'attaccante genera immediatamente una falsa risposta DNS prima che arrivi quella legittima da Internet. Il server DNS locale archivia la risposta falsa nella propria cache (DNS cache poisoning).
3. **Espansione dell'Attacco:** Modificando la sezione "Authority" del pacchetto falsificato, l'attaccante può dichiarare il proprio nameserver come autorità per l'intero dominio, potendo così intercettare le query future per qualsiasi host di quel dominio.


## 6. Remote DNS Attack (Kaminsky)

* **Architettura Bersaglio:** Server DNS locali interrogati da un attaccante posizionato su una rete remota (esterna), impedendo lo sniffing dei pacchetti.
* **Elementi Coinvolti:** Server DNS vittima, Name Server dell'attaccante, ID di transazione DNS (a 16 bit) ed effetto Cache.



### Processo di Exploiting
Poiché l'attaccante non può intercettare l'ID di transazione del pacchetto DNS, deve inondare il server di risposte per "indovinare" l'ID, eludendo i blocchi della cache (Attacco Kaminsky).
1. **Generazione della Query:** L'attaccante interroga il server DNS vittima per un host inesistente generato casualmente (es. `twysw.example.com`).
2. **Forzatura della Risoluzione:** Poiché il nome non è in cache, il server vittima inoltra la query ai name server del dominio.
3. **Flooding:** Mentre il server attende, l'attaccante lo inonda con una pioggia di risposte contraffatte, ognuna con un ID di transazione diverso.
4. **Elusione della Cache:** Se l'attaccante fallisce, la query successiva userà un nome host diverso, forzando una nuova risoluzione senza dover attendere la scadenza della cache (aggirando l'effetto cache).
5. **Avvelenamento:** Se una risposta contraffatta indovina l'ID e arriva per prima, inserisce il nameserver dell'attaccante nella sezione Authority, compromettendo la risoluzione dell'intero dominio.


## 7. DNS Rebinding Attack

* **Architettura Bersaglio:** Dispositivi IoT vulnerabili posizionati dietro un firewall protettivo, gestiti tramite una rete interna.
* **Elementi Coinvolti:** Server DNS controllato dall'attaccante, Same-Origin Policy (SOP) dei browser, JavaScript e Time-To-Live (TTL) del DNS.



### Processo di Exploiting
Questo attacco mira a far eseguire codice dall'interno della rete protetta per raggirare le restrizioni della Same-Origin Policy (SOP).
1. **Navigazione:** L'utente all'interno della rete protetta visita il sito web dell'attaccante (`www.attacker32.com`), che esegue un codice JavaScript sul suo browser.
2. **Rebinding DNS:** Prima che l'utente interagisca (o automaticamente dopo un timer), l'attaccante riconfigura il proprio server DNS (modificando il file di zona), scambiando l'indirizzo IP del suo server web con l'indirizzo IP privato del dispositivo IoT vittima. L'uso di un TTL molto basso permette la rapida scadenza della cache.
3. **Elusione della SOP:** Il codice JavaScript richiede di interagire con `www.attacker32.com`. La SOP si basa sul nome host (che è rimasto invariato) e non sull'indirizzo IP. Il browser risolve di nuovo il dominio, ottiene l'indirizzo IP interno del dispositivo IoT e invia la richiesta HTTP, scavalcando il firewall dall'interno.
