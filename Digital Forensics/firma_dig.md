
> **Note per l'esame:**
> - 7 domande a risposta aperta (1/2 pagina).
> - Riferimenti giuridici non richiesti eccetto artt. 359/360.
> - **DOMANDA ESAME:** Differenza procedimento/processo penale $\to$ il processo è parte del procedimento.
> - **DOMANDA ESAME:** La marca temporale garantisce i file di log al momento dell'evento.
# Firme Elettroniche

La gestione dei documenti informatici si basa su un impianto normativo integrato tra l'**eIDAS** e il **CAD** (Codice dell'Amministrazione Digitale).

Ogni accertamento su firme e documenti digitali poggia su tre proprietà fondamentali:
- **Integrità**: corrispondenza esatta tra originale ed esibito, verificata tramite hashing.
- **Immodificabilità**: garanzia che l'informazione non sia stata alterata nel tempo.
- **Paternità**: connessione univoca tra il bitstream e l'identità del firmatario tramite chiavi crittografiche (basate su PKI).

## Classificazione delle Firme Elettroniche
Il regolamento eIDAS definisce una gerarchia a 3 livelli:

- **FES (Firma Elettronica Semplice)**: dati in formato elettronico associati a un documento (es. PIN, nome in email). È liberamente valutabile dal giudice e possiede valore probatorio limitato.
- **FEA (Firma Elettronica Avanzata)**: possiede 4 requisiti (art. 26 eIDAS), è connessa univocamente al firmatario, idonea a identificarlo e creata con dati sotto il suo controllo esclusivo. Ha il valore di scrittura privata.
- **FEQ (Firma Elettronica Qualificata)**: FEA + certificato qualificato + QSCD. Prevede un'identificazione fisica al momento del rilascio e si appoggia a terzi come garanzia.

#### Standard italiano 
Sottospecie della FEQ basata su infrastruttura PKI nazionale e crittografia asimmetrica (RSA/ECDSA). La firma si applica sull'hash del documento. 
Gode dell'**inversione dell'onere della prova**: per disconoscerla, il titolare deve allegare fatti specifici e dimostrare l'uso abusivo del dispositivo.

### Formati di Firma
- **PAdES (PDF)**: firma incorporata nel PDF, ideale per contratti.
- **CAdES (.p7m)**: busta CMS che ingloba documento e firma in un unico file. È lo standard de facto nel B2B e nella PA italiana.
- **XAdES (XML)**: firma su strutture XML (es. FatturaPA) per flussi interoperabili.

## Validazione e Marcatura Temporale
La validazione di una firma si articola in: 
1) Validità del certificato al time-of-signing
2) Verifica del path di certificazione
3) Verifica crittografica

> **Marcatura Temporale (Time Stamping):**  Attestazione di una Timestamp Authority (TSA) che garantisce che un documento esisteva in una certa forma ad un dato istante. La marca è legata crittograficamente all'hash.

Garantisce la possibilità di verificare la firma al momento della sottoscrizione, anche a certificato scaduto, risultando essenziale per la Long Term Validation (LTV).

## Analisi delle Copie
Il CAD distingue rigidamente tra le repliche di un documento informatico:

- **Duplicato Informatico (1:1)**: Ha la stessa identità assoluta a livello di bitstream e il medesimo valore giuridico dell'originale.
- **Copia Informatica**: contenuto identico ma sequenza binaria diversa (es. cambio formato, aggiornamento metadati di sistema). Ha la stessa efficacia dell'originale solo se attestata da un pubblico ufficiale o se non espressamente disconosciuta.
- **Copia Analogica di Doc. Informatico (Rematerializzazione)**: la stampa perde irrimediabilmente la firma crittografica. L'autenticità è recuperabile solo tramite un **contrassegno** (QR Code o glifo) che collega la copia cartacea all'originale digitale.

## Prassi e Analisi Forense
### FES ed Email
Essendo la FES liberamente valutabile, il Consulente Tecnico (CT) deve fornire criteri oggettivi di affidabilità attraverso:
- Tracciabilità dell'accesso (log, IP).
- Coerenza temporale (artefatti di sistema).
- Identificazione del soggetto (pattern comportamentali).

Per l'email (considerata documento informatico), l'analisi forense prevede l'esame sistematico di:
1. **Header RFC 5322**: per analizzare campi *Received* e anomalie degli IP (spoofing).
2. **Protocolli (SPF, DKIM, DMARC)**: il loro allineamento rappresenta la prova più forte di autenticità.
3. **Struttura MIME**: per rilevare alterazioni post-invio tramite l'analisi dei boundary.

### PDF Incremental Updates
L'architettura dei PDF firmati prevede che non si sovrascrivano i dati. Le modifiche avvengono per sezioni incrementali: la firma copre solo i byte precedenti all'apposizione, permettendo al perito di estrarre e visualizzare le versioni esatte pre-modifica tramite byte-offset.

## Conservazione Digitale
Obbligatoria per la PA e facoltativa per i privati (ruoli specifici previsti), serve a mantenere il valore probatorio nel tempo. Poiché i certificati scadono e gli algoritmi crittografici diventano obsoleti, il conservatore qualificato utilizza il chained timestamping e i formati LTA (Long Term Archival) applicati al **Pacchetto di Archiviazione (PdA)**.
