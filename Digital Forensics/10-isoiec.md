# Gestione degli Incidenti e delle Prove Digitali: Standard ISO/IEC
Questo documento sintetizza le linee guida operative e metodologiche fornite dagli standard internazionali ISO/IEC 27035 e ISO/IEC 27037, fondamentali per la gestione strutturata degli incidenti di sicurezza informatica e per la corretta acquisizione e conservazione delle prove digitali.

## Standard ISO/IEC 27035: Gestione degli Incidenti di Sicurezza
L'ISO/IEC 27035 fornisce un approccio strutturato per identificare, valutare e rispondere agli incidenti informatici, con l'obiettivo di minimizzare l'impatto sul business e alimentare il miglioramento continuo del sistema ISMS (ISO/IEC 27001). 

La catena causale alla base dello standard prevede che una minaccia sfrutti una vulnerabilità di un asset, generando un *evento* di sicurezza che, se impatta le operazioni, si trasforma in un *incidente*.

### Ruoli Chiave
- **Point of Contact (PoC)**: Primo filtro aziendale che riceve le segnalazioni, effettua valutazioni preliminari e coordina l'intervento.
- **Incident Response Team (IRT)**: Team specialistico (noto anche come CERT o CSIRT) incaricato della gestione tecnica e operativa dell'incidente.

### Le 5 Fasi del Processo di Gestione
Il processo è iterativo e si articola in cinque fasi strettamente interconnesse:
1. **Pianificazione e preparazione**: Definizione di policy, costituzione dell'IRT, predisposizione di sistemi di logging e formazione del personale prima che l'incidente avvenga.
2. **Individuazione e documentazione**: Monitoraggio continuo, raccolta informazioni e registrazione accurata degli eventi anomali, preservando le evidenze.
3. **Valutazione e decisione**: Classificazione dell'evento per confermare se si tratti di un reale incidente e determinare il livello di risposta proporzionato (riducendo i falsi allarmi).
4. **Risposta (Response)**: Fase operativa di contenimento, eliminazione della minaccia, recovery dei sistemi e chiusura dell'incidente. Se l'incidente non è sotto controllo, si innesca la gestione della crisi.
5. **Apprendimento (Lessons Learned)**: Analisi post-incidente per identificare vulnerabilità, migliorare le procedure e aggiornare la valutazione del rischio.

### Focus Operativo: Attacco Ransomware
Il ransomware rappresenta l'incidente critico per eccellenza. Il ciclo di vita dell'attacco prevede: accesso iniziale (es. phishing), movimento laterale, esfiltrazione dati e infine la cifratura.
In caso di attacco:
- **Contenimento**: Disconnessione immediata dalla rete dei dispositivi compromessi, valutando attentamente lo spegnimento per non perdere i dati volatili in RAM.
- **Recovery**: La presenza di un backup offline integro (strategia 3-2-1) è il fattore discriminante tra un ripristino rapido e la perdita totale/pagamento del riscatto.
- **Analisi**: Necessaria per verificare l'esfiltrazione e adempiere agli obblighi di notifica (Garante Privacy, ACN), analizzando artefatti di sistema (Event Log, MFT, Amcache).


## Standard ISO/IEC 27037: Gestione delle Prove Digitali
L'ISO/IEC 27037 disciplina le procedure di primo intervento sulla scena del crimine informatico. Il suo perimetro è strettamente limitato all'identificazione, raccolta, acquisizione e conservazione della prova; **esclude deliberatamente la fase di analisi** per concentrarsi sulla protezione del dato da alterazioni.

### Ruoli Operativi
- **DEFR (Digital Evidence First Responder)**: Operatore qualificato che interviene per primo sulla scena, mettendo in sicurezza i dispositivi e preservando la catena di custodia.
- **DES (Digital Evidence Specialist)**: Esperto con competenze avanzate per gestire acquisizioni complesse (reti, sistemi cifrati, malware).

### Requisiti e Pilastri Metodologici
Una prova digitale, per essere ammissibile, deve rispettare tre requisiti legali:
- **Pertinenza**: Utilità diretta ai fini dell'indagine.
- **Affidabilità**: Prova genuina e non alterata.
- **Sufficienza**: Raccolta adeguata e proporzionata al caso.

Tutte le operazioni condotte dal DEFR/DES devono fondarsi su quattro pilastri: **Verificabilità, Ripetibilità, Riproducibilità e Giustificabilità** (ogni scelta tecnica deve poter essere difesa in sede di giudizio).

### Le 4 Fasi del Processo Forense
1. **Identificazione**: Riconoscimento dei dispositivi fisici (inclusi IoT, RFID, NAS) e dei dati volatili. La priorità assoluta è l'acquisizione dei dati in RAM.
2. **Raccolta**: Rimozione e trasporto dei dispositivi. La regola d'oro in questa fase è: *se un dispositivo è acceso, non va spento; se è spento, non va acceso*.
3. **Acquisizione**: Creazione della *copia forense certificata* (copia bit-a-bit che include spazio non allocato e slack space) apportando il minor numero possibile di alterazioni e documentandole rigorosamente.
4. **Conservazione**: Protezione dei reperti da danni fisici (campi magnetici, scariche elettrostatiche).

### La Catena di Custodia (Chain of Custody)
Costituisce la storia completa del reperto. È un registro formale che traccia in modo ininterrotto chi, quando, come e perché ha avuto accesso al dispositivo dal momento della raccolta fino al processo. Include sempre i **valori di hash (MD5, SHA)** utilizzati come firma digitale per dimostrare matematicamente l'integrità dei dati originari. L'assenza o la compilazione errata della catena di custodia compromette irreparabilmente l'utilizzabilità della prova.
