# Digital Forensics nel Processo Civile

A differenza del processo penale, il processo civile è si occupa controversie tra soggetti privati (persone fisiche o giuridiche) per la tutela di un diritto o il risarcimento di un danno, e non a punire un reato. 
Gli ambiti tipici includono concorrenza sleale, violazione di patti di non concorrenza, furto di proprietà intellettuale, diffamazione, o inadempienze nei contratti IT.
L'obiettivo della Digital Forensics in questo contesto è fornire evidenze oggettive, verificabili e inalterabili a supporto delle pretese economiche o legali delle parti.

## Princìpi Fondamentali della Prova

### L'Onere della Prova
Nel processo civile vige il principio cardine secondo cui *"Chi vuol far valere un diritto in giudizio deve provare i fatti che ne costituiscono il fondamento"*. Questo significa che spetta all'Attore portare al Giudice prove informatiche tecnicamente inattaccabili.

### La Prova di Resistenza
Poiché il dato digitale è intrinsecamente volatile e facilmente manipolabile, l'autenticità della prova va accertata olte ogni ragionevole dubbio, altrimenti non ha alcun valore probatorio se disconosciuta formalmente dalla controparte.
La prova deve superare una "prova di resistenza" dimostrando la sua inalterabilità tramite firme digitali, marche temporali, estrazione degli header originali, calcolo dell'hash e catena di custodia certificata.

## Attori e Ausiliari Tecnici

- **Attore e Convenuto**: Le parti private del processo. L'Attore avvia la causa citando in giudizio la controparte, il Convenuto si costituisce per difendersi.
- **Giudice**: Dirige il processo, valuta le memorie, ammette le prove e prende la decisione finale.
- **CTU (Consulente Tecnico d'Ufficio)**: Previsto dall'Art. 61 c.p.c., è l'esperto nominato dal Giudice quando per la decisione sono necessarie specifiche competenze tecniche. A differenza del perito in ambito penale, non è un autonomo mezzo di prova, ma uno strumento per integrare le conoscenze del Giudice. Risponde rigorosamente ed esclusivamente ai "quesiti" formulati dal magistrato e deve operare in totale imparzialità.
- **CTP (Consulenti Tecnici di Parte)**: Nominati da Attore e Convenuto. Affiancano il CTU in tutte le operazioni, tutelano gli interessi dei propri clienti, verificano la correttezza metodologica delle estrazioni e presentano osservazioni tecniche vincolanti alla bozza di relazione del CTU.

## Strumenti Pre-Contenziosi e d'Urgenza

Spesso nel contenzioso civile la prova digitale rischia di essere distrutta o alterata prima che la causa formale di merito entri nel vivo. Per questo l'ordinamento prevede strumenti specifici:
- **Accertamento Tecnico Preventivo**: Viene nominato un CTU per "congelare" la situazione, estrarre i dati e quantificare i danni prim'ancora di avviare il processo vero e proprio (usato tipicamente nei contenziosi per software malfunzionante). L'esito oggettivo dell'ATP porta molto spesso le parti a chiudere la disputa con una transazione stragiudiziale bonaria.
- **Descrizione**: Una sorta di "blitz forense". Se un'azienda sospetta il furto di segreti commerciali da parte di un concorrente, chiede al Giudice l'autorizzazione a ispezionarne i sistemi aziendali *inaudita altera parte* (senza alcun preavviso). Un Ufficiale Giudiziario, affiancato dal CTU, si presenta a sorpresa per eseguire l'acquisizione forense mirata dei dati.
- **Ricorso d'Urgenza**: Azione intrapresa quando vi è un *periculum in mora* (pericolo grave e imminente), ad esempio se c'è il sospetto fondato che un dipendente dimissionario stia attualmente formattando i database aziendali per portarli altrove.

## Le Fasi dell'Accertamento Tecnico (CTU)

Quando il Giudice dispone una CTU informatica, il percorso si articola in fasi procedurali garantiste del pieno contraddittorio:

### 1. Nomina e Formulazione del Quesito
Il Giudice nomina il CTU, il quale presta giuramento di rito. Nella stessa udienza, il Giudice definisce formalmente il "Quesito" (es. "Accerti il CTU se dai computer in uso al convenuto vi sia stata esfiltrazione di dati verso memorie USB"). Le parti, di conseguenza, nominano i propri CTP.

### 2. Inizio Operazioni Peritali (IOP) e Acquisizione
È il momento in cui avvengono le acquisizioni delle copie forensi. A differenza del penale (spesso caratterizzato da sequestri a sorpresa), nel civile tutto deve svolgersi in contraddittorio palese. Il CTU convoca ufficialmente i CTP e l'acquisizione dei dispositivi avviene alla presenza di tutti. Si adottano *write-blocker*, si isolano le evidenze e si calcolano gli algoritmi di hash per cristallizzare la prova e impedire contestazioni future sull'integrità del reperto.

### 3. Analisi, Espunzione e Limiti della Privacy (GDPR)
Questa fase segna una demarcazione fortissima rispetto al penale. Nel civile **non sono ammesse** ispezioni massive e indiscriminate sui dispositivi. 
Il CTU è tenuto ad applicare rigidi filtri di pertinenza e a **espungere** tutte le email personali, i dati medici o i file irrilevanti per il contenzioso. 
Tutto ciò nel rispetto assoluto del GDPR. Violazioni della privacy in questa fase rendono nulla l'intera prova ed espongono l'esperto a pesanti sanzioni.

### 4. Bozza e Osservazioni
Conclusa l'analisi tecnica, il CTU redige una bozza preliminare di perizia e la invia esclusivamente ai CTP. Questi ultimi hanno un termine stabilito per rispondere con le loro "Osservazioni Tecniche", mediante le quali possono attaccare le metodologie usate dal CTU, evidenziare lacune o proporre interpretazioni alternative a favore dei propri assistiti.

### 5. Relazione Finale
Il CTU deposita in Tribunale la relazione definitiva, che deve obbligatoriamente contenere e contro-dedurre in modo puntuale alle osservazioni sollevate dai CTP. Questa relazione diventa un pilastro istruttorio decisivo su cui il Giudice (che pur essendo *peritus peritorum* non possiede le competenze informatiche) fonderà la propria sentenza.
