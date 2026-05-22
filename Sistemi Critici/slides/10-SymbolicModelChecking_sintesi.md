# Symbolic Model Checking e ROBDD

Questo documento sintetizza i concetti relativi alle strutture dati efficienti per la rappresentazione di funzioni booleane (OBDD e ROBDD) e la loro applicazione pratica nel Symbolic Model Checking per contrastare il problema dello State Explosion.

## 1. Strutture Dati Efficienti: BDD

Rappresentare funzioni booleane è fondamentale per la descrizione formale e la progettazione di circuiti. Le classiche rappresentazioni (Formule Proposizionali, DNF, CNF, Tabelle di Verità) spesso mancano di compattezza o rendono "hard" (computazionalmente onerosi) test critici come la validità o la soddisfacibilità.

Un **Albero di Decisione Binario (BDD - Binary Decision Diagram)** è un albero composto da:
- **Nodi non-terminali**: etichettati da variabili booleane, aventi due archi uscenti (uno tratteggiato per il valore 0, uno continuo per il valore 1).
- **Nodi terminali**: etichettati con i valori di verità $0$ o $1$.

Percorrendo l'albero dalla radice ai terminali in base all'assegnamento delle variabili, si determina il valore della funzione. Tuttavia, in forma di albero puro, le dimensioni crescono esponenzialmente ($2^{n+1}-1$ nodi per $n$ variabili, paragonabile alle $2^n$ righe di una tabella di verità).

## 2. Regole di Riduzione (ROBDD)

Per compattare la struttura, il BDD viene trasformato in un **Grafo Diretto Aciclico (DAG)** applicando tre regole di riduzione:
1. **C1 (Rimozione di terminali duplicati):** Se ci sono più nodi terminali con la stessa etichetta, se ne mantiene solo uno condiviso.
2. **C2 (Rimozione di nodi con uscite ridondanti):** Se tutti gli archi uscenti da un nodo $n$ puntano allo stesso nodo destinazione $m$, il nodo $n$ è ridondante, viene eliminato e i suoi archi entranti vengono deviati su $m$.
3. **C3 (Rimozione di non-terminali duplicati):** Se due sottoalberi sono identici, se ne mantiene solo uno condiviso, eliminando l'altro.

Un BDD a cui non è più applicabile alcuna di queste riduzioni è detto **Reduced BDD**.

## 3. Ordinamento delle Variabili e Forma Canonica

L'applicazione delle riduzioni può perdere efficacia se le variabili compaiono in ordini diversi lungo i cammini. 

Si introduce l'**Ordered BDD (OBDD)**: un BDD in cui le etichette dei nodi non-terminali seguono rigorosamente una lista ordinata di variabili (es. $[x_1, \dots, x_n]$). Su ogni cammino, se $x_i$ è seguita da $x_j$, deve valere $i < j$.

**Teorema della Forma Canonica:** Esiste un unico Reduced OBDD (ROBDD) che rappresenta una data funzione $f$, fissato l'ordinamento delle variabili. Due funzioni equivalenti avranno una struttura del ROBDD **identica**.

### Vantaggi della Forma Canonica ROBDD
- **Assenza di variabili ridondanti:** Variabili ininfluenti non compaiono nel grafo.
- **Test di Equivalenza Semantica:** Immediato, basta controllare l'identità strutturale dei due ROBDD ($O(1)$ con hash table in implementazione).
- **Test di Validità (Tautologia):** Una funzione è sempre vera se e solo se il suo ROBDD coincide con il nodo terminale $B_1$.
- **Test di Soddisfacibilità:** Una funzione è soddisfacibile se e solo se il suo ROBDD **non** coincide con il nodo terminale $B_0$.
- **Test di Implicazione ($f ightarrow g$):** Si verifica calcolando il ROBDD di $f \wedge 
eg g$; l'implicazione è valida se e solo se il risultato è $B_0$.

## 4. Algoritmi di Manipolazione dei ROBDD

- **`reduce`:** Etichetta i nodi dal basso verso l'alto con un ID numerico. Assegna `#0` a $B_0$ e `#1` a $B_1$. Per un nodo $n$ (livello variabile $x_i$), se $lo(n)$ e $hi(n)$ hanno lo stesso ID, il nodo $n$ prende l'ID del figlio (Regola C2). Se esiste già un nodo con la stessa variabile e stessi ID figli, si riutilizza quell'ID (Regola C3). Altrimenti assegna un nuovo ID.
- **`apply(op, Bf, Bg)`:** Calcola il ROBDD per $f \ op \ g$ usando l'**Espansione di Shannon**:
  $$f \ op \ g = (
eg x_i \wedge (f[0/x_i] \ op \ g[0/x_i])) ee (x_i \wedge (f[1/x_i] \ op \ g[1/x_i]))$$
  L'algoritmo scende ricorsivamente fino ai terminali, applica l'operatore e, in risalita, ricostruisce il ROBDD.
- **`restrict(v, x, Bf)`:** Calcola la funzione fissando il valore della variabile $x$ a $v$ ($0$ o $1$). Gli archi entranti in nodi con etichetta $x$ vengono diretti verso il ramo corretto e il nodo rimosso.
- **Quantificatori:** - $\exists x.f = f[0/x] ee f[1/x]$ (calcolato combinando `apply` OR e `restrict`).
  - $orall x.f = f[0/x] \wedge f[1/x]$ (calcolato combinando `apply` AND e `restrict`).

---

## 5. Symbolic Model Checking

Il Symbolic Model Checking unisce il Model Checking (es. algoritmo SAT per CTL) alla potenza dei ROBDD. L'idea chiave è operare su **insiemi di stati** in forma simbolica, codificandoli come funzioni booleane, per ovviare allo State Explosion.

### Rappresentazione degli Stati e degli Insiemi
- Ogni stato $s \in S$ è codificato da un vettore di bit $(v_1, v_2, \dots, v_n)$. Usando la funzione di etichettatura $L$, se $x_i \in L(s)$ allora $v_i=1$, altrimenti $v_i=0$. 
  Lo stato è rappresentato dal mintermine $l_1 \wedge l_2 \wedge \dots \wedge l_n$ (dove $l_i = x_i$ o $
eg x_i$).
- Un **insieme di stati** $T \subseteq S$ è descritto dalla sua **funzione caratteristica** $f_T : \{0,1\}^n ightarrow \{0,1\}$. In pratica, l'insieme è l'OR (disgiunzione) delle formule dei singoli stati che vi appartengono.
- Le operazioni insiemistiche classiche diventano operazioni logiche sui ROBDD:
  - Unione $\cup \implies ee$ (OR)
  - Intersezione $\cap \implies \wedge$ (AND)
  - Complemento $\implies 
eg$ (NOT)

### Rappresentazione della Relazione di Transizione ($\Delta$)
Una singola transizione $s ightarrow s'$ è vista come una coppia di vettori: $(v_1, \dots, v_n)$ per lo stato corrente e $(v'_1, \dots, v'_n)$ per lo stato futuro (variabili "primate").
La transizione è la formula $(l_1 \wedge \dots \wedge l_n) \wedge (l'_1 \wedge \dots \wedge l'_n)$.
L'intero grafo di transizione $f_ightarrow$ è dato dall'OR logico di tutte le singole transizioni possibili. L'intera dinamica del sistema diventa un singolo colossale ROBDD a $2n$ variabili.

### Calcolo delle Pre-immagini Simboliche
Per applicare l'algoritmo SAT (es. per calcolare $EX$, $EF$, $EG$), è necessario determinare quali stati possono raggiungere o raggiungono sicuramente un certo insieme target $X$.

1. **Pre-immagine Esistenziale ($pre_\exists$):** Insieme degli stati da cui parte *almeno una* transizione verso $X$.
   $$pre_\exists(X) = \{ s \in S \mid \exists s', (s ightarrow s' 	ext{ e } s' \in X) \}$$
   **Calcolo su ROBDD:** Dato $B_X$ (ROBDD per $X$) e $B_ightarrow$ (ROBDD transizioni):
   - Rinomina le variabili in $B_X$ nella loro forma primata $\implies B_{X'}$.
   - Esegui l'AND logico con le transizioni $\implies apply(\wedge, B_ightarrow, B_{X'})$.
   - Quantifica esistenzialmente le variabili future per tornare allo spazio degli stati correnti $\implies \exists \hat{x}'. (\dots)$

2. **Pre-immagine Universale ($pre_orall$):** Insieme degli stati da cui *tutte* le transizioni portano a $X$.
   $$pre_orall(X) = \{ s \in S \mid orall s', (	ext{se } s ightarrow s' 	ext{ allora } s' \in X) \}$$
   **Calcolo su ROBDD:** Sfrutta la dualità insiemistica per calcolarla partendo da $pre_\exists$:
   $$pre_orall(X) = S - pre_\exists(S - X)$$
