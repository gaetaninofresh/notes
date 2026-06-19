# 1 - Computer Forensics: Fondamenti Metodologici

> **Note per l'esame:** 
> La Digital Forensics poggia sul superamento della **prova di resistenza** giudiziaria. L'onere della prova dell'attendibilità del dato è interamente a carico di chi lo produce in giudizio, a causa della natura intrinsecamente manipolabile dei bit streams.

* **Valore Probatorio**: Il dato informatico è caratterizzato dalla presunzione di ripudio. Se non validato metodologicamente, si presume modificato o alterato.
* **Proprietà del Reperto**:
    * **Integrità**: Corrispondenza matematica esatta tra la sorgente originaria e la copia esibita, verificata e certificata tramite algoritmi di hashing.
    * **Autenticità**: Certezza della provenienza e invulnerabilità da alterazioni esterne, garantita dall'uso combinato di firme digitali e **Marcature Temporali Certificate** (attribuzione di data e ora certa e opponibile).
* **Catena di Custodia**: Documentazione cronologica e formale di ogni singola interazione con il reperto (chi, quando, come, con quale strumento). Qualsiasi lacuna invalida l'utilizzabilità processuale.
* **Fasi del Trattamento del Dato**:
    1.  **Identificazione**: Localizzazione delle potenziali fonti di prova sulla scena.
    2.  **Raccolta / Acquisizione**: Duplicazione forense o repertamento fisico.
    3.  **Conservazione**: Mantenimento dello stato di integrità del dato nel tempo.
    4.  **Analisi**: Estrazione ed esame scientifico delle evidenze tramite cross-referencing.
    5.  **Relazione / Presentazione**: Sintesi tecnica accessibile e dibattimento orale.
* **Bests Practice di Presentazione Orale**: Esposizione rigorosa contratta in una finestra di **10-20 minuti** (soglia critica di attenzione del Giudice). Obbligo di stabilire la parità fisica e di convertire le obiezioni dei CTP in domande tecniche argomentabili.

# 2 - L'Accertamento nelle Indagini Penali

* **Attori del Procedimento**: 
    * **CT PM**: Consulente Tecnico del Pubblico Ministero nominato ex **Art. 359 c.p.p.** (accertamenti ripetibili) o **Art. 360 c.p.p.** (accertamenti tecnici non ripetibili).
    * **CTP**: Consulente Tecnico di Parte, nominato dalla difesa a tutela del Diritto alla Difesa garantito dall'**Art. 24 Cost.**
* **Inquadramento Procedurale**: Differenza sostanziale tra *procedimento penale* (fase complessiva delle indagini preliminari) e *processo penale* (fase successiva all'esercizio dell'azione penale).
* **Ispezione e Perquisizione**: Regolate rispettivamente dall'**Art. 244 co. 2 c.p.p.** (ricerca di tracce digitali su sistemi) e dall'**Art. 247 co. 1-bis c.p.p.** (perquisizione di sistemi informatici o telematici).
* **Scenari Operativi (Live vs Dead)**:
    * **Operazioni Dead** (Dispositivo Spento): Scenario a basso rischio di alterazione. Si applicano **Write-Blocker** hardware (inibitori di scrittura sui supporti), si calcola l'hash **SHA-256** del bitstream e si esegue copia bit-a-bit.
    * **Operazioni Live** (Dispositivo Acceso): Obbligo di estrazione immediata della **RAM volatile** e dei registri di stato attivi prima di procedere allo spegnimento, per evitare la perdita di chiavi crittografiche o sessioni utente aperte.
* **Requisiti dello Standard**:
    * **Ripetibilità**: Possibilità di replicare l'accertamento ottenendo i medesimi risultati anche con strumenti software differenti.
    * **Imparzialità Tecnica**: Conduzione dell'indagine scientifica tesa alla ricerca della verità materiale, indipendentemente dal fatto che i risultati favoriscano l'accusa o la difesa.

# 3 - La Digital Forensics nel Processo Civile

> **Riferimenti Giuridici Base:**
> -   **Art. 2697 c.c.**: Onere della prova (l'attore deve provare i fatti costitutivi del proprio diritto).
> -   **Art. 2712 c.c.**: Riproduzioni meccaniche ed informatiche. Fanno piena prova dei fatti se colui contro il quale sono prodotte non ne disconosce espressamente la conformità.
> -   **Art. 20 e 21 CAD**: Il documento informatico acquisisce l'efficacia di scrittura privata ex **Art. 2702 c.c.** (presunzione di paternità) solo se sottoscritto con firma digitale, qualificata o avanzata.

* **Il Ruolo del CTU e del CTP**: Il Consulente Tecnico d'Ufficio (CTU) opera come ausiliario del Giudice. I CTP delle parti monitorano l'operato del CTU garantendo il rispetto dei principi di Lealtà, Correttezza, Trasparenza e Buona Fede.
* **Limiti del GDPR**: Il trattamento forense nel civile deve sottostare rigidamente ai principi di minimizzazione dei dati e limitazione delle finalità. Il CTU ha l'obbligo tassativo di **espunzione dei dati irrilevanti** (es. comunicazioni personali, chat familiari, cartelle mediche non pertinenti) prima del deposito.
* **Inammissibilità**: Una prova digitale raccolta in violazione delle tutele della privacy o senza garantire l'integrità del supporto è passibile di nullità o inutilizzabilità, determinando la responsabilità civile e penale del perito.
* **Quantificazione del Danno da Illecito Informatico**:
    1.  **Danno Emergente**: Costi diretti documentati per indagini forensi, remediation sistemistica e contenimento.
    2.  **Lucro Cessante**: Mancato guadagno o perdita di quote di mercato direttamente riconducibili all'azione illecita.
    3.  **Royalty Equivalente**: Determinazione del danno basata sul "prezzo del consenso", ovvero il valore di licenza che il violatore avrebbe dovuto pagare per l'uso legittimo della risorsa.

# 4 - Informatica Forense e Firme Elettroniche

* **Ontologia del Documento**: Struttura duale inscindibile composta da **Bitstream** (rappresentazione binaria pura del contenuto) + **Metadata** (informazioni di contesto memorizzate all'interno dell'architettura del file o del file system).
* **Gerarchia Normativa delle Firme (Regolamento eIDAS - Reg. UE 910/2014)**:
    1.  **FES (Semplice)**: Dati elettronici allegati ad altri dati (es. nome in calce a una mail, PIN, credenziali). Valore probatorio debole, liberamente valutabile dal Magistrato.
    2.  **FEA (Avanzata)**: Connessa univocamente al firmatario, idonea a identificarlo, creata con dati sotto il controllo esclusivo del firmatario e collegata al documento in modo da rilevare modifiche successive (Art. 26 eIDAS).
    3.  **FEQ / Firma Digitale**: FEA basata su un certificato qualificato e creata mediante un dispositivo sicuro (Token/HSM). Ha l'efficacia della scrittura privata e inverte l'onere della contestazione della firma.
* **Analisi Forense delle Email (Cassazione 14046/2024)**: Un'email priva di firma digitale non può essere scartata a priori dal giudice, ma costituisce documento informatico (FES). La validazione tecnica richiede l'esame di:
    * **Header RFC 5322**: Analisi della catena dei server nei campi *Received* per intercettare IP spoofing o falsificazioni.
    * **Protocolli di Autenticazione**: Verifica dell'allineamento di **SPF** (Sender Policy Framework), **DKIM** (DomainKeys Identified Mail, firma crittografica dell'header) e **DMARC** (Domain-based Message Authentication, Reporting and Conformance).
    * **Struttura MIME**: Verifica dell'integrità dei *boundary* (separatori di sezione) per rilevare l'iniezione postuma di allegati o la modifica del testo HTML.
* **PDF Incremental Updates**: L'architettura interna di un PDF firmato prevede l'aggiunta di modifiche in modalità incrementale (append) senza sovrascrivere i byte coperti dalla firma originale. Tramite l'analisi dei byte-offset con tool quali `pdf-parser.py`, `pdfid.py` o `mutool` il perito deve estrarre le versioni storiche e verificare le autorizzazioni **MDP / UR (User Rights)**.
* **Conservazione a Norma / LTA**: Procedura centralizzata per estendere la validità legale oltre la scadenza dei certificati o l'obsolescenza degli algoritmi (es. transizione da SHA-1 a SHA-256, o RSA-1024 debilitati). Utilizza formati **LTA (Long Term Archive)** con marcatura temporale concatenata (*chained timestamping*).

# 5 - D.Lgs. 231/2001 e Digital Forensics Aziendale

* **Evoluzione del Quadro dei Reati**: L'estensione della responsabilità amministrativa degli enti include i reati informatici. Rilevano la **L. 90/2024** (nuove fattispecie di estorsione informatica e attacchi a infrastrutture critiche) e la **L. 132/2025** (circostanze aggravanti specifiche per l'uso di sistemi di Intelligenza Artificiale nella commissione del reato).
* **Esimente ex Art. 6**: L'ente evita la responsabilità se dimostra l'adozione e l'efficace attuazione di un **MOG (Modello di Organizzazione, Gestione e Controllo)** idoneo.
* **Requisiti Forensi e di Controllo del MOG**:
    * Mappatura e inventario stringente degli asset informatici.
    * Certificazione immutabile digitale delle attività di formazione del personale.
    * Implementazione tecnica di protocolli di **SoD (Segregazione delle Funzioni)** e architetture di **Identity & Access Management (IAM)**.
* **Intersezione con NIS2 e DORA**: La violazione conclamata dei requisiti cyber imposti dalle direttive europee NIS2 e DORA si configura come evidenza di "colpa organizzativa", annullando l'esimente del MOG.
* **Whistleblowing (D.Lgs. 24/2023)**: Obbligo di adozione di canali di segnalazione digitali cifrati (es. **GlobaLeaks** o Whistleblowing PA) ad accessi rigorosamente segregati per i soli membri dell'**OdV (Organismo di Vigilanza)**. 
* **Vincolo Forense sul Whistleblowing**: In caso di indagini interne generate da una segnalazione, l'analista forense deve blindare l'anonimato del segnalante. È vietata l'analisi di metadati o log IP che possano condurre, anche indirettamente, alla de-anonimizzazione del whistleblower (Art. 17 - Divieto di ritorsione).

# 6 - GDPR e Gestione dei Data Breach

* **Principio di Accountability (Art. 5.2 GDPR)**: Inversione dell'onere probatorio aziendale. Il Titolare deve implementare misure adeguate (cifratura, pseudonimizzazione, log di audit granulari) ed essere in grado di dimostrarne l'efficacia ex-ante tramite documentazione tecnica dettagliata e **DPIA** (Valutazione d'Impatto).
* **Sanzioni Edittali (Art. 83 par. 5)**: Fino a 20.000.000 € o fino al 4% del fatturato mondiale totale annuo dell'esercizio precedente per violazioni dei principi cardine o dei diritti degli interessati. Responsabilità civile solidale per danno ex **Art. 82**.
* **Ambito Applicativo ed Esclusioni**: Trattamento automatizzato o archiviazione strutturata di dati personali. Escluse le attività domestiche e i trattamenti penali/giudiziari, governati dalla **Direttiva LED (2016/680)**. I dati ex **Art. 9** (particolari/sanitari/biometrici) e **Art. 10** (giudiziari) impongono log di audit non modificabili.
* **Procedura Forense di Incident Response**:
    * **Tempistiche di Notifica**: Segnalazione formale al Garante Privacy entro **72 ore** dalla scoperta del breach (Art. 33).
    * **RAM Capture**: Divieto assoluto di spegnere o riavviare i sistemi infetti prima dell'estrazione della memoria volatile tramite tool certificati (**WinPmem**, **DumpIt**), al fine di preservare indicatori di compromissione (IoC) volatili.
    * **Paziente Zero**: Analisi forense finalizzata all'individuazione del vettore di accesso iniziale (**Initial Access** secondo la matrice *MITRE ATT&CK*). Senza l'esatta individuazione del Paziente Zero, il perimetro del breach non è giuridicamente quantificabile.

## 6a - Cyber Resilience Act (CRA)

* **Quadro Regolamentare (Reg. UE 2024/2847)**: Introduce requisiti orizzontali di cybersecurity obbligatori per tutti i prodotti hardware e software dotati di elementi digitali immessi nel mercato UE. La sicurezza diventa precondizione per la **Marcatura CE**.
* **Pilastri Operativi**: Obbligo di approccio **Security by Design**, assenza totale di vulnerabilità note e sfruttabili al momento del rilascio commerciale, gestione continuativa del ciclo di vita del prodotto tramite il rilascio obbligatorio di aggiornamenti di sicurezza.
* **Evidenze Tecniche**: Redazione della Documentazione Tecnica comprensiva della **SBOM (Software Bill of Materials)** e della Dichiarazione di Conformità (DoC) prima della commercializzazione.

## 6b - Direttiva NIS2

* **Recepimento Nazionale (D.Lgs. 138/2024)**: Quadro per la resilienza cibernetica di soggetti Essenziali e Importanti operanti in settori critici.
* **Architettura delle Notifiche ad ACN (Agenzia per la Cybersicurezza Nazionale)**:
    1.  **Early Warning**: Entro **24 ore** dalla scoperta di un incidente significativo.
    2.  **Incident Notification**: Entro **72 ore**, contenente la valutazione iniziale della gravità e gli indicatori di compromissione.
    3.  **Final Report**: Entro **1 mese**, comprensivo della *root cause analysis* (causa radice) e del piano dettagliato di remediation.
* **La Data Room di Compliance**: Obbligo di mantenimento e storicizzazione delle evidenze operative in sede di ispezione: *risk register* aggiornati, log dei sistemi di monitoraggio, verbali firmati degli organi direttivi, report formali di audit sui fornitori della *supply chain*. Ciò che non è documentato equivale a non eseguito.

# 7 - Digital Forensics e Privacy del Lavoratore

> **Riferimenti Giuridici Base:**
> -   **Art. 4 Statuto dei Lavoratori (SdL)**: Divieto di controllo a distanza dell'attività lavorativa. Gli impianti audiovisivi e gli altri strumenti da cui derivi anche la possibilità di controllo a distanza possono essere impiegati esclusivamente per esigenze organizzative e produttive, per la sicurezza del lavoro e per la tutela del patrimonio aziendale.
> -   **Riforma Jobs Act**: Introduce la separazione tra strumenti di controllo generici e "strumenti di lavoro" (es. smartphone, PC aziendali, badge), i quali sono esentati dall'accordo preventivo a condizione che sia fornita al lavoratore un'adeguata informativa.

* **Canali di Autorizzazione per Strumenti Generici**: In mancanza di accordo scritto con le rappresentanze **Sindacali** aziendali, il datore di lavoro deve presentare istanza amministrativa formale all'Ispettorato Territoriale o Nazionale del Lavoro (**ITL/INL**), allegando il progetto tecnico dettagliato delle misure e delle finalità (es. videosorveglianza con limite di conservazione standard a **48 ore**).
* **Esoneri Speciali dall'Autorizzazione**: Cronotachigrafo, sistemi Telepass installati su flotta aziendale e geolocalizzazione GPS di smartphone/veicoli aziendali (se strettamente necessari alla prestazione lavorativa).
* **Il "Territorio Grigio" tecnologico**: Dispositivi embedded quali terminali indossabili, *smart glove* o *smart watch* azzerano la separazione tra la metrica dell'esecuzione del lavoro e il monitoraggio biometrico/personale del dipendente, imponendo limitazioni stringenti sul trattamento dei dati.
* **Controlli Difensivi Aziendali**: Indagini mirate ad accertare specifiche condotte illecite ex-post. Requisiti di legittimità:
    * Presenza di un **sospetto preesistente, mirato e oggettivamente verificabile** nei confronti di un soggetto specifico.
    * Apertura formale di un fascicolo di indagine difensiva curato da un avvocato.
    * Divieto assoluto di eseguire controlli massivi, preventivi o predittivi alla ricerca di illeciti generici.
* **Il Framework Giurisprudenziale (Caso Bărbulescu c. Romania - CEDU 2017)**: Fissa i 6 criteri obbligatori per validare il controllo della corrispondenza aziendale:
    1.  *Informativa Preventiva*: Il lavoratore deve essere avvisato chiaramente della facoltà di controllo dei dispositivi aziendali.
    2.  *Estensione*: Distinzione rigorosa tra l'accesso ai soli metadati e l'accesso al contenuto delle comunicazioni (quest'ultimo fortemente limitato).
    3.  *Giustificazione e Proporzionalità*: Presenza di motivazioni legittime e assenza di misure meno intrusive.
    4.  *Conseguenze e Garanzie*: Tutela contro l'accesso non preavvisato ai contenuti.
* **Provvedimento Garante 21/12/2023**: Restrizioni rigide sulla conservazione dei metadati di posta elettronica dei dipendenti (es. timestamp, mittente, destinatario), la cui retention prolungata oltre i limiti tecnici minimi configura un controllo a distanza illegittimo.
* **Rilevanza Penale**: I controlli abusivi integrano le fattispecie di cui all'**Art. 615-ter c.p.** (accesso abusivo a sistema informatico protetto da misure di sicurezza) e l'intercettazione/illecita diffusione di comunicazioni informatiche o telematiche.

# 8 - Architettura del File System e Artefatti Windows

* **NTFS (New Technology File System)**: Organizza lo storage logico suddividendo lo spazio in **Cluster** (composti a loro volta da **Settori** fisici di dimensione fissa). Gestisce l'indicizzazione del volume tramite File Speciali contraddistinti dal prefisso `$`:
    * `$Boot`: Primo settore del volume; contiene il *BIOS Parameter Block* (BPB) e punta alla posizione di partenza della MFT.
    * `$MFT` (Master File Table): Database relazionale in cui ogni file o cartella possiede almeno un record fisso di **1024 byte**.
    * `$Bitmap`: Tabella binaria di allocazione dei cluster; mappa lo stato di ogni blocco (0 = libero, 1 = occupato).
    * `$LogFile`: Registro transazionale dei metadati; logga le modifiche per garantire il ripristino del file system in caso di crash.
    * `$AttrDef`: Dizionario contenente le definizioni di tutti gli attributi del file system (es. attributo `$STANDARD_INFORMATION`).
    * `$Secure`: Database centralizzato dei descrittori di sicurezza del volume; contiene le **ACL** (Access Control List) e i permessi.
    * `$UsnJrnl` (Update Sequence Number Journal): Giornale delle modifiche che storicizza cronologicamente ogni operazione eseguita su file e directory.
    * `$Volume`: Memorizza i metadati del volume logico (Label e Serial Number).
    * `$Data`: Contiene il flusso dei dati effettivi del file. Se la dimensione del file è inferiore a circa 700 byte, i dati vengono scritti direttamente nel record della MFT (**Dati Residenti**); se superiore, vengono allocati cluster esterni (**Dati Non Residenti**).
* **Struttura dei Timestamp MACE**: NTFS traccia quattro metriche temporali (*Modified, Accessed, Created, Entry Modified*) duplicate all'interno di due diversi attributi della MFT:
    * `$STANDARD_INFORMATION`: Timestamp modificabili e alterabili da API utente (vulnerabili a tecniche di *timestomping*). L'attributo *Accessed* è spesso disattivato di default da Microsoft per ottimizzare le performance I/O.
    * `$FILE_NAME`: Timestamp gestiti direttamente ed esclusivamente dal kernel del sistema operativo; non sono modificabili tramite normali funzioni utente. Rappresentano il termine di confronto forense per rilevare alterazioni.
* **Slack Space**: Spazio residuo che si genera all'interno dell'ultimo cluster allocato a un file, dovuto alla discrepanza tra la dimensione variabile del file e la dimensione fissa dei blocchi di memoria. Si divide in:
    * *RAM Slack*: Spazio compreso tra la fine fisica del file e la fine del settore corrente (può contenere residui della memoria di swap).
    * *Drive Slack*: Spazio compreso tra la fine del settore corrente e la fine complessiva del cluster allocato (contiene frammenti storici di file precedentemente memorizzati e poi cancellati).
* **Tecniche di Carving**: Estrazione di dati in assenza di puntatori del file system (es. supporti formattati o partizioni danneggiate):
    * *File Carving*: Identificazione e ricostruzione del file basata sulla ricerca di sequenze binarie note di **Header** (inizio file) e **Footer** (fine file).
    * *Data Carving*: Estrazione mirata di stringhe di testo o strutture dati specifiche ignorando l'integrità complessiva del contenitore del file.
* **VSS (Volume Shadow Copy Service)**: Servizio di snapshot di sistema che crea copie shadow incrementali di file e volumi. Permette al perito di viaggiare a ritroso nel tempo per recuperare versioni storiche di file modificati o cancellati dall'utente.

# 8+ Artefatti di Sistema Avanzati (Windows)

* **RecentDocs**: Chiave di registro che storicizza gli ultimi **150 file** aperti dall'utente, categorizzandoli per estensione di file. Conserva il timestamp dell'ultima interazione; necessita di cross-referencing a causa della facilità di sovrascrittura.
* **Shellbags**: Set di chiavi di registro che memorizzano le preferenze di visualizzazione delle cartelle da parte dell'utente. 
    * *Rilevanza forense*: Forniscono la prova storica incontestabile dell'accesso e dell'interazione dell'utente con specifiche directory o archivi ZIP, inclusi volumi esterni (es. chiavette USB) o condivisioni di rete non più connessi al sistema al momento dell'analisi.
* **UserAssist**: Artefatto del registro che traccia l'esecuzione di tutte le applicazioni dotate di interfaccia grafica (GUI) avviate dall'utente. I dati sono codificati in formato *ROT13*.
    * *Rilevanza forense*: Memorizza il contatore esatto delle esecuzioni del programma (**Run Count**) e il tempo cumulativo di permanenza dell'applicazione in primo piano (**Focus Time**).
* **Prefetch (`.pf`)**: File generati automaticamente nella directory `%SystemRoot%\Prefetch` per ottimizzare il caricamento delle applicazioni. Storicizzano i file binari eseguiti sul sistema, memorizzando il timestamp dell'ultima esecuzione (e fino alle ultime 8 esecuzioni nelle versioni Windows moderne), i file e le DLL caricate nei primi 10 secondi di avvio.
* **LNK Files / Jump List**: File di collegamento generati dal sistema o dall'utente. 
    * *Regola Forense del file LNK*: La data di **creazione** del file LNK indica la prima apertura assoluta del file originale sul sistema; la data di **modifica** del file LNK corrisponde cronologicamente all'ultimo accesso eseguito dall'utente sul file originale.
* **Browser Forensics**: Analisi dei database in formato **SQLite** utilizzati dai browser web per memorizzare cronologia, cookie, cache e sessioni. 
    * *La Modalità Incognito*: La navigazione privata inibisce la scrittura persistente sui DB SQLite, ma lascia tracce estratti tramite **Memory Carving** della RAM attiva, leak informativi all'interno dei file di Prefetch e record storici dei file scaricati all'interno delle Jump List e dei file LNK.

# 9 - Cloud Forensics

* **Infrastruttura e Accesso**: Assenza di accesso fisico diretto ai dischi rigidi. L'acquisizione avviene esclusivamente tramite logica remota, sfruttando le **API** del provider o i pannelli di amministrazione centralizzati.
* **Azure Legal Hold**: Strumenti di congelamento dei dati in ambiente Microsoft Azure:
    * *Policy di Immutabilità su Azure Blob Storage*: Impediscono la modifica o la cancellazione dei dati memorizzati nei container per un intervallo di tempo predefinito e vincolante.
    * *Resource Locks*: Vincoli applicati a livello di risorsa o gruppo di risorse che bloccano le operazioni di eliminazione e modifica anche per gli utenti amministrativi, formalizzando l'integrità della catena di custodia in Cloud.
* **Piattaforme di Analisi**: Integrazione forense tramite suite commerciali quali **Magnet AXIOM** e **FTK (Forensic Toolkit)**, dotate di connettori nativi per l'estrazione e la correlazione dei log cloud.
* **Metriche di Scelta del Metodo**: La scelta della strategia di acquisizione dipende dall'equilibrio di quattro variabili di contesto: **Tempo** di esecuzione, **Costo** computazionale/licenze, stato della **VM Attiva** (macchina virtuale accesa/spenta) e requisiti di **Scalabilità** dell'infrastruttura.

# 10 - Protezione Forense dei Segreti Commerciali

* **Requisiti di Tutela (Art. 98 Codice della Proprietà Industriale - CPI)**: Per accedere alla tutela civilistica contro la sottrazione di know-how e asset aziendali, un'informazione deve soddisfare contemporaneamente tre requisiti:
    1.  **Segretezza**: L'informazione non deve essere nota o facilmente accessibile ad esperti e operatori del settore nella sua esatta configurazione.
    2.  **Valore Economico**: La segretezza dell'informazione deve garantire un vantaggio competitivo diretto sul mercato (la divulgazione deve comportare una perdita economica).
    3.  **Misure Ragionevolmente Adeguate**: Il titolare deve aver implementato misure di sicurezza adeguate allo stato dell'arte per proteggere il dato (art. 98 lett. c CPI).
* **Misure di Sicurezza ex-ante**: La giurisprudenza individua la conformità forense prendendo come benchmark lo standard **ISO/IEC 27001:2022**. Esempi di misure: sottoscrizione di **NDA** (accordi di riservatezza), implementazione di sistemi **DLP (Data Loss Prevention)** e introduzione di policy restrittive sull'uso aziendale dei sistemi di Intelligenza Artificiale.
* **Segreto vs Brevetto**: Il brevetto impone la pubblicazione dell'invenzione e ha una durata limitata a **20 anni** (per il software è applicabile solo come *Computer-Implemented Inventions* dotate di carattere tecnico ex **Art. 52 CBE**). Il segreto commerciale non è registrato, ha durata potenzialmente illimitata nel tempo, ma la sua diffusione — anche colposa o accidentale — lo distrugge in modo definitivo e irreversibile.
* **Analisi Forense sul Furto di Know-How**:
    * Ricerca tramite stringhe di testo e parole chiave all'interno dei log di sistema.
    * Applicazione di algoritmi di **Fuzzy Hashing** (es. *ssdeep*) per intercettare file parzialmente modificati o codice sorgente parzialmente alterato dall'ex dipendente.
    * Analisi dei metadati dei file system per dimostrare l'esportazione illecita su supporti USB esterni o canali email personali.
* **Il Caso Samsung 2023**: Dipendenti caricarono codice sorgente proprietario e verbali confidenziali su ChatGPT per ottimizzare il lavoro, provocando la perdita del requisito di segretezza a causa dell'acquisizione dei dati da parte del fornitore dell'LLM. Ha determinato l'adozione di massa di sistemi DLP dedicati al blocco degli LLM non aziendali.

# 11 - Il Software come Opera dell'Ingegno

* **Quadro Normativo**: Il software è tutelato dal Diritto d'Autore (**L. 633/1941 - L.d.A.**) e dalla **Direttiva 2009/24/CE**. Non rientra nel CPI, salvo per rari profili brevettuali legati all'hardware.
* **Oggetto della Protezione (Art. 2 n. 8 L.d.A. e Sentenza CGUE SAS Institute v. WPL - 2012)**:
    * *Protetto*: Il codice sorgente, il codice oggetto e tutto il materiale preparatorio di progettazione (forma espressiva).
    * *NON Protetto*: Le idee alla base del software, gli algoritmi matematici, le funzionalità del programma, i linguaggi di programmazione, i formati dei file e l'architettura delle interfacce grafiche puramente funzionali (**CGUE C-393/09 BSA - 2010**).
* **Tipologie di Plagio del Codice**:
    * *Plagio Letterale*: Copia speculare, diretta e non autorizzata del codice sorgente o di sue parti rilevanti (operazioni di *copy-paste*).
    * *Plagio Non Letterale*: Copia dell'architettura logica, della sequenza e dell'organizzazione del software (definita nella giurisprudenza USA come **SSO - Structure, Sequence, and Organization**). Richiede la separazione tra lo sviluppo e l'analisi tramite audit indipendenti.
* **Reverse Engineering**: Generalmente vietato. È consentito eccezionalmente dall'**Art. 64-ter L.d.A.** esclusivamente se eseguito dall'utente legittimo per finalità di studio del funzionamento, correzione di bug critici o per garantire l'**interoperabilità** con sistemi di terze parti.
* **Il Test AFC (Abstraction-Filtration-Comparison)**: Protocollo peritale obbligatorio nelle indagini di plagio:
    1.  **Abstraction**: Scomposizione del software in livelli logici progressivi (codice sorgente, funzioni, macro-moduli, architettura).
    2.  **Filtration**: Eliminazione dall'analisi di tutti gli elementi non originali, come il codice di pubblico dominio, le librerie open source, i vincoli tecnici obbligatori e le **scènes à faire** (soluzioni standardizzate obbligate dal problema). Si applica la *merger doctrine* (quando idea ed espressione coincidono, l'elemento non è proteggibile).
    3.  **Comparison**: Confronto delle somiglianze residue. Solo le uguaglianze scoperte in questa fase configurano il plagio.
* **Metodologie di Analisi Tecnica**:
    * *Livello Lessicale*: Algoritmi di *string matching* e comparazione del testo.
    * *Livello Strutturale*: Analisi dei token, generazione dell'**AST (Abstract Syntax Tree)**, del *PDG (Program Dependence Graph)* e del *CFG (Control Flow Graph)* per intercettare operazioni di offuscamento del codice.
    * *Livello Semantico*: Analisi dinamica del comportamento del software in esecuzione.
* **Strumenti Processuali Nazionali ed Europei**:
    * *Descrizione e Sequestro*: Artt. 161-162 L.d.A., eseguibili anche *inaudita altera parte* (senza preavviso alla controparte) per cristallizzare il codice sorgente sui server prima della sua alterazione.
    * *ATP*: Accertamento Tecnico Preventivo ex **Art. 696-bis c.p.c.**
    * *Regolamento e-Evidence (Reg. UE 2023/1543)*: In vigore dal **18 agosto 2026**, introduce l'Ordine europeo di produzione e conservazione delle prove elettroniche direttamente ai provider esteri.
* **Titolarità del Software Aziendale**: In caso di lavoro subordinato, i diritti di sfruttamento patrimoniale spettano automaticamente al datore di lavoro. Il **Diritto di Paternità** morale dell'opera resta inalienabile in capo al programmatore persona fisica.

# 12 - Valutazione Forense e Stima del Software

* **Metriche Fondamentali di Stima**: Determinazione economica basata sul **Valore Effettivo** di mercato o sul **Costo di Sviluppo** (riproduzione).
* **Contesti di Applicazione**: Operazioni di *Due Diligence* in contesti M&A (Fusioni/Acquisizioni), rivalutazione di asset immateriali da iscrivere nella voce **B.I.3 dello Stato Patrimoniale (Art. 2424 c.c.)** secondo i principi contabili **OIC 24**, e quantificazione del danno da plagio o pirateria.
* **Fasi Operative della Perizia**:
    1.  Verifica dell'esistenza materiale e della consistenza tecnica del codice sorgente.
    2.  Audit e verifica di conformità delle licenze **Open Source** integrate.
    3.  Accertamento della titolarità dei diritti e dei contratti di cessione IP.
    4.  Calcolo economico tramite l'applicazione e la riconciliazione di tre metodologie: *Cost Approach* (metrica dei costi interni), *Market Approach* (confronto transazioni di mercato) e *Income Approach* (attualizzazione dei flussi di cassa prospettici).
* **Responsabilità del Perito Estimatore**: Il CTU risponde civilmente e penalmente della perizia ex **Art. 64 c.p.c.** (integrazione del reato di falsa perizia ex **Art. 373 c.p.**) in caso di colpa grave o dolo.
* **Tipologie di Frode Tecnico-Economica**:
    * *Commit Artificiali*: Iniezione automatizzata di righe di codice ripetitive o fittizie nei repository Git per simulare un'attività di sviluppo inesistente e gonfiare le metriche basate sulle LOC (Lines of Code).
    * *Refactoring Cosmetico*: Modifica superficiale dei nomi delle variabili e dei commenti prima della perizia forense al fine di alterare artificialmente gli indici di qualità statica del codice sorgente.
* **Stima dei Modelli di Intelligenza Artificiale**: Disciplina priva di standard consolidati. Richiede la scomposizione del valore economico in due macro-asset: il valore del **Dataset di addestramento** (valutato in base a qualità, volume, pulizia e fattore di esclusività dei dati) e il valore del **Modello addestrato** (valutato in base all'architettura neurale, all'accuratezza metrica e alla capacità di generalizzazione).

# 13 - Digital Forensics Sanitaria

* **Framework Normativo Integrato**: Artt. 4 e 9 GDPR (trattamento categorie speciali di dati relativi alla salute), **D.Lgs. 101/2018** (adeguamento Codice Privacy italiano), requisiti di cybersecurity by design imposti ai produttori di dispositivi medicali connessi (**IoMT**) dal **Regolamento UE 2017/745 (MDR)**, e precetti del Codice Deontologico Medico.
* **CCE (Cartella Clinica Elettronica)**: Si configura giuridicamente come un **Atto Pubblico di fede privilegiata** ai sensi degli **Artt. 2699 c.c.** e seguenti. Il suo contenuto fa piena prova fino a querela di falso.
    * *Compito Forense*: Verifica dell'integrità dei log di audit associati alla CCE per escludere alterazioni ex-post, accessi abusivi da parte di personale non autorizzato o modifiche retroattive dei parametri clinici.
* **Il Conflitto Giuridico di Contesto**: Tensione irriducibile tra il dovere di **Segreto Professionale Medico (Art. 622 c.p.)** e l'**Obbligo di Referto (Art. 365 c.p.)** che impone la denuncia immediata dei reati perseguibili d'ufficio. L'acquisizione forense di cartelle cliniche e dati IoMT da parte degli inquirenti richiede un **Decreto Motivato** dell'Autorità Giudiziaria nel rispetto dei principi di proporzionalità del GDPR, pena l'inutilizzabilità assoluta della prova ex **Art. 191 c.p.p.**
* **Verifica Forense dei Sensori IoMT**: Analisi e correlazione dei log di input/output dei sensori medici per verificare la corrispondenza tra i comandi impartiti dai medici e le risposte fornite dal sistema, accertando la presenza della marcatura CE del software come dispositivo medico (MDR).

# 14 - Mobile Forensics: Livelli di Acquisizione

* **La Tassonomia dei Cinque Livelli di Acquisizione**:
    1.  **Esame Manuale**: Interazione fisica visiva con lo schermo del dispositivo; documentazione tramite acquisizione fotografica/video esterna dei contenuti esistenti.
    2.  **Acquisizione Logica**: Estrazione selettiva degli oggetti logici attivi e non cancellati memorizzati nelle API utente (es. contatti, SMS, file multimediali visibili).
    3.  **Acquisizione File System (Logica Avanzata)**: Estrazione parziale o totale delle directory e dei file utente. Su piattaforme **iOS** si sfrutta il protocollo di backup combinato con l'**AFC (Apple File Conduit)**; su piattaforme **Android** si ricorre a protocolli di backup associati a connessioni **MTP** (Media Transfer Protocol) e interfacce **ADB** (Android Debug Bridge) previa attivazione del *Debugging USB*.
    4.  **Full File System (FFS)**: Estrazione speculare e completa di tutte le partizioni del file system, inclusi i database protetti delle applicazioni, le aree di cache e i dati residui/cancellati non ancora sovrascritti. Richiede l'esecuzione di exploit root (spesso commerciali e non pubblici).
    5.  **Acquisizione Fisica**: Dump speculare bit-a-bit dell'intera memoria flash del dispositivo dal primo all'ultimo bit. Richiede interventi hardware intrusivi quali connessioni **JTAG**, tecniche **ISP** (In-System Programming) o dissaldatura fisica delle memorie (**Chip-Off**).
* **L'Exploit Hardware Checkm8**: Creato dall'hacker *axiomX*. Si tratta di una vulnerabilità permanente di tipo unpatchable situata all'interno della maschera ROM di avvio (**BootROM**) dei chip Apple compresi tra il SoC A5 e il SoC A11 (dall'iPhone 4S all'iPhone X). 
    * *Rilevanza forense*: Permette il bypass completo delle restrizioni logiche del sistema operativo per l'estrazione del FFS. Non è patchabile via software da Apple, ma ha azzerato la sua efficacia operativa sui dispositivi aggiornati a **iOS 17** e successivi.
* **Log di Sistema Avanzati (iOS)**:
    * **Unified Logs**: Registro eventi di sistema ad altissima granularità. Storicizza le transizioni di stato del device (avvii, blocchi, agganci di rete). Caratterizzato da una **volatilità estrema**: i dati vengono sovrascritti ciclicamente nel giro di poche ore o al massimo un paio di giorni.
    * **Sysdiagnose**: Funzione nativa di diagnostica Apple che genera un archivio compresso contenente lo stato del sistema, la configurazione dei moduli e i log storici sui consumi energetici (**PowerLogs**). Costituisce la principale risorsa forense qualora l'estrazione FFS sia bloccata dalle protezioni hardware del dispositivo.

# 15 - IoT (Internet of Things) Forensics

* **Paradigma Architetturale**: L'ecosistema IoT è decentralizzato per natura: la logica operativa e lo storage dei dati storici vengono sistematicamente delegati a server cloud remoti, riducendo la persistenza delle informazioni sul dispositivo fisico locale.
* **Fonti Forensi Consumer**:
    * *Smart Display (Amazon Echo Show 15)*: L'estrazione richiede connessioni hardware sulla scheda logica tramite tecniche ISP; permette l'analisi dell'archiviazione locale dei database di interazione utente.
    * *Apple TV*: Estrazione finalizzata alla ricostruzione della timeline dettagliata delle connessioni di rete e delle interazioni multimediali dell'account associato.
    * *Elettrodomestici Smart (Roomba 690)*: I dati vengono estratti tramite rimozione fisica della memoria e lettura in modalità *Chip-Off* (eseguita da laboratori specializzati quali *VTO Labs*). L'artefatto evidenzia l'**assenza totale di cifratura**, memorizzando in chiaro all'interno dei log: numero di serie, fuso orario, log storici di utilizzo, **SSID e password delle reti Wi-Fi in chiaro**.
    * *Samsung Smart Fridge*: Analisi ed estrazione tramite Chip-Off dei moduli di memoria per il recupero dei log delle attività utente.
* **Ambito Industrial IoT / OT Forensics**: Analisi forense applicata a infrastrutture critiche e reti industriali:
    * Estrazione dei log di accesso e degli allarmi dalle interfacce **SCADA / HMI**.
    * Intercettazione e analisi dei pacchetti di telemetria industriale basati su protocolli **MQTT**, **Modbus** e tracciamento dei log di accesso alle **VPN** industriali.
    * *Sfide Strutturali*: Eterogeneità estrema dei sistemi operativi proprietari dei vendor, volatilità strutturale dei registri di memoria PLC, limitazioni logistiche nell'accesso ai siti industriali e vincolo assoluto di **Continuità Operativa** (impossibilità di spegnere i sistemi di produzione per eseguire copie forensi tradizionali).

# 16 - ISO/IEC 27035 e Gestione Incidenti Ransomware

* **Fase di Ricognizione**: Ispezione preliminare e mappatura completa dello stato logico dell'infrastruttura di rete compromessa per censire gli asset infetti e determinare la tipologia di ransomware.
* **Ordine di Volatilità e RAM Capture**: Obbligo di estrazione della memoria volatile e degli artefatti live prima di qualsiasi interruzione dell'alimentazione dei sistemi, per scongiurare la distruzione definitiva delle chiavi di cifratura del ransomware memorizzate esclusivamente in RAM.
* **Analisi delle Evidenze e Ricostruzione della Timeline**: Correlazione incrociata degli artefatti del file system per determinare le azioni dell'attaccante:
    * Esame sistematico degli **Event Log** di Windows per intercettare movimenti laterali o esecuzione di script abusivi.
    * Analisi della **MFT** (Master File Table) per mappare la cronologia esatta della cifratura di massa dei file.
    * Analisi dei file **LNK**, della **Web Cache** dei browser e dei dati di consumo memorizzati nel database **SRUM** (System Resource Usage Monitor) per ricostruire la timeline complessiva dell'intrusione.
    * Acquisizione dei log perimetrali dei **Firewall** e dei server **VPN** per validare il perimetro dell'attacco ed escludere l'esfiltrazione di dati ex-ante.

# 17 - ISO/IEC 27037: Gestione delle Prove Digitali

> **Attenzione - Limite Dello Standard:**
> Lo standard ISO/IEC 27037 fornisce linee guida armonizzate esclusivamente per le fasi di **Identificazione, Raccolta, Acquisizione e Conservazione** delle prove digitali. Lo standard **NON disciplina in alcun modo la fase di Analisi Forense** delle evidenze e la successiva stesura della relazione peritale.

* **Definizioni di Standardizzazione**:
    * **DEFR (Digital Evidence First Responder)**: Personale qualificato autorizzato a operare sulla scena del crimine informatico per l'identificazione e la messa in sicurezza dei reperti digitali.
    * **DES (Digital Evidence Specialist)**: Specialista forense incaricato delle procedure complesse di acquisizione ed esame tecnico avanzato.
    * **Riproducibilità**: Principio secondo cui le operazioni forensi eseguite sui reperti devono poter essere replicate da soggetti terzi con strumenti software/hardware differenti, pervenendo a risultati identici.
    * **Giustificabilità**: Obbligo per l'operatore (DEFR/DES) di argomentare e dimostrare scientificamente che la scelta operativa adottata sulla scena sia stata la migliore opzione tecnica possibile nel contesto dato.
    * **Computer**: Ai fini dello standard, si configura come computer qualsiasi dispositivo standalone dotato di capacità di ricezione, elaborazione e memorizzazione di flussi di dati, indipendentemente dallo stato di connessione alla rete al momento dell'intervento.
* **Protocollo Operativo del DEFR sulla Scena**:
    * Identificazione e censimento di tutti i computer, periferiche e dispositivi connessi, inclusi i supporti nascosti o miniaturizzati (**miniSD, microchip RFID, dispositivi embedded**) o dislocati in aree remote (**Cloud o SAN remoti**).
    * Mappatura e identificazione sistematica di tutte le interfacce di rete hardware con cui il dispositivo ha scambiato dati storicamente, anche se disconnesse al momento dell'ispezione.
    * *Se il dispositivo è ACCESO*: Obbligo di documentazione fotografica/video immediata di tutto ciò che è visibile a schermo prima di implementare qualsiasi interazione software.
    * Predisposizione di verbali scritti dettagliati, associati a fotografie e riprese video macro dei seriali e dello stato logico dei sistemi.
    * Repertazione sistematica di tutti i cavi di alimentazione e dei caricatori proprietari dei dispositivi alimentati a batteria (per prevenire lo spegnimento improvviso e il blocco crittografico del terminale in fase di trasporto).
    * Impiego obbligatorio di rilevatori di segnali wireless per intercettare e schermare trasmissioni radio non visibili (es. posizionamento immediato dei device all'interno di **Gabbie di Faraday** o attivazione hardware della modalità aereo per impedire comandi di *remote wipe* o cancellazione a distanza).
