## Note per l'esame
> - **DOMANDA ESAME:** Differenza procedimento/processo penale $\to$ il procedimento penale è la fase delle indagini preliminari (dominata dal PM), il processo penale è eventuale ed è la fase che inizia solo con l'esercizio dell'azione penale (davanti al Giudice).
> - **DOMANDA ESAME:** Differenza perizia (penale) e CTU (civile) $\to$ nel penale la perizia è un mezzo di prova autonomo (ammesso solo per questioni complesse), mentre nel civile la CTU è uno strumento integrativo e ausiliario per colmare le lacune tecniche del giudice.
> - **DOMANDA ESAME:** Accertamenti ripetibili vs irripetibili $\to$ (Art. 359 vs 360 c.p.p.). I secondi modificano il reperto irreversibilmente (es. sblocco distruttivo, analisi di un sistema live) e impongono l'obbligo di avviso alla difesa per garantire il contraddittorio fin da subito.
> - **DOMANDA ESAME:** La catena di custodia e l'hash $\to$ L'acquisizione forense deve garantire l'inalterabilità tramite calcolo di impronte crittografiche (es. SHA-256) e documentazione ininterrotta di ogni passaggio.

# Digital Forensics nel Processo Penale

L'innesco dell'intero procedimento è la **notitia criminis** (denuncia, querela, ecc.). 
Le indagini digitali devono rispettare rigorosamente procedure codificate, pena l'inutilizzabilità o la nullità della prova.

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
- **Ispezione**: Attività descrittiva su un sistema per rilevarne le tracce e gli effetti materiali. Spesso si traduce nella descrizione e nell'acquisizione in copia bit-a-bit con calcolo dell'hash.
- **Perquisizione**: Atto a sorpresa per cercare il corpo del reato. La perquisizione *live* (eseguita a computer acceso) è un'operazione irripetibile e molto delicata.
- **Sequestro**: È l'atto di spossessamento del bene. In ossequio al principio di proporzionalità (specialmente per le imprese), ove possibile il sequestro si limita all'estrazione di una copia informatica, lasciando l'hardware (e la continuità aziendale) al proprietario, usando sigilli elettronici.


# Digital Forensics nel Processo Civile

L'obiettivo è risolvere liti tra privati (o aziende). A differenza del penale, non c'è una "polizia" che sequestra d'ufficio: vige il principio dell'onere della prova, secondo cui chi fa valere un diritto in giudizio deve procurare le prove per dimostrarne i fatti.

## Ausiliari Tecnici Civili e la Dialettica
- **CTU (Consulente Tecnico d'Ufficio)**: Nominato dal Giudice, offre un parere tecnico per tutto ciò che richiede competenza informatica. Non va alla ricerca di prove in modo esplorativo, ma si limita a valutare i fatti già introdotti dalle parti o a rispondere agli specifici quesiti formulati.
- **CTP (Consulente Tecnico di Parte)**: Tecnico fiduciario delle parti in lite. Assiste alle operazioni del CTU per tutelare gli interessi del cliente.
- **Il Rito del Contraddittorio**: La perizia si snoda in tre atti formali per garantire il diritto di difesa tecnico:
  1. Il CTU invia alle parti una **Bozza** della sua relazione.
  2. Entro un termine fissato, i CTP rispondono redigendo e trasmettendo le loro **Osservazioni** scritte e critiche alla bozza.
  3. Il CTU elabora la relazione definitiva in cui include la bozza, le osservazioni dei CTP, e le proprie **Repliche motivate**, depositando il tutto nel fascicolo del Tribunale.

## La Prova Digitale: Disconoscimento e Criticità
- Nel civile, le riproduzioni informatiche (es. file log, estratti di email ordinarie, screenshot di WhatsApp) formano **piena prova** dei fatti se colui contro il quale sono prodotte non ne disconosce la conformità ai fatti medesimi.
- **Il Disconoscimento**: Per essere valido non può essere generico, ma deve essere "chiaro, circostanziato ed esplicito" fin dalla prima udienza utile.
- **L'importanza del "metadato"**: In caso di disconoscimento, portare al Giudice la semplice "stampa" di una mail o un PDF è un suicidio forense. Il CTU non potrà mai asseverarne la genuinità. Occorrono i file originali, con i relativi header completi, meglio se precedentemente estratti e marcati temporalmente o "hashati" da un tecnico in fase pre-contenziosa.

## Strumenti Istruttori e Pre-contenziosi
- **Accertamento Tecnico Preventivo - ATP**: Spesso usato nei contratti IT (es. fornitura di software non funzionante o difettoso). Prima di avviare una lunga e costosa causa di merito, si chiede un CTU "preventivo" per accertare lo stato dei luoghi informatici e quantificare il danno, favorendo spesso una transazione bonaria basata sui risultati tecnici.
- **Descrizione**: Un "blitz forense". Se un'azienda sospetta che un concorrente le abbia rubato segreti industriali o codice sorgente, chiede al Giudice (senza informare la controparte) l'autorizzazione all'accesso. L'Ufficiale Giudiziario, il CTU e i legali procedono all'acquisizione forense immediata dei server del concorrente.
- **Ricorso**: Azione d'urgenza concessa quando c'è un pericolo imminente (*periculum in mora*). Esempio: un dipendente in procinto di lasciare l'azienda sta formattando dischi aziendali o esportando database clienti via cloud. Il Giudice autorizza immediatamente il blocco e la cristallizzazione forense delle utenze prima che il danno sia irreparabile.
