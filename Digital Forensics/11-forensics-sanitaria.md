# Digital Forensics Sanitaria

> **Extra.** L'ecosistema sanitario è caratterizzato da dati "particolari" protetti in modo rafforzato (Art. 9 GDPR) e da sistemi critici real-time la cui analisi non deve mai compromettere la continuità delle cure.

La Digital Forensics Sanitaria è la branca che applica le metodologie della computer forensics ai sistemi informativi del settore salute. L'obiettivo primario è la ricostruzione rigorosa di eventi clinici, amministrativi o tecnici, garantendo l'ammissibilità probatoria delle evidenze dinanzi all'autorità giudiziaria.

> **NB.** L'attività forense in questo ambito si muove sull'intersezione di molteplici normative, in primis il GDPR, il D.Lgs. 101/2018, il Codice Deontologico Medico e il Regolamento MDR sui dispositivi medici.

Per le indagini su reati gravi, dove vige un conflitto tra il segreto professionale medico e l'obbligo di referto, l'acquisizione dei sistemi clinici necessita sempre di un decreto motivato dall'autorità giudiziaria e del rispetto del principio di proporzionalità.

## Aree di indagine e sistemi critici
I bersagli dell'indagine forense spaziano dai complessi software gestionali dell'ente ospedaliero ai singoli dispositivi a stretto contatto con il paziente.

### Cartella Clinica Elettronica (CCE)
La CCE costituisce un atto pubblico di fede privilegiata: l'alterazione fraudolenta di questi documenti configura il reato di falso in atto pubblico. 
Qualsiasi analisi presuppone:
- **Controllo delle Firme**: l'obbligo normativo di utilizzare la Firma Elettronica Qualificata (FEQ) o Avanzata (FEA) per garantire l'immodificabilità del referto dopo la sottoscrizione.
- **Analisi dell'Audit Trail**: l'investigazione sistematica dei log di sistema per tracciare rigorosamente ogni accesso, modifica o cancellazione.
- **Verifica delle Inconsistenze Temporali**: il confronto tra i timestamp dell'applicazione, del DBMS e del sistema operativo per rilevare modifiche retroattive non visibili dalla normale interfaccia utente.

### Dispositivi Medici (IoMT) e Diagnostica
L'estrazione di dati da dispositivi loMT (es. pacemaker, pompe insuliniche, robot chirurgici) mira a verificare se un evento avverso sia scaturito da un malfunzionamento, una manomissione o un errore clinico.
- **Formato DICOM**: i ricchi metadati presenti nelle immagini diagnostiche consentono di svelare steganografie forensi, manipolazioni di radiografie o discordanze sulle date dell'esame.
- **Software Forensics**: indagini basate sull'analisi di bug o sulla revisione dei log di input/output dei sensori per verificare i comandi impartiti a macchinari autonomi.

> L'accesso fisico per l'estrazione di dati da dispositivi medici impiantati nel paziente richiede un'autorizzazione giudiziaria specifica e la supervisione di un medico specialista.

## Reati e Responsabilità
La ricostruzione forense affianca e integra la classica perizia medico-legale.

### Controlli difensivi e Cyber-Responsabilità
Accertamenti eseguiti per individuare condotte illecite dei professionisti o negligenze della struttura sanitaria stessa.
- **Accesso abusivo a sistema informatico**: la prassi illegittima di consultare i dati clinici di colleghi o figure pubbliche senza un'effettiva necessità medica (Art. 615-ter c.p.). L'illiceità si dimostra incrociando i log di accesso con i registri degli appuntamenti.
- **Cyber-Responsabilità dell'ente**: in scenari di attacchi ransomware o data breach, viene valutata la colpa organizzativa (D.Lgs. 231/2001) e la presenza documentata di adeguati piani di Business Continuity o misure di sicurezza tecniche a tutela del paziente.
- **Furto di identità medica**: la sottrazione di un template biometrico (usato per firmare o accedere a medicinali) costituisce un danno permanente e irreversibile in quanto la biometria, a differenza di una password, non può essere "reimpostata".

## Esonero dalle procedure standard
A causa delle specificità sanitarie, il normale modus operandi delle indagini forensi subisce delle radicali variazioni.

### Territorio grigio e Incident Response
La necessità di non spegnere macchine salvavita obbliga gli investigatori in ambito ospedaliero a far ricorso esclusivo alla "Live Forensics". Si procede all'acquisizione dei log e della memoria volatile in tempo reale, aggirando le classiche procedure di contenimento per rispettare il vincolo etico primario delle cure vitali.

> In contesti affini, strumenti personali non prettamente sanitari (es. fitness tracker e smartwatch) producono tracce utilissime in ambito di medicina legale digitale per stimare con accuratezza l'esatto momento del decesso o la caduta dei parametri vitali.
