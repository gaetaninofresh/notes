# Analisi Avanzata: Sicurezza delle Architetture Data-Intensive e AI

Questo documento fornisce una disamina dettagliata dei principali framework e modelli architetturali affrontati nel corso "Sicurezza delle Architetture Orientate ai Servizi / Architetture Data Intensive", con particolare riferimento alla messa in sicurezza delle pipeline di Machine Learning e dei sistemi basati su agenti LLM.

---

## 1. Asset Model e Metodologia STRIDE / STRIDE-AI

[Image of Data Flow Diagram for Machine Learning pipeline threat modeling]

L'approccio moderno alla sicurezza delle architetture data-intensive inizia sempre con la definizione di un **Asset Model**. Prima di poter valutare le minacce, è necessario decomporre il sistema nei suoi componenti fondamentali (asset) e mapparne il flusso di dati (Data Flow Diagram).

### 1.1 Il concetto di Asset Model nelle Pipeline ML
In una pipeline di intelligenza artificiale, gli "asset" differiscono dai tradizionali sistemi software. I tipici asset includono:
* **Database di Ingestion:** Sorgenti dati grezze (es. log di sistema, immagini dicom).
* **Moduli di Preprocessing:** Sistemi che eseguono bilanciamento (es. SMOTE), normalizzazione o estrazione delle feature.
* **Training Set & Val/Test Set:** I dati strutturati utilizzati per addestrare il modello.
* **Algoritmi e Modelli (Classifier/Regressor):** Il modello matematico vero e proprio (es. Multilayer Perceptron, SVM, ResNet).
* **Output e Feedback Loop:** Le previsioni del modello e le azioni conseguenti.

### 1.2 STRIDE-AI: Mappatura delle Minacce
Una volta definito l'Asset Model, si applica la metodologia **STRIDE** (creata da Microsoft) per identificare le minacce su ciascun nodo e arco del flusso dati. Nel contesto dell'AI, il framework è stato esteso in **STRIDE-AI**:

* **S - Spoofing (Identità):** Nelle architetture AI, lo spoofing può tradursi nella falsificazione dei metadati di provenienza del dataset (es. far credere al sistema che i dati provengano da un sensore affidabile).
* **T - Tampering (Integrità):** È la categoria più critica per l'ML. Include il **Data Poisoning** (avvelenamento del training set con label alterate per degradare le performance) e la manipolazione dei parametri del modello (*Model Backdooring*).
* **R - Repudiation (Non-Ripudiabilità):** Difficoltà nel tracciare chi ha modificato un set di dati o quale componente ha originato un falso positivo.
* **I - Information Disclosure (Riservatezza):** Fughe di dati sensibili usati nel training attraverso attacchi di **Model Inversion** (ricostruire i dati di input a partire dalle predizioni) o **Model Extraction** (copiare i pesi del modello interrogandolo intensamente).
* **D - Denial of Service (Disponibilità):** Saturazione della pipeline di inferenza (es. inviando input complessi che richiedono molta computazione) o distruzione dei server di calcolo.
* **E - Elevation of Privilege (Autorizzazione):** Sfruttamento di vulnerabilità nel codice del modello (es. deserializzazione non sicura tramite librerie come Pickle) per ottenere l'esecuzione di codice in remoto (RCE).

---

## 2. Framework PASTA (Process for Attack Simulation and Threat Analysis)

Mentre STRIDE fornisce un elenco tecnico di minacce, il **PASTA** è un framework di threat modeling di tipo *Risk-Centric* strutturato in 7 fasi. È cruciale per la governance della sicurezza aziendale perché allinea i rischi tecnici agli obiettivi di business.

Nelle architetture data-intensive, PASTA permette di evitare l'implementazione di controlli di sicurezza eccessivamente costosi se il rischio aziendale è basso.

**Le 7 fasi del PASTA:**
1.  **Definizione degli obiettivi di business:** (es. garantire la disponibilità H24 di un regressor per il credit scoring).
2.  **Definizione del perimetro tecnico:** (Asset Model e topologia).
3.  **Decomposizione dell'applicazione:** (Analisi dei processi e dei flussi di dati).
4.  **Analisi delle minacce:** (Estrazione di log di intelligence sulle minacce).
5.  **Analisi delle vulnerabilità e delle debolezze:** (Identificazione dei flaw algoritmici, es. suscettibilità agli outlier).
6.  **Analisi degli scenari d'attacco:** (Simulazione di come un attaccante sfrutta le vulnerabilità identificate).
7.  **Analisi del rischio e dell'impatto:** (Quantificazione economica e decisione delle mitigazioni).

---

## 3. Framework DREAD

**DREAD** è uno strumento di valutazione quantitativa del rischio. Subentra una volta che le minacce sono state identificate (es. tramite FMEA o STRIDE) per classificarne la gravità e stabilire l'ordine di priorità per le mitigazioni (Risk Prioritization).

Il calcolo prevede l'assegnazione di un punteggio (solitamente da 1 a 10) per ciascuno dei 5 attributi:

1.  **Damage (Danno):** Quanto sarebbe grave l'impatto finanziario, reputazionale o operativo se l'attacco avesse successo?
2.  **Reproducibility (Riproducibilità):** L'attacco è facile da riprodurre ogni volta o richiede condizioni atmosferiche/di rete molto specifiche e rare?
3.  **Exploitability (Sfruttabilità):** Quante competenze o risorse tecniche sono necessarie per lanciare l'attacco? (1 = hacker governativo, 10 = script automatizzato).
4.  **Affected Users (Utenti Coinvolti):** Quanti utenti o processi aziendali subiscono gli effetti del down del sistema o della compromissione dei dati?
5.  **Discoverability (Rilevabilità):** Quanto è facile per un attaccante trovare la vulnerabilità?

**Score DREAD:** Il punteggio totale o medio determina la criticità della minaccia e guida l'allocazione del budget di sicurezza.

---

## 4. Analisi FMEA (Failure Modes and Effects Analysis) per l'AI

[Image of FMEA table for machine learning systems]

La **FMEA**, originariamente sviluppata nell'ingegneria manifatturiera e aerospaziale, è stata riadattata in queste discipline per valutare i guasti nelle pipeline ML. Analizzando l'Asset Model, la FMEA costringe i progettisti a pensare a cosa succede se ogni singolo componente "si guasta", includendo nei "guasti" sia gli errori casuali che le manipolazioni avversariali.

### Struttura della Tabella FMEA in ambito ML
Per un asset come un *Multivariate Auto-Regressor* (es. stima delle precipitazioni) o un *Isolation Forest* (rilevatore di anomalie), la tabella FMEA è così strutturata:

| Asset | Failure Mode (Modo di Guasto) | Cause (Spontanee o Avversariali) | Effetto Locale / Globale | Proprietà di Sicurezza / STRIDE | RPN (Severity x Occurrence x Detection) | Azione di Mitigazione Proposta |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Regressor** | Produzione di stime drasticamente errate | Ingestione di Outlier causata da manipolazione dei sensori (Data Poisoning) | Previsioni inaffidabili $\rightarrow$ Decisioni aziendali errate | Integrità / Tampering | 8 x 6 x 4 = **192** | Implementare un pre-filtro *Isolation Forest*; Validation checks sui range attesi. |
| **API** | Denial of Service in Inferenza | Attacco computazionale con input malevoli complessi | Saturazione delle risorse cloud, pipeline non disponibile | Disponibilità / DoS | 7 x 5 x 6 = **210** | Rate limiting, Sandboxing, Dimensionamento dinamico. |
| **Model** | Rilascio di dati sensibili del training | Model Inversion Attack (API interrogata milioni di volte) | Violazione della Privacy, sanzioni GDPR | Confidenzialità / Info Disclosure | 9 x 4 x 2 = **72** | Ridurre la precisione dell'output (es. rounding delle probabilità); Limitare le query per utente. |

**Nota chiave sull'RPN:** L'indice RPN (Risk Priority Number) va da 1 a 1000. Mitigare un rischio significa introdurre controlli architetturali che abbassano la *Severity*, diminuiscono l'*Occurrence* (rendendo l'attacco più difficile) o aumentano la *Detection* (capacità di rilevare l'attacco in corso).

---

## 5. Architettura ReAct (Reasoning + Acting) per i Sistemi Agentici

[Image of ReAct paradigm architecture for LLM agents]

Con l'avvento dei Large Language Models (LLM), le architetture sono passate da modelli puramente generativi a **Sistemi Multi-Agente**. In questi sistemi, l'LLM funge da motore cognitivo che non si limita a rispondere a testo, ma compie azioni nel mondo reale.

### Il Paradigma ReAct
Il pattern **ReAct** (introdotto da Yao et al.) è il framework dominante per la creazione di agenti autonomi. Esso struttura le chiamate al modello in un ciclo iterativo (loop) di tre fasi:
1.  **Thought (Ragionamento):** Il modello analizza il task e pianifica esplicitamente il prossimo passo da compiere all'interno di una sorta di "scratchpad" interno.
2.  **Action (Azione):** Il modello decide di invocare un tool esterno (es. una chiamata API, una web search, una query su un database SQL, un calcolo in Python).
3.  **Observation (Osservazione):** L'output generato dal tool viene reiniettato nel prompt del modello come contesto aggiornato. Il modello valuta l'osservazione e riparte con un nuovo "Thought" fino alla risoluzione del problema.

### Precauzioni di Sicurezza per Sistemi ReAct
Delegare ad un modello probabilistico la capacità di eseguire azioni solleva enormi problemi di sicurezza (come valutato dal nuovo framework **OWASP AIVSS** - AI Vulnerability Scoring System). Le strategie chiave di difesa includono:

* **Least Agency (Principio del Minimo Privilegio):** Concedere all'agente solo i tool strettamente necessari e limitare i permessi delle chiavi API usate dall'agente (es. permessi in sola lettura per l'accesso a DB).
* **Guardrails e Human-in-the-Loop (HITL):** Implementare sistemi di filtraggio input/output (Guardrails) per intercettare prompt dannosi (Prompt Injection) e richiedere esplicitamente l'approvazione umana (HITL) prima che l'agente possa eseguire azioni distruttive (es. inviare email, eliminare record, eseguire bonifici).
* **Sandboxing:** Eseguire qualsiasi codice generato dall'agente in un ambiente containerizzato e isolato dalla rete interna dell'azienda.
* **Audit Trail e Continuous Monitoring:** Mantenere log immutabili della catena *Thought-Action-Observation* per indagini forensi e implementare meccanismi per rilevare deviazioni dagli obiettivi (goal drift), inclusi kill-switch che interrompono l'agente dopo un numero eccessivo di step senza successo.
