> Prerequisiti per l'accesso abusivo 
> 1. Presenza di  misure di sicurezza (ius escludenti, principio labile), in contesto penale è sufficiente la loro presenza, in contesto civile è necessario che queste misure siano adeguate.

# Segreti commerciali
## Segretezza vs Riservatezza
Nel contesto di rapporti B2B/rapporti col cliente/dipendenti vi è la possibilità di concordare NDA, obblighi di riservatezza unilaterali o reciproci che impongono il divieto di diffusione di una certa informazione. Tipicamente vengono previste allo stesso tempo obblighi e penali.

## Codice della proprietà industriale
Canonizza i principi sulla preservazione sul patrimonio immateriale di un azienda (tecniche operative/processi aziendali/competenze di settore/...).
Per informazioni segrete commerciali (fornitori e accordi con questi/pricing models/margini/clientela/...) si intendono in generale informazioni che hanno rilevanza economica.

### Art. 98 CPI - Ogetto della tutela
A livello civilistico per rientrare nella tutela dei segreti un'informazione deve rispettare tre requisiti:
1. **Segretezza**: Le informazioni, nella loro precisa configurazione o in configurazioni dei loro elementi, non sono note o facilmente accessibili ad esperti e operatori di settore.
2. **Valore economico**: Le informazioni in quanto segrete hanno valore economico: il mantenimento della loro segretezza conferisce un vantaggio economico a chi le  detiene o viceversa la loro divulgazione causa perdite economiche
3. **Misure di sicurezza adeguate**: le informazioni devono essere sottoposte a misure di sicurezza ragionevolmente adeguate, non solo dal punto di vista puramente tecnico ma anche organizzativo ie. prevedere corsi di formazione sulle misure a tuela del segreto.

L'adeguatezza delle misure di sicurezza deve essere valutata in considerrazione del contesto di applicazione, delle misre state-of-the-art e del valore del segreto che proteggono. è inoltre necessario che queste misure siano controllate e verificate periodicamente.

> In contesti in cui per qualche motivo (principalmente economico) non è possibile istituire misure di sicurezza in un certo ambito (ie. protezione fisica) è necessario che si sopperisca in maniera alterativa.

In contesto civilistico la misura di intervento giuridico è il **ricorso**: va presentato al giudice dimostrando (o dando forti elementi indiziari) la concorrenza sleale da parte di qualcuno soggetta all'utilizzo di segreti industriali. 
È necessario dimostrare che vi sia un'effettiva violazione del segreto e che le misure a sua protezione siano sufficienti.
Durante la fase preliminare del ricorso è sufficiente il *fumus boni iuris*: un insieme di elementi che supportino sufficientemente le argomentazioni di chi lo ha esposto.
Va anche considerata la presenza di *periculum in mora*: il mancato intervento causa un continuo/peggioramento dei danni a chi ha sporto ricorso.

A questo punto viene nominato un CTU e costituito un accesso *inaudita altera  parte*: il CTU effettua la verifica senza preventiva notifica sotto ordine del giudice e il soggetto della verifica non può rifiutarsi. Questo processo si chiama *descrizione*, termine che risale ad accertamenti di questo tipo in contesti puramente aziendali.

Se si dovesse risconntrare l'effettiva presenza delle informazioni segrete durante la descrizione si può procedere con il sequestro, l'applicazione di misure correttive specifiche (ritiro dal mercato e distruzione dei materiali derivati in modo abusivo) e la richiesta di risarcimento del danno. 

Nel caso del software, le procedure di descrizione, ispezione e sequestro seguono la Legge sul Diritto d'Autore anziché il CPI, e il CTU ha il compito di acquisire copie forensi non solo dell'eseguibile, ma anche di sorgenti, log e repository, documentando rigorosamente la catena di custodia.

### Quadro sanzionatorio penale
In ambito penale la norma cardine è l'Art. 623 c.p. in materia alla rivelazione di segreti scientifici o industriali. A questa si affiancano i reati informatici strumentali, quali l'accesso abusivo a sistema informatico (Art. 615-ter c.p.), tipico vettore di esfiltrazione. Nel caso specifico del software, rileva anche l'Art. 171-bis L.d.A. per la duplicazione abusiva a fini di profitto.

### Diritto d'autore
La legge a protezione del diritto d'autore nasce a tutela di opere proprietarie che non rimangono segrete (ie. opere d'arte, brani musicali, fotografie, ...).
In contesto informatico il codice sorgente può essere protetto cumulativamente come *opera dell'ingegno* e, se mantenuto segreto, come *segreto commerciale*.

Rientrano sotto questa protezione:  codice sorgente, eseguibili, documentazione progettuale.
Vengono protetti:
- Programmi sorgente
- Programmi oggetto
- Materiale preparatorio (diagrammi di flusso, schemi logici, documentazione di progetto)

Non vengono protetti sotto il diritto d'autore:
- Idee, principi ed algoritmi sottostanti
- Funzionalità del programma
- Linguaggi di programmazione e formati dei file
- Interfacce puramente funzionali
- Elementi obbligati dal contesto tecnico (*scènes à faire*), legati indissolubilmente all'idea stessa (*merger doctrine*) o appartenenti al pubblico dominio.

Il plagio del software può essere "letterale" (copia speculare del codice) o "non letterale" (copia della struttura, sequenza e organizzazione). Una tipica tecnica di sviluppo per prevenire accuse di plagio non letterale è il *Clean Room Design*, che prevede l'uso di due team di sviluppo rigorosamente isolati (uno analizza l'originale redigendo le specifiche, il secondo programma senza mai vedere il codice sorgente di partenza).

Non è possibile brevettare software come stand-alone ma è possibile brevettarlo in combinazione con componenti hardware.

All'interno del diritto d'autore convivono due aspetti di tutela:
- **Diritti Morali**: paternità, integrità, pubblicazione. Considerati inalienabili indipendentemente da accordi contrattuali.
- **Diritti Patrimoniali**: riproduzione, esecuzione, distribuzione, elaborazione. A differenza di quelli morali, sono cedibili.

La tutela del diritto d'autore ha scadenza di 70 anni, vengono tutelate anche rielaborazioni successive.
La tutela sul diritto d'autore non prevede la segretezza del codice ma, ove dimostrabile, protegge il lavoro originale dell'autore.

### Quantificazione del danno
Tipologie di danno risarcibili:
- Danno emergente: spese di indagine, sicurezza, limitazione del danno
- Lucro cessante: mancato guadagno attibuibile al periodo e all'oggetto
- Royalty equivalente: 'prezzo del consenso' che il violatore avrebbe dovuto pagare (licenze)
- Danni morali

#### Contesto penale
In diritto d'autore è applicabile anche in contesto penale in situazioni in cui a violazione viene utilizzata per generare profitto. 

### Titolarità del software
In contensti di lavoro subordinato i diritti patrimoniali sul software spettano per legge al datore di lavoro (Art. 12-bis L.d.A.). Al contrario il diritto di paternità spetta sempre all'autore e non è cedibile in nessun contesto. Nel caso di software su commissione, la cessione dei diritti patrimoniali richiede necessariamente la forma scritta ad probationem. In contesti open source, i diritti possono essere in comunione tra più contributori.

### Brevetto
Il brevetto tutela un informazione progettuale reso di pubblico dominio, proteggendo per un periodo limitato il *lavoro d'ingegno* per 7 anni (so come stai facendo qualcosa ma non posso comunque replicarlo). 
Considerando la crescente integrazioen del software in componenti meccaniche si è ampliata la protezione del brevetto anche ad esso ma eclusivamente come componente associato ad hardware. In Europa (tramite l'Ufficio Brevetti Europeo - EPO), le *Computer-Implemented Inventions* (CII) sono brevettabili solo se presentano un contributo tecnico ulteriore (*further technical effect*) che vada oltre la normale interazione fisica col computer.

### Modalità d'analisi (Software Forensics)
Le metodologie d'esame si dividono in:
- **Analisi Statica**: esame del codice senza esecuzione (sorgenti, script, metadati).
- **Analisi Dinamica**: osservazione a runtime (API, accessi al file system, traffico di rete).
- **Analisi Comparativa e Semantica**: confronto sistematico per individuare logiche identiche dissimulate.

Si fa un analisi delle risorse confrontando marcatori su più livelli:
- Livello lessicale: string matching, commenti, nomi di variabile, costanti.
- Livello strutturale: confronto di alberi sintattici (AST diff) o Control Flow Graph (CFG) sui file binari.
- Storico di sviluppo (Git Forensics): analisi di commit, blame e repository, prestando attenzione a possibili manomissioni (rebase, alterazione dei timestamp).

Indizi forti di copia (le "firme involontarie") includono bug identici nella medesima posizione, errori di battitura nei commenti replicati e costanti errate preservate. Altri indicatori rilevanti provengono dalla Software Bill of Materials (SBOM) e dall'uso non dichiarato di librerie soggette a licenze Open Source. Il consulente tecnico (CT) incaricato ha il compito di descrivere le evidenze tecniche emerse e isolare il codice funzionalmente necessario, senza mai emettere autonomamente giudizi giuridici di "plagio".

## Reverse Engineering
Il reverse engineering è vietato in quanto viola il diritto d'autore e può essere consentito dalla legge unicamente se strettamente necessario per conseguire l'interoperabilità con altri sistemi. 
L'utente legittimo può tuttavia analizzare il funzionamento black-box del software per comprenderne i principi, ma senza aggirare o eludere in maniera abusiva le misure poste a tutela del programma.
