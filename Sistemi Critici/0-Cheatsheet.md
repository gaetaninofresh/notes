## ASM
#### Aggiornamenti inconsistenti
Sia $l=(f, (i_1, \dots, i_n))$ locazione per $f(x_1, \dots, x_n)$. Un update set $U$ contiene update inconsistenti se $\exists l(f, a), l(f,b) \in U | a \neq b$

#### ASM multi-agenti
Un ASM multi-agente è $M = \{ (a, \text{program}(a)), a \in \text{AGENT}, \text{program}(a) \text{ è ASM} \}$

## Kripke
$\mathcal{M} = (S, I, \lambda, L)$:
- $S$: insieme degli stati
    - $f_S = (p_1 \land \dots \land p_n) \lor (p_1 \land \dots \land p_n) \dots$, con $p1, \dots, p_n$ atomi booleani dello stato 
- $I$: insieme stati iniziali
- $\lambda \subseteq S \times S$: relazione di transizione
    - $f_\lambda(V, V') = \bigvee_{(s, s') \in R} (s \land s')$ con $(s, s')$ rappresentazioni booleane degli stati del cammino
- $L: S \to 2^{AP}$: funzione di labeling (mappa stati a a proposizioni vere nello stato)

## CTL
#### Priorità degli operatori
1. Operatori unari: $\neg, AG, EG, AF, EF, AX, EX$
2. Operatori binari: $\lor, \land$
3. $AU, EU$

#### Semantica CTL
* **$\mathcal{M}, s \models AX\phi$**
    $$
    \forall s' \in S : (s, s') \in R \implies \mathcal{M}, s' \models \phi
    $$
    In tutti gli stati immediatamente successivi a $s$, la formula $\phi$ è soddisfatta.
* **$\mathcal{M}, s \models EX\phi$**
    $$
    \exists s' \in S : (s, s') \in R \land \mathcal{M}, s' \models \phi
    $$
    Esiste almeno uno stato immediatamente successivo a $s$ in cui la formula $\phi$ è soddisfatta ($E$ è il duale di $A$).
* **$\mathcal{M}, s \models AG\phi$**
    $$
    \forall \pi = (s_0, s_1, \dots) \text{ con } s_0 = s, \forall i \ge 0 : \mathcal{M}, s_i \models \phi
    $$
    In ogni possibile cammino di computazione uscente da $s$, la proprietà $\phi$ è globalmente invariante (vera in ogni stato del cammino).
* **$\mathcal{M}, s \models EG\phi$**
    $$
    \exists \pi = (s_0, s_1, \dots) \text{ con } s_0 = s, \forall i \ge 0 : \mathcal{M}, s_i \models \phi
    $$
    Esiste almeno un cammino di computazione uscente da $s$ in cui la proprietà $\phi$ è globalmente invariante.
* **$\mathcal{M}, s \models AF\phi$**
    $$
    \forall \pi = (s_0, s_1, \dots) \text{ con } s_0 = s, \exists i \ge 0 : \mathcal{M}, s_i \models \phi
    $$
    In ogni possibile cammino di computazione uscente da $s$, la proprietà $\phi$ è soddisfatta in almeno uno stato futuro.
* **$\mathcal{M}, s \models EF\phi$**
    $$
    \exists \pi = (s_0, s_1, \dots) \text{ con } s_0 = s, \exists i \ge 0 : \mathcal{M}, s_i \models \phi
    $$
    Esiste almeno un cammino di computazione uscente da $s$ tale per cui la proprietà $\phi$ è soddisfatta in almeno uno stato futuro.
* **$\mathcal{M}, s \models A[\phi_1 \mathcal{U} \phi_2]$**
    $$
    \forall \pi = (s_0, s_1, \dots) \text{ con } s_0 = s, \exists i \ge 0 : \left( \mathcal{M}, s_i \models \phi_2 \land \forall 0 \le j < i : \mathcal{M}, s_j \models \phi_1 \right)
    $$
    In ogni possibile cammino di computazione uscente da $s$, esiste uno stato in cui $\phi_2$ è vera e, in tutti gli stati strettamente precedenti ad esso lungo il cammino, $\phi_1$ è vera.
* **$\mathcal{M}, s \models E[\phi_1 \mathcal{U} \phi_2]$**
    $$
    \exists \pi = (s_0, s_1, \dots) \text{ con } s_0 = s, \exists i \ge 0 : \left( \mathcal{M}, s_i \models \phi_2 \land \forall 0 \le j < i : \mathcal{M}, s_j \models \phi_1 \right)
    $$
    Esiste almeno un cammino di computazione uscente da $s$ in cui esiste uno stato in cui $\phi_2$ è vera e, in tutti gli stati strettamente precedenti ad esso lungo il cammino, $\phi_1$ è vera.

#### Equivalenze Semantiche
* $\neg AF\phi \leftrightarrow EG\neg\phi$
* $\neg EF\phi \leftrightarrow AG\neg\phi$
* $\neg AX\phi \leftrightarrow EX\neg\phi$
* $AF\phi \leftrightarrow A[\top \ U \ \phi]$
* $EF\phi \leftrightarrow E[\top \ U \ \phi]$

#### Riduzione ad insieme adeguato
1. $\top = \neg\perp$
2. $\phi_1 \lor \phi_2 = \neg(\neg\phi_1 \land \neg\phi_2)$
3. $\phi_1 \rightarrow \phi_2 = (\neg\phi_1 \vee \phi_2)$
4. $AX(\phi_1) = (\neg EX\neg\phi_1)$
5. $EF(\phi_1) = E[\top \ U \ \phi_1]$
6. $EG(\phi_1) = (\neg AF\neg\phi_1)$
7. $AG(\phi_1) = (\neg EF\neg\phi_1)$
8. $A[\phi_1 \ U \ \phi_2] = \neg(E[\neg\phi_2 \ U \ (\neg\phi_1 \wedge \neg\phi_2)] \vee \neg AF\phi_2)$


### Proprietà CTL

| Proprietà | Descrizione / Concetto | Traduzione CTL | Note e Varianti |
| :--- | :--- | :--- | :--- |
| **Raggiungibilità** | Possibilità di raggiungere una data situazione | $EF \varphi$| **Condizionata:** Usa operatore$EU$<br>**Da stati raggiungibili:** $AG(EF \ \text{init})$ |
| **Safety** | Cose cattive non si verificano mai | $AG \varphi$| **Condizionata:** Usa operatore$AU$ |
| **Liveness** | Cose buone prima o poi si verificano | Combinazioni (es. $AG + AF$) | $AG(\text{richiesta} \to AF \ \text{risposta})$ |
| **No Deadlock** | Il sistema non finisce mai in stallo | $AG(EX \ \top)$ | Da ogni stato esiste sempre uno stato successore |
| **Fairness** | Un evento accade un numero infinito di volte | *Non esprimibile* | Manca l'operatore "infinitely often" in puro CTL |

### SAT
#### Espressioni Base e Riduzioni
| Formula ($\varphi$) | Valutazione / Sotto-chiamata |
| :--- | :--- |
| $\top$|$S$ |
| $\bot$|$\emptyset$ |
| $p$|$\{s \in S \mid p \in L(s)\}$ |
| $\neg\varphi_1$|$S \setminus \text{SAT}(\varphi_1)$ |
| $\varphi_1 \land \varphi_2$|$\text{SAT}(\varphi_1) \cap \text{SAT}(\varphi_2)$ |
| $\varphi_1 \lor \varphi_2$|$\text{SAT}(\varphi_1) \cup \text{SAT}(\varphi_2)$ |
| $\varphi_1 \to \varphi_2$|$\text{SAT}(\neg\varphi_1 \lor \varphi_2)$ |
| $EX(\varphi_1)$|$\text{SAT\_EX}(\varphi_1)$ |
| $E[\varphi_1 U \varphi_2]$|$\text{SAT\_EU}(\varphi_1, \varphi_2)$ |
| $AF(\varphi_1)$|$\text{SAT\_AF}(\varphi_1)$ |

---

#### Calcolo degli Operatori Temporali

**$EX$ (`SAT_EX`)**
* **Inizializzazione:** $X = \text{SAT}(\varphi)$
* **Risultato:**
    $Y = \{s \in S \mid \exists s', s \to s' \text{ e } s' \in X\}$

**$AF$ (`SAT_AF`)** — *Minimo Punto Fisso*
* **Inizializzazione:** $X = S$, $Y = \text{SAT}(\varphi)$
* **Iterazione** (ripeti finché $X = Y$):
    1.  $X = Y$
    2.  $Y = Y \cup \{s \in S \mid \forall s' \text{ se } s \to s' \text{ allora } s' \in Y\}$
* **Risultato:** $Y$

**$EU$ (`SAT_EU`)** — *Minimo Punto Fisso*
* **Inizializzazione:** $W = \text{SAT}(\varphi)$, $X = S$, $Y = \text{SAT}(\psi)$
* **Iterazione** (ripeti finché $X = Y$):
    1.  $X = Y$
    2.  $Y = Y \cup (W \cap \{s \in S \mid \exists s', s \to s' \text{ e } s' \in Y\})$


## NUSMV

#### Struttura
```
MODULE main
VAR      -- Definisce lo spazio degli stati (S)
DEFINE   -- Definisce le etichette/macro (L)
ASSIGN   -- Definisce stati iniziali (I) e transizioni (R)
CTLSPEC  -- Proprietà da verificare
```

#### VAR (Variabili = Stati $S$)
* **Tipi ammessi:** Solo finiti. `boolean`, enumerati (`{ready, busy}`), range interi (`0..10`), array (`array 0..3 of boolean`).
* Una variabile dichiarata in `VAR` ma senza `ASSIGN` viene trattata come input esterno. Cambia in modo totalmente non deterministico ad ogni step.

#### ASSIGN (Dinamica = $I$ e $R$)
* **Inizializzazione ($I$):** `init(x) := value;` (anche non deterministico: `{val1, val2}`).
* **Transizione ($R$):** `next(x) := case ... esac;`
* `case` è valutato **dall'alto verso il basso**. Deve SEMPRE finire con `TRUE : <valore_default>;`. 
* Vietate le dipendenze circolari nello stesso passo (`x := y; y := x;`). Consentite solo sui `next`.

#### Composizione (Sincrona vs Asincrona)
* **Sincrona (Default):** `m1 : Modulo();` $\rightarrow$ Tutti i moduli avanzano simultaneamente allo scatto del clock.
* **Asincrona:** `m1 : process Modulo();` $\rightarrow$ Avanza un solo processo alla volta (interleaving).
* Con `process` un modulo potrebbe non essere mai schedulato. È **obbligatorio** aggiungere `FAIRNESS running` per impedirlo.

