# Differenze tra Processo Penale e Processo Civile in Digital Forensics
## 1. Scopo dell'Indagine e Iniziativa
- **Processo Penale**: L'obiettivo è l'accertamento della verità materiale per verificare se è stato commesso un reato e punire il colpevole. L'azione penale è **obbligatoria** e viene esercitata dallo Stato tramite il Pubblico Ministero (PM) non appena vi è una *notitia criminis*.
- **Processo Civile**: L'obiettivo è dirimere una controversia patrimoniale o tutelare un diritto tra soggetti privati. Il Giudice non cerca la verità assoluta, ma decide in base alle prove fornite dalle parti. L'iniziativa è **di parte**: il processo inizia solo se un soggetto (l'Attore) decide volontariamente di citare in giudizio un altro (il Convenuto).

## 2. L'Onere della Prova
- **Processo Penale**: Spetta all'accusa (il PM) dimostrare la colpevolezza dell'imputato **"oltre ogni ragionevole dubbio"**. Se la prova digitale (es. l'attribuzione di un indirizzo IP a una persona fisica) presenta zone d'ombra o difetti di catena di custodia, il Giudice deve propendere per l'assoluzione.
- **Processo Civile**: Vige l'Art. 2697 c.c.: **"Chi vuol far valere un diritto in giudizio deve provare i fatti che ne costituiscono il fondamento"**. L'Attore deve produrre prove digitali inoppugnabili (superando la "prova di resistenza") e il Giudice decide basandosi sulla regola del "più probabile che non" (preponderanza dell'evidenza).

## 3. Gli Ausiliari Tecnici del Giudice
- **Il Perito (Penale)**: Viene nominato dal Giudice eccezionalmente, solo quando occorre svolgere indagini che richiedono specifiche competenze tecniche che esulano dal patrimonio conoscitivo del magistrato. Il suo elaborato costituisce un **mezzo di prova autonomo**.
- **Il CTU (Civile)**: Viene nominato dal Giudice quasi sistematicamente nelle cause tecnologiche. Non è un mezzo di prova, ma uno **strumento istruttorio ed estensione tecnica del Giudice**. Il CTU non può indagare liberamente, ma è strettamente vincolato al "quesito" formulato dal Giudice.

## 4. Modalità di Acquisizione e Contraddittorio
- **In Ambito Penale**: L'acquisizione della prova è spesso **coatta e a sorpresa** (ispezione, perquisizione, sequestro tramite Polizia Giudiziaria). Il contraddittorio può essere posticipato: nel caso di accertamenti irripetibili la difesa viene avvisata, ma nel caso di accertamenti ripetibili il PM procede senza avvisare la controparte.
- **In Ambito Civile**: Domina il **contraddittorio preventivo e continuo**. Le acquisizioni forensi avvengono durante l'Inizio Operazioni Peritali (IOP) indetto dal CTU, alla presenza dei CTP di entrambe le parti, che possono vigilare in tempo reale su ogni singolo click. Le uniche eccezioni a sorpresa sono gli ordini di "Descrizione" inaudita altera parte per tutelare i segreti industriali.

## 5. Limiti dell'Indagine, Pertinenza e Privacy
- **L'Indagine Penale**: Tende a essere "massiva". Se viene sequestrato il computer di un indagato, il CTPM clona l'intero hard disk e analizza tutto il contenuto alla ricerca di qualsiasi traccia pertinente al reato, con ampi poteri di ispezione.
- **L'Indagine Civile**: È dominata dal principio di stretta pertinenza e dal GDPR. Sono assolutamente vietate le *fishing expedition* (acquisizioni a strascico sperando di trovare qualcosa di illecito). Il CTU è obbligato a **espungere** (isolare e non acquisire/cancellare) tutto ciò che attiene alla sfera personale, sanitaria o estranea al mandato, pena la nullità della prova.
