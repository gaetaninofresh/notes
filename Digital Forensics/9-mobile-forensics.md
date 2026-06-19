# Mobile Forensics: Acquisizione e Analisi

La mobile forensics si struttura su diversi livelli di acquisizione e si scontra costantemente con i meccanismi di crittografia dei sistemi operativi.

## Livelli di Acquisizione
Esistono cinque livelli principali di acquisizione, classificati in base al grado crescente di completezza dei dati e tecnicità richiesta.
* **Esame Manuale**: L'esaminatore scorre fisicamente il dispositivo e documenta i contenuti tramite fotografie o video.
* **Acquisizione Logica**: Estrazione del contenuto di oggetti logici esistenti e non cancellati (es. foto, contatti, SMS).
* **Acquisizione File System (Logica Avanzata)**: Estrazione parziale di directory e file utente, tipicamente tramite backup e protocolli come AFC per iOS o ADB/MTP per Android.
* **Full File System (FFS)**: Estrazione completa del file system, inclusi dati residui e non cancellati, spesso ottenuta tramite exploit costosi.
* **Acquisizione Fisica**: Ottenimento della memoria bit-a-bit tramite metodi hardware avanzati (JTAG, ISP, chip-off) che include tutte le informazioni, anche quelle cancellate.

## Crittografia e Protezione Dati

### Android: FDE vs FBE
L'evoluzione della crittografia su Android ha impatti diretti sulle indagini forensi.

* **Full Disk Encryption (FDE)**: Presente da Android 4.4 a 12, codifica tutti i dati utente assieme utilizzando un'unica chiave richiesta all'accensione. Comporta un avvio più lento del dispositivo.
* **File-Based Encryption (FBE)**: Standard per i dispositivi Android 10+ e successivi, permette a file diversi di usare chiavi di crittografia indipendenti posizionate nel Trusted Execution Environment (TEE). Supporta il Direct Boot per avvii rapidi.
* **Credential Encrypted (CE)**: Dati sensibili delle app accessibili solo dopo lo sblocco da parte dell'utente.
* **Device Encrypted (DE)**: Dati di sistema essenziali disponibili sempre, anche prima dello sblocco.

> **NB.** Con l'adozione dell'FBE su Android, il recupero dei dati cancellati non è più possibile perché ogni file possiede una propria chiave crittografica che viene distrutta al momento dell'eliminazione del file.

### iOS: Classi di Protezione
I dati su iOS sono protetti secondo diverse classi crittografiche.

* **Class A (Complete Protection)**: La chiave, derivata dal passcode e dall'UID, viene cancellata 10 secondi dopo il blocco del dispositivo, rendendo i dati inaccessibili.
* **Class B (Protected Unless Open)**: Usata per file che devono essere scritti a dispositivo bloccato; la chiave in memoria viene mantenuta finché il file non è chiuso.
* **Class C (Protected Until First User Auth)**: Classe predefinita per le app di terze parti; la chiave decrittografata resta in memoria anche quando il dispositivo è bloccato.
* **Class D (No Protection)**: Il file risulta protetto unicamente dall'UID utente.

## Strumenti Forensi e Analisi
Le acquisizioni avvengono tramite soluzioni commerciali e gratuite, a cui fa seguito l'analisi degli artefatti estratti.
## L'Exploit Checkm8

Individuato dal ricercatore axiomX, colpisce fisicamente l'hardware dei dispositivi Apple che montano chip da iPhone 4S fino a iPhone X (inclusi diversi iPad).
* **Impossibilità di Patch:** Poiché la vulnerabilità risiede a livello fisico nel chip (BootROM), non può essere corretta da Apple tramite semplici aggiornamenti software.
* **Potenziale Forense:** Permette di superare le restrizioni logiche del sistema operativo, garantendo agli analisti un'estrazione completa del Full File System (FFS).
L'exploit è stato nativamente integrato in molti strumenti commerciali per anni, tuttavia oggi ha perso la sua efficacia e non è più funzionale sui dispositivi aggiornati a iOS 17.

### Strumentazioni
Tool come UFED, Inseyets e Magnet Graykey/Verakey permettono di automatizzare l'estrazione e bypassare i blocchi. Prima dell'acquisizione logica Android, ad esempio, richiedono l'attivazione della modalità aereo e delle opzioni sviluppatore (Debugging USB e trasferimento file).

UFED fornisce anche dispositivi standalone che supportano l'intero processo di acquisizione ed offrono la possibilità di fare deployment live di exploit (anche su dispositivi non sbloccati), fare brute forcing del codice di blocco e dump della ram.

### Log di Sistema su iOS
* **Unified Logs**: Sistema di registrazione eventi ad altissima granularità che traccia avvii, connessioni e stati di blocco. Sono estremamente volatili, venendo spesso sovrascritti nel giro di ore o al massimo un paio di giorni.
* **Sysdiagnose**: Funzione diagnostica di Apple capace di fotografare lo stato del dispositivo creando un archivio contenente log, configurazioni e metriche di consumo (PowerLogs). Rappresenta una miniera d'oro in ambito forense qualora il FFS non sia estraibile.
