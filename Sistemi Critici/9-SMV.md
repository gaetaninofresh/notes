# Appunti su NuSMV e AsmetaSMV

## 1. NuSMV
Implementazione di Simbolic Model Checking, nasce per la verifica di microprocessori e successivamente rimplementato. Supporta sia sistemi mono che multi agente (sincroni ed asincroni), l'asincronia viene eseguita in maniera analoga ad un sistema single thread (solo uno avanza per step). Ad ogni step di computazione assegna tutti i valori possibili ad ogni variabile (nondeterministic). Sono quindi solamente supportati tipi: Boolean (`TRUE/FALSE`), Integer, Enum (`a : {1, 2, 3}`), Array (`array 0..3 of boolean`).

### Modellazione di macchine in NuSMV
Le macchine vanno modellate come automi di Kripke. Struttura base:
* **MODULE main**: inizio specifiche macchina.
* **VAR**: inizializzazione variabili (vanno inizializzate tutte).
* **ASSIGN**: etichettatura degli stati e assegnazione dei loro successori. Le variabili che non hanno un valore assegnato possono variare assegnazione durante la simulazione (equivalente monitorate).
* **DEFINE**: macro per sostituire la definizione di una variabile con un'espressione logica senza creare una nuova variabile nel BDD.
* **SPEC**: formula/e temporali da verificare. Non sono consentite definizioni circolari per la `next`.

Per mappare in un automa di Kripke, definiamo una `VAR state` contenente tutti gli stati, usiamo `DEFINE` per le variabili come gli stati in cui sono vere, e `ASSIGN` con lo stato iniziale (se omesso si assumono tutti possibili) e le transizioni `next`.

### Sincronismo e Asincronismo
La composizione asincrona è implementata attraverso `process`: tra tutti i moduli process ne viene scelto nondeterministicamente uno che avanza. La proprietà di fairness va espressa esplicitamente tramite `FAIRNESS running`. Modelli con stati iniziali con variabili monitorate pongono problemi per le verifiche `EF` a causa del non determinismo sui valori iniziali; si aggira provando la negazione `!EF`.

### Model Advisor
Tool per la review dei modelli, basato su NuSMV, che verifica meta-proprietà:
* **Coerenza**: assenza di aggiornamenti inconsistenti (MP1).
* **Completezza**: completezza di regole condizionali (MP2) ed effettivo aggiornamento delle funzioni controllate (MP7).
* **Minimalità**: nessun elemento non utilizzato e assenza di over-specification (MP3, MP4, MP5, MP6).

## 2. AsmetaSMV: da AsmetaL a NuSMV
L'obiettivo di AsmetaSMV è sfruttare le potenzialità del model checker NuSMV nel framework Asmeta, evitando di scrivere due modelli separati per simulazione e verifica.

### Domini
Sono supportati solo i domini di cardinalità finita il cui tipo è presente in NuSMV. Per semplificare, conviene definire un modello completo e uno ridotto usato solo per la verifica.
* **Boolean Domain**: Pienamente supportato e mappato nel tipo `boolean` di NuSMV (valori simbolici `TRUE`/`FALSE`). In NuSMV non c'è il corrispettivo di undef.
* **Enum Domain**: I nomi non cambiano (richieste almeno due lettere maiuscole). Non essendoci l'undef nativo, NuSMV aggiunge a tutti i domini enum l'elemento `ENUMDOM_UNDEF`.
* **Concrete Domain (Interi/Naturali)**: Diventano enum types di interi in NuSMV. I sottoinsiemi di Integer vengono mappati 1:1, ma non potendo mappare domini infiniti vanno sempre definiti come sottoinsiemi. Per gestire l'undef viene aggiunto il valore `-2147483647` (min_int).

### Funzioni
Per ogni funzione AsmetaL, AsmetaSMV crea tante variabili quante sono le locazioni.
* Le funzioni 0-arie vengono mappate come singola variabile con lo stesso nome.
* Funzioni di arietà 1 vengono mappate in $k$ variabili (es. `idFunc_elDom`), e per arietà maggiori nel prodotto logico delle combinazioni dei domini.
* Funzioni che prendono come argomento altre funzioni non sono supportate nativamente; occorre esplicitare l'argomento usando la `let rule` per estrarne il valore in una variabile temporanea.

**Funzioni Controllate**
Le locazioni sono aggiornate nelle transition rule (direttamente dalla macchina). AsmetaSMV esplora l'albero delle condizioni, calcola gli update set di ogni stato e li unisce in un "global update set" associando valori e condizioni. Il global update set è mappato nella sezione `ASSIGN` tramite l'espressione `case` di NuSMV. In caso di aggiornamenti inconsistenti, il parser AsmetaL non li rileva e NuSMV prosegue aggiornando la variabile al primo valore la cui condizione è soddisfatta nell'albero sequenziale.

**Funzioni Monitorate**
Lette ma non aggiornate dalla macchina (variano ad opera dell'ambiente). In NuSMV sono dichiarate come `VAR` ma non vengono né inizializzate né inserite nell'`ASSIGN`. NuSMV crea uno stato parallelo per ogni possibile valore assumibile.

**Funzioni Statiche e Derivate**
Non cambiano il proprio valore tramite macchine/update. Sono tradotte avvalendosi della dichiarazione `DEFINE` invece di instanziare una vera e propria variabile nel BDD. Per essere corrette in NuSMV, le condizioni valutate in queste definizioni devono essere esaustive e la funzione non può mai assumere un valore undef.

### Regole
Il tool esegue una visita in profondità partendo dalla `main rule` gestendo uno stack globale (`Conds`) su cui aggiunge e rimuove le condizioni booleane via via che entra ed esce dagli scope.
* **Update rule**: L'aggiornamento viene salvato nel global update set condizionato ai valori presenti nello stack. Per i domini numerici, viene automaticamente aggiunta una condizione al `case` in NuSMV per impedire aggiornamenti a valori fuori dal dominio della variabile, disattivabile tramite parametro `-nc`.
* **Block rule (`par`)**: Regole valutate in parallelo; lo stack di base delle condizioni all'inizio di ogni ramo è lo stesso.
* **Conditional/Case rule**: La condizione viene spinta (`push`) sullo stack per valutare il ramo logico (`then` o `case specifici`) e rimossa in uscita; se presente, l'opposto logico viene aggiunto per i rami `else` e `otherwise`.
* **Forall rule**: Il blocco viene espanso operando per ogni possibile combinazione e tupla di valori del dominio. Il numero di rami valutati in NuSMV corrisponde al prodotto delle cardinalità dei domini interessati.
* **Choose rule**: Crea una variabile per ogni elemento logico scelto (es. `var_vi_chld`) tipizzata col dominio. Genera in NuSMV il codice iterando i rami sostituendo la variabile con i valori correnti e aggiunge un blocco `INVAR` per forzare la simulazione affinché queste variabili assumano valori compatibili con le guardie imposte dalla choose in ogni stato.

### Esplorazione dei Rami Condizionali e Gestione degli Stati
Il processo di traduzione da AsmetaL a NuSMV si basa su una visita in profondità dell'albero delle regole, a partire dalla `main rule`. Durante questa visita, AsmetaSMV gestisce dinamicamente uno stack globale denominato `Conds`. Ogni volta che il tool incontra un costrutto condizionale (come una regola `if-then-else` o uno `switch-case`), la corrispondente espressione booleana viene inserita nello stack (`push`) per definire il contesto del ramo corrente, e viene successivamente rimossa (`pop`) quando si esce dal relativo scope di visibilità.

Quando il tool raggiunge l'aggiornamento effettivo di una locazione (una `update rule`), tale operazione viene registrata all'interno del *global update set*. La guardia logica che determina l'attivazione di quell'aggiornamento viene calcolata come il prodotto logico (AND) di tutte le condizioni correntemente presenti nello stack `Conds`.

Questo meccanismo introduce una profonda differenza semantica e operazionale tra l'esecuzione nativa in Asmeta e la sua trasposizione in NuSMV:
1. **Aggiornamenti in parallelo vs sequenziali:** In AsmetaL, i blocchi `par` implicano che tutti gli aggiornamenti vengano collezionati simultaneamente alla fine del passo algoritmico. Se due regole tentano di assegnare valori diversi alla stessa locazione nello stesso momento, il simulatore ad alto livello (AsmetaS) rileva un errore semantico di "update inconsistente" e interrompe l'esecuzione.
2. **Comportamento in NuSMV:** Il *global update set* viene tradotto nella sezione `ASSIGN` del modello NuSMV impiegando il costrutto condizionale `case ... esac`. All'interno del blocco `case`, NuSMV valuta le espressioni in modo strettamente sequenziale, dall'alto verso il basso. Di conseguenza, se un modello AsmetaL contiene un aggiornamento inconsistente non rilevato in fase di parsing, il model checker NuSMV non segnalerà alcun errore a runtime; esso assegnerà alla variabile il valore associato alla prima condizione soddisfatta in ordine di scrittura, continuando la verifica su un modello logico alterato rispetto alle intenzioni originarie del progettista.

### Esempi di Regole e Trasposizione NuSMV
Di seguito viene riportato un esempio pratico di modellazione per ciascuna delle regole supportate da AsmetaSMV e la rispettiva traduzione generata per il model checker NuSMV:

```asm
// ==========================================
// 1. UPDATE RULE (con controllo del dominio numerico)
// ==========================================
// AsmetaL
domain MyDomain subsetof Integer
definitions:
    domain MyDomain = {1..4}
    main rule r_Main =
        foo := foo + 1

// NuSMV (Viene inserita la guardia di bound checking per il tipo)
ASSIGN
    next(foo) :=
        case
            (foo + 1) in {1, 2, 3, 4} : (foo + 1);
            TRUE : foo;
        esac;


// ==========================================
// 2. BLOCK RULE (par ... endpar)
// ==========================================
// AsmetaL
main rule r_Main =
    par
        fool := AA
        foo := BB
    endpar

// NuSMV (Le transizioni parallele condividono lo stesso stack di condizioni iniziali)
ASSIGN
    next(fool) := AA;
    next(foo) := BB;


// ==========================================
// 3. CONDITIONAL RULE (if ... then ... else)
// ==========================================
// AsmetaL
main rule r_Main =
    if (guard = CC) then
        foo := AA
    else
        foo := BB
    endif

// NuSMV (Le ramificazioni popolano lo stack Conds e generano i rami del case)
ASSIGN
    next(foo) :=
        case
            !(guard = CC) : BB;
            (guard = CC) : AA;
            TRUE : foo;
        esac;


// ==========================================
// 4. CASE RULE (switch)
// ==========================================
// AsmetaL
main rule r_Main =
    switch (sw)
        case AA: foo := CC
        case BB: foo := BB
        otherwise foo := AA
    endswitch

// NuSMV (Il ramo otherwise diventa il prodotto logico delle disuguaglianze precedenti)
ASSIGN
    next(foo) :=
        case
            !(sw = AA) & !(sw = BB) : AA;
            (sw = BB) : BB;
            (sw = AA) : CC;
            TRUE : foo;
        esac;


// ==========================================
// 5. FORALL RULE
// ==========================================
// AsmetaL
domain ConcrDom subsetof Integer
definitions:
    domain ConcrDom = {1..3}
    main rule r_Main =
        forall $x in ConcrDom with $x < 3 do
            foo($x) := 1

// NuSMV (Il blocco viene espanso per ciascun elemento del dominio finito)
ASSIGN
    next(foo_1) :=
        case
            (1 < 3) & 1 in {1, 2, 3} : 1;
            TRUE : foo_1;
        esac;
    next(foo_2) :=
        case
            (2 < 3) & 1 in {1, 2, 3} : 1;
            TRUE : foo_2;
        esac;
    next(foo_3) :=
        case
            (3 < 3) & 1 in {1, 2, 3} : 1;
            TRUE : foo_3;
        esac;


// ==========================================
// 6. CHOOSE RULE
// ==========================================
// AsmetaL
domain MyDomain subsetof Integer
definitions:
    domain MyDomain = {1..4}
    main rule r_Main =
        choose $x in MyDomain with $x < 2 do
            foo := $x + 2
        ifnone
            foo := 4

// NuSMV (Viene introdotta una variabile di scelta e un vincolo di invarianza globale)
VAR
    var_x_0 : 1..4; // Dichiarazione della variabile logica associata alla choose
ASSIGN
    next(foo) :=
        case
            (var_x_0 = 1) & (1 < 2) & (1 + 2) in {1, 2, 3, 4} : (1 + 2);
            (var_x_0 = 2) & (2 < 2) & (2 + 2) in {1, 2, 3, 4} : (2 + 2);
            (var_x_0 = 3) & (3 < 2) & (3 + 2) in {1, 2, 3, 4} : (3 + 2);
            (var_x_0 = 4) & (4 < 2) & (4 + 2) in {1, 2, 3, 4} : (4 + 2);
            !(1 < 2) & !(2 < 2) & !(3 < 2) & !(4 < 2) & 4 in {1, 2, 3, 4} : 4;
            TRUE : foo;
        esac;
INVAR
    ((var_x_0 = 1) & (1 < 2)) |
    ((var_x_0 = 2) & (2 < 2)) |
    ((var_x_0 = 3) & (3 < 2)) |
    ((var_x_0 = 4) & (4 < 2)) |
    (!(1 < 2) & !(2 < 2) & !(3 < 2) & !(4 < 2));
```

### Proprietà CTL/LTL

Le proprietà si dichiarano dopo gli invarianti nel modello e vengono mappate 1:1 verso le spec di NuSMV.
- **Sintassi**: Le `CTLSPEC` precedono sempre le `LTLSPEC`.
- **Operatori**: Vengono utilizzati sfruttando i file di importazione `CTLlibrary.asm` e `LTLlibrary.asm` (che forniscono le funzioni unarie e binarie come ag, ex, ef, eu, ecc.) e si possono usare quantificatori.
- **Invarianti**: Un vincolo `invariant over id_1...id_n: expr` impone il rispetto dell'espressione in ogni stato. In AsmetaSMV, tale invariante viene tradotto in NuSMV convertendolo nella proprietà formale corrispondente: `CTLSPEC ag(expr)`.
