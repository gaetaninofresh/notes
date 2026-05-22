# Metodologia ASM: Ground Model e Raffinamento
Il metodo basato sulle Abstract State Machine (ASM) permette la specifica rigorosa e formale di sistemi software ad elaborazione sequenziale, parallela, non deterministica e distribuita.

La metodologia si basa su tre concetti cardine:
- **ASMs**: formalismo di base.
- **Ground Model**: specifica iniziale dei requisiti.
- **Raffinamento di modelli**: sviluppo incrementale per giungere al codice.

## Ground Model (GM)
Il Ground Model è il primo modello del sistema, matematicamente corretto e sufficientemente astratto, anche se non necessariamente completo rispetto a tutti i dettagli tecnologici. Funge da contratto tra il cliente e lo sviluppatore.

Le caratteristiche principali del GM includono:
- **Evolutivo e non ambiguo**: descrive i requisiti in modo semplice e conciso.
- **Corrispondenza 1:1**: ogni entità del modello (predicati, funzioni, regole) corrisponde a processi, relazioni o proprietà del mondo reale.
- **Flessibilità e Riusabilità**: può essere facilmente esteso o adattato a diversi domini applicativi.
- **Validabile**: comprensibile sia dagli specialisti del dominio applicativo che da quelli del dominio tecnologico.

> La definizione del GM richiede la scelta del corretto livello di astrazione. È necessario identificare agenti, stati, domini, condizioni di transizione, condizioni di errore ed effetti sulle variabili dinamiche.

## Raffinamento di Modelli
Il raffinamento è il processo iterativo tramite cui, partendo da una macchina astratta ($A$), si ottiene una macchina più concreta ($R$). 

Per applicare il raffinamento occorre definire:
- La nozione di stato raffinato per $R$.
- Gli stati di interesse e la corrispondenza tra gli stati di $A$ ed $R$ (inclusi quelli iniziali e finali).
- La corrispondenza tra segmenti di computazione (passi) in $A$ e i relativi passi in $R$.
- Le locazioni di interesse e una relazione di equivalenza ($\equiv$) tra i dati astratti e quelli raffinati.

### Correttezza del Raffinamento
Una macchina $R$ è un corretto raffinamento di $A$ se e solo se ogni esecuzione (*run*) di $R$ simula un'esecuzione di $A$ procedendo tra stati corrispondenti equivalenti.
- Le sequenze di stati corrispondenti devono essere **minimali** (nessuno stato intermedio equivalente all'interno della stessa sequenza).

> **Stuttering Refinement**: Variante semplificata che supporta controlli automatici. Richiede che la *run* raffinata sia scomponibile in segmenti in cui tutti gli stati intermedi sono equivalenti allo stesso singolo stato della *run* astratta corrispondente.

### Tipi di Raffinamento
Classifichiamo il raffinamento in due tipologie:
- **Orizzontale (External change)**: Sviluppo per estensione conservativa, si aggiungono nuovi comportamenti o risposte all'ambiente senza alterare l'astrazione di base. Supporta il principio del *design for change*.
- **Verticale**: Vengono introdotti maggiori dettagli strutturali o algoritmici su elementi già modellati. Supporta il principio del *design for reuse*.

### Meccanismi Operativi
Dal punto di vista pratico, raffinare un modello comporta:
- **Raffinamento della segnatura**: modifica o aggiunta di nuovi domini e funzioni.
- **Raffinamento procedurale**: sostituzione di macro o di regole astratte/vuote con comportamenti computazionali più complessi e concreti.

## Esempio Architetturale: SluiceGate
La presentazione illustra il processo tramite il caso di studio di una saracinesca (SluiceGate) evoluto su più livelli:
- **Livello 0 (Ground Model)**: Modellazione del solo trascorrere del tempo in relazione agli stati "totalmente aperta" o "totalmente chiusa", ignorando la logica di transizione meccanica.
- **Livello 1 (Raffinamento Verticale)**: Introduzione del dettaglio procedurale e hardware tramite funzioni sul motore, direzione e sensori di fine corsa.
- **Livello 2 (ASM Multi-Agente)**: Separazione delle responsabilità assegnando l'elaborazione logica a un agente di controllo e l'interazione pura con i pin hardware a un agente di comunicazione impulsi. Ad ogni livello viene verificata la correttezza rispetto allo strato superiore e ai requisiti utente.
