Termine lezioni 18/06 -  sovrapposizione con sicureza reti
**Esame**
    7 domande a risposta aperta 1/2 pagina a risposta possibilmente intelligente
    Riferimenti giuridici non richiesti eccetto 359/360
    Appello con scappato di casa
    

**DOMANDA ESAME**: differenzza procediment/processo penale -> processo è parte del procedimento
**DOMANDA ESAME**: la marca temporale garantisce i file di log al momento dell'evento

//////////////////////////////////////////////////////////////

# **Firme elettroniche**
Uno dei fondamenti normativi per le firme elettroniche è il Codice per l'Amministrazione Digitale - CAD che regola le comunicazioni tra PA e cittadini e determina delle linee guida per i privati.

Gli obiettivi fondamentali di una firma elettronica sono:
    - **Integrità**: corrispondenza tra originale ed esibito
    - **Immodificabilità**: garanzia della non alterazione dell'informazione. 
    - **Paternità**: connessione univoca tra bitstream e identità del firmatario tramite chiave crittografica.

**Firma digitale in Italia**
L'eIDAS 2.0 è lo standard europeo per la firma digitale, definisce una gerarchia a 3 livelli:
    - **FES** (semplice): Dati in formato elettronico associati ad un documento. Ha valore giuridico/probatorio limitato ed è facilmente disconoscibile.
    - **FEA** (avanzata): Ha 4 requisiti, connessa univocamente al firmatario e valida per l'identificazione, creata a partire da dati sotto coontrollo esclusivo del firmatario.
    - **FEQ** (Qualificata): FEA + certificato qualificato + QSDC. La firma digitale italiana (FED) è basato su PKI nazionale. Si appoggia a terzi come garanzia e prevede un identificazione fisica al momento del rilascio.  

Altri tipi di firma:
     - **PAdES**: firma incorporata in PDF
     - **CAdES** (.p7m): ingloba documento e firma in un unico file. Standard per B2B e PA.
     - **XAdES**: ... (Non ne ha parlato)

# Marcatura Temporale
La marcatura temporale qualificata è un attestazione di una Timestamp Authority (TSA) che attesta l'esistenza e forma di un documento in un preciso istante di tempo.
La marca è crittograficamente garantita dall'hash e gode di presunzione di accuratezza ed è opponibile a terzi.
Garantisce la possibilità di verificare la validità della firma al momento della sottoscrizione, anche dopo la scadenza del certificato. è strumento cardine per la Long Term Validation (LTV).
Viene calcolata da una terza parte in base all'hash e tempo.

# Analisi delle copie

Il CAD distingue rigidamente copie (bitstream) e repliche/duplicati. A livello giuridico una copia bitsrtream ha valore equivalente ad ogni effetto della legge.
Si pongono i problemi:
    - scadenza della chiave per la firma digitale  
    - collisioni frequenti a causa di standard precedenti insufficenti (ie. SHA1)
    - accertare l'autenticità di versioni digitali di documenti cartecei -> In questo caso va attestata la conformità da un pubblico ufficiale/notaio tramite firma digitale qualificata.

**Copie Analogiche di documenti informatici**
La copia di un documento informatico avviene tramite QR che consente l'accesso alla risorsa originale ie. gazzetta ufficiale. 

# Analisi forense delle firme
## FES
Considerando che la FES è liberamente valutabile dal giudice il CT deve determinarne l'attendibilità.
Gli elementi rilevanti possono essere:
     - Tracciabilità dell'accesso -> log, indirizzi ip, sessioni ...
     - Coerenza temporale -> analisi di artefatti di sistema.
     - Identificazione del soggetto -> elementi che possono correlare la firma al sogetto. 

# Conservazione digitale
Esistono procedure specifiche per la conservazione dei documenti digitali (se ne occcupano ruoli specifici nella PA)













