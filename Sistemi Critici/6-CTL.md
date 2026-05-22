# Logica CTL e Model Checking

Il Model Checking è una tecnica di verifica formale basata sull'utilizzo di una logica temporale per dimostrare che un modello $\mathcal{M}$ (che descrive il comportamento di un sistema) soddisfi una specifica proprietà $\phi$ ($\mathcal{M} \models \phi$).

## Logiche Temporali
Le logiche temporali si dividono in due classi fondamentali:
* **Linear-Time Logics (LTL):** Considerano il tempo come un insieme di cammini (sequenze di istanti di tempo).
* **Branching-Time Logics (BTL):** Rappresentano il tempo come un albero, dove la radice è l'istante corrente.

La **Computation Tree Logic (CTL)** è una logica branching-time discreta. I suoi modelli sono strutture ad albero in cui il futuro non è deterministico: esistono differenti computazioni possibili e una di queste sarà il percorso effettivamente realizzato.

### Sintassi CTL
Le formule CTL sono costruite a partire da un insieme di formule atomiche $AP$ (descrizioni atomiche del sistema) e combinano:
1.  **Connettivi logici classici:** $\top, \perp, \neg, \wedge, \vee, \rightarrow$
2.  **Quantificatori di cammino (Path quantifiers):**
    * $A$: "along All paths" (inevitabilmente).
    * $E$: "along at least one path" (esiste un cammino).
3.  **Operatori temporali lineari:**
    * $X$: next state (stato successivo).
    * $F$: some future state (un qualche stato futuro).
    * $G$: all future states (tutti gli stati futuri/globally).
    * $U$: until (fino a quando).

> **Nota Bene:** Gli operatori temporali $X, F, G, U$ in CTL devono sempre essere immediatamente preceduti da un quantificatore di cammino $A$ o $E$ (es. $AG, EF, A[\dots U \dots]$).

#### Priorità degli operatori
Gli operatori legano con priorità diverse (da più alta a più bassa):
1. Operatori unari: $\neg, AG, EG, AF, EF, AX, EX$
2. Operatori binari: $\lor, \land$
3. $AU, EU$



### Semantica CTL
Un'interpretazione di CTL è una **struttura di Kripke** $\mathcal{M} = (S, \Delta, I, L)$:
* $S$: insieme degli stati.
* $\Delta$: relazione di transizione (senza deadlock: ogni stato ha almeno un successore).
* $I$: insieme di stati iniziali.
* $L$: funzione di etichettatura che associa a ogni stato le formule atomiche vere in esso.

Si impone l'assenza di deadlock: $\forall s \in S \exists s' | s \to s'$ (per garantirla si può aggiungere uno stato fittizio $s_d$)

La semantica stabilisce quando una formula $\phi$ è vera in uno stato $s$ del modello $\mathcal{M}$ (scritto $\mathcal{M}, s \models \phi$). Alcuni esempi chiave:
* **$AX\phi$**: in *tutti* gli stati immediatamente successivi $\phi$ è vera.
* **$EX\phi$**: in *almeno uno* stato immediatamente successivo $\phi$ è vera.
* **$AG\phi$**: in tutti i cammini a partire dallo stato corrente, $\phi$ è vera in *tutti* gli stati (sempre vera).
* **$EF\phi$**: esiste almeno un cammino in cui, in un *qualche* stato futuro, $\phi$ diventa vera.
* **$A[\phi_{1} \ U \ \phi_{2}]$**: in tutti i cammini, $\phi_{1}$ rimane vera fino a quando $\phi_{2}$ diventa vera.

### Equivalenze Semantiche
Due formule CTL $\psi$ e $\phi$ sono semanticamente equivalenti se ciascun stato in ciascun modello che soddisfa l'una soddisfa anche l'altra.

Alcune equivalenze fondamentali sono:
* $\neg AF\phi \leftrightarrow EG\neg\phi$
* $\neg EF\phi \leftrightarrow AG\neg\phi$
* $\neg AX\phi \leftrightarrow EX\neg\phi$
* $AF\phi \leftrightarrow A[\top \ U \ \phi]$
* $EF\phi \leftrightarrow E[\top \ U \ \phi]$

Grazie a queste equivalenze, è possibile definire un **insieme di simboli adeguato** composto esclusivamente da $\{ \perp, \neg, \vee, AF, EU, EX \}$. Ogni formula CTL può essere riscritta in una formula equivalente che utilizza solo questi connettivi di base nel seguente modo:
1. $\top = \neg\perp$
2. $\phi_1 \wedge \phi_2 = \neg(\neg\phi_1 \vee \neg\phi_2)$
3. $\phi_1 \rightarrow \phi_2 = (\neg\phi_1 \vee \phi_2)$
4. $AX(\phi_1) = (\neg EX\neg\phi_1)$
5. $EF(\phi_1) = E[\top \ U \ \phi_1]$
6. $EG(\phi_1) = (\neg AF\neg\phi_1)$
7. $AG(\phi_1) = (\neg EF\neg\phi_1)$
8. $A[\phi_1 \ U \ \phi_2] = \neg(E[\neg\phi_2 \ U \ (\neg\phi_1 \wedge \neg\phi_2)] \vee \neg AF\phi_2)$

## Algoritmo di Model Checking (Labelling Algorithm)
Dati un modello $\mathcal{M}$ e uno stato $s \in S$, assieme ad una formula $\phi$, il problema è verificare se $\mathcal{M}, s \models \phi$. Questo si articola solitamente in due varianti:
1. Considerare uno stato iniziale $s_0 \in I$ e verificare che $\mathcal{M}, s_0 \models \phi$.
2. Estrarre l'insieme $X \subseteq S$ di *tutti* gli stati in cui $\phi$ è soddisfatta. Quest'ultimo caso risolve implicitamente anche il primo controllando se $s_0 \in X$.

### L'algoritmo
Il Labelling Algorithm prende in INPUT un modello $\mathcal{M}$ e una formula $\phi$, e restituisce in OUTPUT l'insieme $X \subseteq S$ degli stati che la soddisfano.
L'algoritmo restringe preventivamente il set di operatori CTL all'insieme adeguato ($\perp, \neg, \vee, AF, EU, EX$) ed etichetta ciascuno stato $s \in S$ con l'insieme delle sottoformule vere in esso, procedendo dalle sottoformule più piccole (bottom-up) fino alla formula completa $\phi$.

Valutazione in base alla struttura della sottoformula $\psi$:
* **$\perp$**: Nessuno stato viene etichettato.
* **$p$ (atomo)**: Si etichetta lo stato $s$ con $p$ se $p \in L(s)$.
* **$\psi_1 \wedge \psi_2$**: Si etichetta lo stato $s$ se è già etichettato con entrambe le sottoformule $\psi_1$ e $\psi_2$.
* **$\neg\psi_1$**: Si etichetta lo stato $s$ con $\neg\psi_1$ se $s$ *non* è già etichettato con $\psi_1$.
* **$AF\psi_1$**:
  1. Se $s$ è etichettato con $\psi_1$, allora viene etichettato anche con $AF\psi_1$.
  2. Uno stato viene etichettato con $AF\psi_1$ se *tutti* i suoi stati successori sono etichettati con $AF\psi_1$. Si ripete questo processo fin quando è possibile (raggiungimento del punto fisso).
* **$E[\psi_1 \ U \ \psi_2]$**:
  1. Se $s$ è etichettato con $\psi_2$, viene etichettato con $E[\psi_1 \ U \ \psi_2]$.
  2. Uno stato viene etichettato con $E[\psi_1 \ U \ \psi_2]$ se è etichettato con $\psi_1$ e *almeno uno* dei suoi successori è etichettato con $E[\psi_1 \ U \ \psi_2]$. Anche in questo caso si itera fin quando è possibile.
* **$EX\psi_1$**: Si etichetta lo stato $s$ con $EX\psi_1$ se *almeno uno* dei suoi successori è etichettato con $\psi_1$.

### Pseudo-codice dell'algoritmo di Model Checking
La procedura principale può essere tradotta nella funzione `SAT`, che manipola direttamente gli insiemi:

```text
function SAT(φ)
begin
    case φ di:
        ⊤: return S
        ⊥: return ∅
        atomo p: return {s ∈ S | p ∈ L(s)}
        ¬φ_1: return S - SAT(φ_1)
        φ_1 ∧ φ_2: return SAT(φ_1) ∩ SAT(φ_2)
        φ_1 ∨ φ_2: return SAT(φ_1) ∪ SAT(φ_2)
        φ_1 → φ_2: return SAT(¬φ_1 ∨ φ_2)
        AX(φ_1): return SAT(¬EX¬φ_1)
        EX(φ_1): return SAT_EX(φ_1)
        A[φ_1 U φ_2]: return SAT(¬(E[¬φ_2 U (¬φ_1 ∧ ¬φ_2)] ∨ EG¬φ_2))
        E[φ_1 U φ_2]: return SAT_EU(φ_1, φ_2)
        EF(φ_1): return SAT(E[⊤ U φ_1])
        EG(φ_1): return SAT(¬AF¬φ_1)
        AF(φ_1): return SAT_AF(φ_1)
        AG(φ_1): return SAT(¬EF¬φ_1)
    end case
end function
```

#### Procedure per gli operatori temporali specifici

**Gestione di $EX$ (`SAT_EX`)**
```text
function SAT_EX(φ)
local var X, Y
begin
    X := SAT(φ);
    Y := {s_0 ∈ S | s_0 → s_1 per qualche s_1 ∈ X}
    return Y
end function
```

**Gestione di $AF$ (`SAT_AF`)**
Si calcola mediante il minimo punto fisso.
```text
function SAT_AF(φ)
local var X, Y
begin
    X := S;
    Y := SAT(φ)
    repeat until X = Y
    begin
        X := Y;
        Y := Y ∪ {s | ∀s' con s → s' è vero che s' ∈ Y}
    end
    return Y
end function
```

**Gestione di $EU$ (`SAT_EU`)**
```text
function SAT_EU(φ, ψ)
local var W, X, Y
begin
    W := SAT(φ)
    X := S
    Y := SAT(ψ);
    repeat until X = Y
    begin
        X := Y;
        Y := Y ∪ (W ∩ {s | ∃s' con s → s' e s' ∈ Y})
    end
    return Y
end function
```

### Complessità e State Explosion Problem
La complessità dell'algoritmo di labelling è $O(f \times V \times (V \times E))$, con $f$ pari al numero di connettivi nella formula $\phi$, $V$ il numero di stati ed $E$ il numero di transizioni. 

Sebbene l'algoritmo sia **lineare rispetto alla dimensione del modello**, la dimensione stessa dello spazio degli stati del modello tende a crescere in maniera esponenziale all'aumentare del numero di variabili (aggiungerne una raddoppia la complessità) e dei componenti del sistema. 
Questo ostacolo fundamental è noto como **State Explosion Problem**.
