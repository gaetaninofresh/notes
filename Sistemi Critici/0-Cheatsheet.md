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

## SMV

#### Rappresentazione degli Stati e degli Insiemi
- Ogni stato $s \in S$ è codificato da un vettore $(v_1, v_2, \dots, v_n)$. 
    - Usando la funzione di etichettatura $L$, se $x_i \in L(s)$ allora $v_i=1$, altrimenti $v_i=0$. 
    - Lo stato è rappresentato dal mintermine $l_1 \wedge l_2 \wedge \dots \wedge l_n$ (dove $l_i = x_i$ o $
eg x_i$).
- Un **insieme di stati** $T \subseteq S$ è descritto dalla sua **funzione caratteristica** $f_T : \{0,1\}^n \rightarrow \{0,1\}$. 
    > In pratica, l'insieme è l'OR delle formule dei singoli stati che vi appartengono.

#### Rappresentazione della Relazione di Transizione ($\Delta$)
- Una singola transizione $s \rightarrow s'$ è vista come una coppia di vettori: $((v_1, \dots, v_n)$, $(v'_1, \dots, v'_n))$.
- La transizione è $(l_1 \wedge \dots \wedge l_n) \wedge (l'_1 \wedge \dots \wedge l'_n)$ con $l_1, \dots, l_n$ mintermini degli stati.
- L'intero grafo di transizione $f_\rightarrow$ è dato dall' $\lor$ di tutte le singole transizioni possibili.

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
