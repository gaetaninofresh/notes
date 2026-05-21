
# Verifica di Proprietà Temporali

> **Domande Esame**
> - Modello ASM  da specifica 
> - Model checking con Labeling e SAT 
> - Costruzione di formule temporali

Nella verifica e specifica dei sistemi critici, l'analisi si concentra sulla dimostrazione di specifiche proprietà temporali classificate in quattro categorie principali:
- **Proprietà di raggiungibilità**
- **Proprietà di safety**
- **Proprietà di liveness**
- **Proprietà di fairness** e **assenza di deadlock**

## Raggiungibilità (Reachability)
Afferma che determinate situazioni particolari del sistema possono essere raggiunte. Queste sono le proprietà più semplici da verificare: se il tool costruisce il grafo di raggiungibilità, l'analisi richiede solo l'ispezione dei cammini.

In CTL (Computation Tree Logic) si esprime con la formula $EF \varphi$.
- Indica che esiste almeno un cammino dallo stato corrente lungo cui un qualche stato soddisfa $\varphi$.
- $\varphi$ è definita "present tense formula", una formula proposizionale senza connettivi temporali.

> **Raggiungibilità condizionata e da stati raggiungibili**
> - Se una condizione deve restringere il cammino per raggiungere lo stato, si usa il connettivo $EU$, come in $E[\varphi \ U \ \psi]$ (es. $E[n \neq 0 \ U \ \text{crit\_sec}]$).
> - Se la raggiungibilità parte da qualsiasi stato già raggiungibile, si annidano $EF$ e $AG$ ottenendo $AG(EF \ \text{init})$.

## Safety
Afferma che, sotto certe condizioni, un determinato evento negativo non accadrà mai ("cose cattive non si verificano mai").
In pratica, la safety corrisponde spesso alla "not-reachability".

In CTL si esprime con $AG \varphi$.
- Indica che ogni stato, di ogni cammino partendo dallo stato corrente, soddisfa $\varphi$.
- Si esprime equivalentemente come negazione di una proprietà di raggiungibilità: $\neg EF(\dots) \equiv AG(\dots)$.

> **Safety condizionata**: utilizza il connettivo $AU$ nella forma $A[\varphi \ U \ \psi]$, indicando che un evento $\varphi$ (quello che non deve verificarsi) non si verifica finché non si presenta $\psi$.

## Liveness
Afferma che, sotto determinate condizioni, un qualche evento auspicato alla fine si verificherà ("cose buone prima o poi si verificano").

In CTL si esprime comunemente combinando operatori:
- $AG + AF$ oppure $AG + EF$.
- Un esempio tipico è garantire che una richiesta venga sempre soddisfatta: $AG(\text{req} \rightarrow AF \ \text{sat})$.

## Assenza di Deadlock
Garantisce che il sistema non si troverà mai in una condizione di stallo (situazione in cui non è possibile compiere alcun progresso).

In CTL si esprime come:
- $AG(EX \ \text{true})$.
- Assicura che, per qualunque stato il sistema raggiunga ($AG$), esisterà sempre almeno uno stato immediatamente successore ($EX \ \text{true}$).

## Fairness
Afferma che, sotto certe condizioni, un evento accadrà un numero infinito di volte.

> Le proprietà di fairness non possono essere modellate in pura logica CTL poiché manca l'operatore specifico per "infinitely often". In strumenti come SMV, queste ipotesi di fairness vanno specificate internamente come parte integrante del modello stesso.

## Applicazioni: Garantire la CIA
Le proprietà temporali sono impiegano per validare la sicurezza dei protocolli (Confidentiality, Integrity, Authentication):
- **Confidentiality**: un dato $x$ è noto solo all'agente corretto e non esiste nel futuro uno stato in cui un agente ostile conosca $x$.
- **Authentication**: se esiste uno stato futuro in cui i due partecipanti legittimi conoscono il segreto scambiato, allora l'agente ostile non lo conosce.
- **Integrity**: non esiste uno stato futuro in cui l'agente ostile riesce ad alterare la sezione di payload di un messaggio.

