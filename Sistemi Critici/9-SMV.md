# NuSMV
Implementazione di Simbolic Model Checking, nasce per la verifica di microprocessori e successivamente rimplementato.
Supporta sia sistemi mono che multi agente (sincroni ed asincroni), l'asincronia viene eseguita in maniera analoga ad un sistema single thread (solo uno avanaza per step).
Ad ogni step di computazione assegna tutti i valori possibili ad ogni variabile (nondeterministic). Sono quindi solamente supportati tipi:
- Boolean
    - `TRUE/FALSE`
- Integer
- Enum
    - `a : {1, 2, 3}`
- Array
    - `array 0..3 of boolean`

## Modellazione di macchine in NuSMV
Le macchine vanno modellate come automi di Kripke, con struttura:
```
MODULE main
VAR
    v0: boolean;
    ...
    vn: ...;
ASSIGN
    init(v0) := 0;
    next(v0) := ...;
SPEC
    AG EX 1
    ...
```

- `MODULE`: inizio specifiche macchina
- `VAR`: inizializzazione variabili (vanno inizializzate tutte)
- `ASSIGN`: etichettatura degli stati e assegnazione dei loro successori. Le variabili che non hanno un valore assegnato possono variare assegnazione durante la simulazione (equivalente monitored)
    - `DEFINE`: macro per sostituire la definizione di una variabile con un espressione logica senza creare una nuova variabile nel BDD.
- `SPEC`: formula/e temporali da verificare

> **NB** 
> Non sono consentite definizioni circolari per la `next`

### Kripke in NuSMV
- Definiamo una VAR state contenente tutti glis stati della macchina
- DEFINE delle variabili come gli stati in cui sono vere
- ASSIGN con definizione dello state come stato iniziale, next definito come case di tutte le possibili transizioni di stato 

> **NB** 
> In caso di stato iniziale non specificato si assumono tutti gli stati come possibili iniziali.

## Sincronismo
...
## Asincronismo
La composizione asincrona è impementata attraverso `process`: tra tutti i moduli `process` ne viene scelto nondeterministicamente uno che avanza.

> **NB**
> La proprietà di fairness va espressa esplicitamente tramite `FAIRNESS running` (running è una proprietà associata ai moduli).

## Modelli con stati iniziali con var monitorate
In ASM con stati iniziali con transizioni basate sul valore di monitorate la verifica di specifiche diventa problematica: se vogliamo imporre un EF il solver, applicando il non determinismo per ogni possibile combinazione di valori incappa in stati inizali che non lo verificano e di conseguenza non verifica la condizione. Si aggira il problema provando la $\neg$EF.


## Model Advisor
Tool per la review dei modelli, basato su NuSMV. Permette di verificare uan serie di meta-proprietà del modello:
- **mp1**: No inconsistent update is ever performed
- **mp2**: Every conditional rule must be complete
- **mp3**: Every rule can eventually fire
- **mp4**: No assignment is always trivial
- **mp5**: For every domain element e there exists a location which has value e
- **mp6**: Every controlled function can take any value in its co-domain
- **mp7**: Every controlled location is updated and every location is read

Queste possono essere raccolte in 3 macro-classi in base alle proprietà che verificano:
- Coerenza: garantisce assenza di aggiornamenti inconsistenti (MP1)
- Completezza: ogni comportamento del sistema sia modellato in modo esplicito: completezza di regole condizionali (MP2) ed effettivo aggiornamento delle lunzioni controllate (MP7)
- Minimalità (no over-specification): No elementi definiti/dichiarati ma mai utilizzati (MP3, MP4, MP5, MP6); solo funzioni necessarie (MP7)
