# Sicurezza delle Architetture Data-Intensive: Metodologie e Framework

## 1. L'Asset Model per STRIDE-AI 

L'Asset Model in STRIDE-AI rappresenta il primo e più importante passo metodologico: permette di identificare tutto ciò che può essere attaccato o che può fallire all'interno di un sistema AI/ML, garantendo una copertura olistica. Gli asset nell'AI non sono isolati, ma interdipendenti, e i guasti possono propagarsi lungo l'intero ciclo di vita.

STRIDE-AI definisce sei macro-categorie di asset da modellare:

* **Data Assets (Dati):** Includono i dataset grezzi, i dati di training, validazione e test, gli embeddings e le etichette di ground-truth. Esempi pratici sono le cartelle cliniche dei pazienti o le misurazioni storiche dei sensori.
* **Model Assets (Modelli):** Rappresentano il core predittivo, come le architetture dei modelli addestrati, i pesi neurali, i LLM sottoposti a fine-tuning, gli endpoint di inferenza e le API.
* **Artefact Assets (Artefatti):** Elementi di supporto che permettono il funzionamento, come il codice sorgente, i file di configurazione, i checkpoint, gli script di training e i file di metadati come le ModelCard o DataCard.
* **Process Assets (Processi):** I flussi operativi astratti, come le pipeline end-to-end, i flussi di orchestrazione e i loop di feedback per il retraining automatizzato.
* **Actor Assets (Attori):** Entità umane coinvolte nel ciclo di vita. Comprendono sviluppatori, data scientist, data engineer, team MLOps, operatori, utenti finali e auditor.
* **Tool Assets (Strumenti e Ambienti):** L'infrastruttura sottostante, come i framework di machine learning (PyTorch, TensorFlow), le librerie, i cluster Kubernetes, i servizi cloud e i server di tracciamento o catalogo (es. MLflow, DataHub, DVC).

## 2. Il Framework DREAD

Il framework DREAD è un modello di scoring quantitativo sviluppato da Microsoft e integrato in STRIDE-AI per la prioritizzazione dei rischi di sicurezza. Assegna a ciascuna minaccia identificata un valore numerico su una scala da 1 a 10 basandosi su cinque fattori specifici:

* **Damage (Danno potenziale):** Misura l'impatto potenziale qualora la minaccia venga realizzata.
* **Reproducibility (Riproducibilità):** Valuta la facilità con cui l'attacco può essere ripetuto in modo consistente nel tempo.
* **Exploitability (Sfruttabilità):** Misura la difficoltà tecnica, il tempo e le risorse (strumenti, competenze) necessarie per sfruttare la vulnerabilità.
* **Affected Users (Utenti/Sistemi Coinvolti):** Quantifica il volume o l'importanza degli utenti e dei sistemi impattati.
* **Discoverability (Rilevabilità):** Indica la facilità con cui la vulnerabilità può essere scoperta da un potenziale attaccante esterno.

Il punteggio di rischio complessivo viene calcolato come la media aritmetica dei cinque fattori. Un punteggio finale superiore a 7 è indice di un rischio elevato che richiede mitigazione prioritaria.

## 3. Tabella FMEA (Failure Mode and Effects Analysis)

La FMEA è una metodologia sistematica applicata a ciascun asset prima della mappatura delle minacce, utile per identificare i potenziali modi di guasto (sia accidentali che malevoli), valutarne gli effetti e le cause, e prioritizzare le azioni correttive.

Il rischio all'interno della FMEA è quantificato tramite il *Risk Priority Number* (RPN), calcolato come:
$$RPN = S \times O \times D$$

Dove i tre fattori (valutati su una scala da 1 a 10) rappresentano:
* **S (Severity):** La gravità dell'effetto del guasto sul sistema.
* **O (Occurrence):** La probabilità che la causa e la modalità di guasto si verifichino effettivamente.
* **D (Detection):** La capacità dei controlli attuali di rilevare il guasto prima che il sistema ne subisca gli effetti completi.

Di seguito viene mostrato un esempio di struttura tabellare FMEA basato su una pipeline data-intensive per la stima delle precipitazioni:

## 4. Architettura ReAct (Reasoning + Acting) per l'AI

L'architettura ReAct rappresenta il paradigma fondamentale per l'interazione dei sistemi agentici autonomi basati su LLM, dove il modello esegue un ciclo iterativo che combina il ragionamento (Reasoning) e l'esecuzione di azioni concrete (Acting).

Il loop di esecuzione si basa sulla ripetizione di tre fasi distinte fino al raggiungimento della risposta o dello stato desiderato:
1.  **Thought (Pensiero):** Il modello esplicita e documenta la logica del passo successivo da compiere.
2.  **Action (Azione):** Il sistema invoca autonomamente uno strumento esterno (es. chiamate API, web search, esecuzione di codice in una shell).
3.  **Observation (Osservazione):** Il risultato restituito dal tool esterno viene raccolto e re-iniettato nel prompt, integrandosi nella memoria dell'agente.

Ogni iterazione arricchisce il contesto dell'LLM con la storia completa. Tuttavia, l'introduzione dell'uso di tool e l'ampliamento della memoria autonoma espandono notevolmente la superficie d'attacco, esponendo il sistema a rischi quali Goal Hijacking, Tool Misuse e Memory Poisoning.


## 5. La Matrice del Rischio (Risk Matrix)

La Matrice del Rischio è una griglia bidimensionale utilizzata per mappare visivamente i rischi in ambito di governance aziendale, mettendo in relazione la probabilità e l'impatto:

$$\text{Rischio} = \text{Probabilità (Likelihood)} \times \text{Impatto (Severity)}$$

Mentre DREAD ed FMEA generano punteggi numerici per i tecnici, la matrice fornisce una sintesi visiva immediata (espressa in livelli Low, Medium, High, Critical) ideale per la comunicazione verso i board aziendali e per alimentare i registri dei rischi richiesti da normative come la NIS2, l'ISO/IEC 27001 e l'AI Act.

### Esempio di Matrice del Rischio (3x3)
| Probabilità \ Impatto | Basso (Low) | Medio (Medium) | Alto (High) |
| :--- | :--- | :--- | :--- |
| **Alta (High)** | Medio | Alto | Critico |
| **Media (Medium)** | Basso | Medio | Alto |
| **Bassa (Low)** | Basso | Basso | Medio |

*Esempi di mappatura in contesti LLM Cloud:*
* *Indirect Prompt Injection via RAG:* DREAD medio 8.8 $\rightarrow$ Rischio **Critico**.
* *Data Poisoning:* DREAD medio 7.2 $\rightarrow$ Rischio **Alto**.

## 6. Guardrailing degli LLM: Architetture e Tipologie

Nel moderno panorama della sicurezza, i guardrail rappresentano strati di sicurezza post-training progettati per filtrare input e output di un LLM al fine di prevenirne utilizzi dannosi, agendo a runtime senza richiedere un riaddestramento completo.

La formula concettuale che governa un guardrail basato su classificazione è:
$$\text{Guardrail}(x) = \begin{cases} \text{ALLOW} & \text{se } f_\theta(x) < \tau \\ \text{BLOCK/REWORD} & \text{altrimenti} \end{cases}$$
dove $f_\theta$ rappresenta un classificatore di sicurezza (spesso un LLM di piccole dimensioni) e $\tau$ è la soglia di rischio tollerabile impostata.

### Tipologie Principali di Guardrail
Operano tipicamente come un middleware di sicurezza:
* **Input Guardrails:** Analizzano il prompt dell'utente prima che raggiunga l'LLM principale (es. tramite *Llama-Guard-3*) per bloccare tentativi di jailbreak o prompt injection.
* **Output Guardrails:** Ispezionano la risposta generata dall'LLM prima di mostrarla, bloccando esfiltrazioni di PII/PHI o allucinazioni.
* **Action Guardrails / Tool Permissioning:** Cruciali nei sistemi agentici; prima di eseguire un tool, valutano l'azione per verificare permessi ed evitare comandi distruttivi.
* **Contextual Memory Check:** Analizza lo stato interno dell'agente per intercettare attacchi di poisoning della memoria.
* **Constitutional Guardrails:** Applicano policy deterministiche scritte a livello infrastrutturale (es. via *Open Policy Agent*) che limitano i privilegi indipendentemente dal modello probabilistico.

### Limitazioni e Mitigazioni
I guardrail LLM presentano limitazioni quali l'introduzione di latenza (circa +5-15% sul loop), un tasso di Falsi Positivi su task complessi, e una fragilità residua verso attacchi di ottimizzazione avanzati come il *Greedy Coordinate Gradient (GCG)*. Pertanto, la sicurezza *by-design* richiede di affiancare ai guardrail un sandboxing rigoroso e controlli *Human-in-the-Loop*.


## 7. Categorizzazione delle Minacce: Framework ENISA e STRIDE-AI

Nelle architetture data-intensive, la categorizzazione delle minacce non si affida a un unico modello, ma si articola attraverso framework complementari che analizzano il rischio da prospettive diverse.

### 7.1 La Tassonomia ENISA (Approccio per Origine e Ciclo di Vita)
L'agenzia europea ENISA adotta un approccio orientato al ciclo di vita dell'AI, suddividendo le minacce in quattro macro-categorie fondamentali basate sulla natura dell'azione:
* **Nefarious Activity/Abuse (Attività Nefaste/Abusi):** Attacchi intenzionali e mirati al sistema (es. data poisoning, label flipping, evasion attacks, manipolazione tramite iniezioni).
* **Eavesdropping/Interception/Hijacking:** Minacce legate alla cattura non autorizzata di dati in transito o estrazione del modello (Model Inversion/Extraction).
* **Unintentional Damage / Failures / Malfunctions:** Difetti software, errori umani di configurazione o concept drift accidentale.
* **Physical Attacks & Outages:** Danni diretti all'hardware o disservizi fisici ai server/nodi edge.

### 7.2 Il Framework STRIDE-AI (Approccio per Proprietà di Sicurezza Violata)
Mentre ENISA descrive origine e contesto, STRIDE-AI categorizza "come" l'attacco impatta tecnicamente il sistema, mappando 6 macro-tipi di minaccia sull'esagono esteso della sicurezza ($CIA^3-R$):
* **Spoofing (Sostituzione di persona/entità):** Viola la proprietà di **Autenticità (Authenticity)**.
* **Tampering (Manomissione):** Modifica non autorizzata (es. alterazione dei pesi). Viola l'**Integrità (Integrity)**.
* **Repudiation (Ripudio):** Negazione di azioni compiute (es. manipolazione dei log). Viola il **Non-ripudio / Accountability (Non-Repudiation)**.
* **Information Disclosure (Divulgazione):** Esposizione o furto di dati sensibili. Viola la **Riservatezza (Confidentiality)**.
* **Denial of Service (Interruzione di Servizio):** Disruption per esaurimento risorse (es. attacchi ai KV-cache). Viola la **Disponibilità (Availability)**.
* **Elevation of Privilege (Elevazione dei Privilegi):** Ottenere diritti non concessi (es. tramite excessive agency). Viola l'**Autorizzazione (Authorization)**.

### Sintesi Operativa
Questi framework lavorano in sinergia: le minacce globali rilevate da ENISA o da standard come OWASP vengono contestualizzate sui specifici asset dell'organizzazione, per poi essere classificate tecnicamente tramite STRIDE-AI all'interno della tabella FMEA, ottenendo così una valutazione dei rischi rigorosa e quantificabile.
