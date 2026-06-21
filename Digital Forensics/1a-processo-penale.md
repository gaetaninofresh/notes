# Digital Forensics nel Processo Penale

L'innesco dell'intero procedimento è la **notitia criminis** (denuncia, querela, referto ecc.), che costituisce il presupposto logico e giuridico per avviare le indagini. In informatica forense, la notitia criminis stessa contiene spesso i primi elementi tecnici, come log di sistema e segnalazioni dei provider.
Le indagini digitali devono rispettare rigorosamente procedure codificate, pena l'inutilizzabilità o la nullità della prova.

## Le Quattro Fasi del Procedimento Penale

Dal momento in cui l'autorità giudiziaria riceve la notizia di reato fino alla sentenza definitiva, il procedimento si articola in quattro fasi principali. È importante distinguere tra "procedimento" (sempre presente e contenitore di tutte le fasi) e "processo" (fase eventuale che si apre solo con l'esercizio dell'azione penale).

### Fase I — Indagini Preliminari
Hanno la funzione di raccogliere elementi di prova sufficienti a sostenere (o escludere) l'imputazione. 
- Il **PM** dirige l'attività della PG e nomina i Consulenti Tecnici (CTPM) per valutare se procedere o archiviare.
- Se il PM richiede l'archiviazione, la parte offesa può opporsi (spesso grazie all'ausilio di un proprio CTP che evidenzia lacune investigative).
- Se gli elementi raccolti sono sufficienti, il PM esercita l'azione penale chiedendo il rinvio a giudizio.

### Fase II — Udienza Preliminare (Fase eventuale)
Funziona da filtro processuale tra indagini e dibattimento per i reati di competenza del Tribunale collegiale.
- Il Giudice dell'Udienza Preliminare (GUP) valuta, in un contraddittorio prevalentemente cartolare sugli atti d'indagine, se rinviare a giudizio l'imputato o pronunciare una sentenza di non luogo a procedere.
- In questa fase, l'imputato può chiedere riti alternativi (es. giudizio abbreviato o patteggiamento). Per i reati minori (citazione diretta a giudizio), questa fase non è prevista.

### Fase III — Atti Preliminari al Dibattimento
Costituisce il segmento di atti preparatori tra il decreto che dispone il giudizio e l'apertura effettiva del dibattimento.
- Si formalizzano le ultime regolarità e la costituzione in giudizio delle parti (parte civile, responsabile civile).
- **Istanze probatorie:** Le parti depositano e formulano al Giudice le proprie richieste di prova, tra cui liste dei testimoni, la produzione documentale o le formali richieste di perizia tecnica. 

### Fase IV — Il Dibattimento e la Decisione
Il dibattimento è il cuore del processo: qui ha luogo la formazione delle prove in pieno contraddittorio tra le parti.
- **Istruttoria dibattimentale:** I mezzi di prova (come l'esame di testimoni, periti e consulenti) vengono assunti davanti al Giudice in udienza.
- **Poteri del Giudice (Art. 507 c.p.p.):** Il Giudice ha il potere di disporre l'assunzione di ulteriori prove d'ufficio se risultano assolutamente necessarie per completare l'accertamento.
- **Sentenza:** Conclusa l'istruttoria e dopo la requisitoria del PM e le difese degli avvocati, il Giudice si ritira in camera di consiglio e pronuncia la sentenza, corredata da una motivazione tecnica e giuridica sui punti controversi. Se il Giudice dovesse discostarsi dal parere tecnico del Perito o del CT, è tenuto a motivare adeguatamente la sua scelta.

## Attori e Ausiliari Tecnici
- **PG (Polizia Giudiziaria)**: Interviene per prima sulla scena (es. Polizia Postale). Il suo compito primario è il "triage" forense: isolare il dispositivo, documentare fotograficamente l'ambiente, e "congelare" gli elementi. 
- **PM (Pubblico Ministero)**: Dominus delle indagini. Coordina la PG e valuta gli elementi raccolti per decidere sull'azione penale.
- **CTPM (Consulente Tecnico del PM)**: Nominato dal PM per affiancare la polizia. Lavora per l'accusa con l'obbligo della verità scientifica.
- **Difesa e CTP (Consulente Tecnico di Parte)**: Controlla e confuta le tesi dell'accusa. Verifica il rispetto della legge sulle metodologie di acquisizione e può assistere alle operazioni del CTPM, presentare memorie tecniche e formulare riserve se nota difetti nella catena di custodia.
- **Giudice e Perito**: Il Perito è un esperto super-partes che interviene solitamente in fase dibattimentale per dipanare questioni tecniche complesse. Risponde ai quesiti del Giudice e il suo elaborato fa piena prova.

## Tipologie di Accertamento Tecnico
La distinzione tra ripetibile e irripetibile è il cuore della procedura penale informatica:
- **Accertamenti Ripetibili**: Sono eseguiti sulle copie forensi. Poiché l'originale non viene alterato e l'operazione può essere rifatta identica mille volte, il PM procede senza preavvisare la difesa.
- **Accertamenti Irripetibili**: Interventi che alterano il dato o l'hardware. Impongono l'obbligo di preavviso a indagati e parte offesa.
- **Incidente Probatorio**: Una "parentesi" delle indagini preliminari. Se c'è rischio che una prova si disperda o modifichi prima del processo, viene assunta subito davanti al Giudice (GIP) con tutte le garanzie del dibattimento.

## Mezzi di Ricerca della Prova Digitale
- **Ispezione**: Attività descrittiva su un sistema per rilevarne le tracce e gli effetti materiali. Spesso fatta su sistemi "live".
- **Perquisizione**: Ricerca mirata di dati o dispositivi specifici. È un atto a sorpresa.
- **Sequestro**: Acquisizione coatta del dispositivo o del dato. Nel digitale, sempre più spesso si sequestra il "dato" (es. tramite cloud extraction o acquisizione presso il provider) restituendo l'hardware per minimizzare il danno all'azienda.
- **Intercettazioni Telematiche**: Captazione occulta del flusso dati in transito (es. trojan di stato, sniffing di rete).

## La Relazione Tecnica
Il risultato di qualsiasi accertamento deve confluire in una relazione tecnica.
- Deve essere **comprensibile**: Il Giudice è "peritus peritorum" ma non è un informatico.
- Deve indicare la **metodologia**: Strumenti usati, versioni software, algoritmi di hashing.
- Deve distinguere tra **fatto tecnico** ("il file X è stato creato il giorno Y") e **valutazione** ("è probabile che l'utente sapesse").

## In Dibattimento
L'esame incrociato (cross-examination) è il momento della verità. 
Il CTP cercherà di invalidare la prova attaccando la metodologia, la catena di custodia o le conclusioni. 
Un esperto di parte ben preparato può far emergere errori o imprecisioni, rendendo la prova inutilizzabile.
