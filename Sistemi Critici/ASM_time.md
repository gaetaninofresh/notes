# Timer in ASMETA

## Motivazione
Molti sistemi reali (safety-critical, cyber-physical) hanno vincoli temporali. Modellarli richiede primitive esplicite per la gestione del tempo.

## Approcci per modellare il tempo

In ASMETA il tempo può essere modellato tramite **funzioni monitorate**:
- `monitored time: Integer` — funzione intera monotona crescente, gestita dall'utente
- `monitored timePassed: Boolean` — flag booleano che indica il passare di un intervallo

> **Attenzione**: la correttezza del modello temporale (monotonicità, coerenza tra unità) è responsabilità dell'utente.

## TimeLibrary

ASMETA fornisce due librerie dedicate al tempo:

- **`TimeLibrary.asm`** — completa, supporta più unità temporali (`NANOSEC`, `MILLISEC`, `SEC`, `MIN`, `HOUR`)
- **`TimeLibrarySimple.asm`** — semplificata, usa solo `mCurrTimeSecs: Integer`

### Funzioni principali

| Funzione/Regola | Descrizione |
|---|---|
| `start(t)` | tempo di avvio del timer (dalla clock locale) |
| `duration(t)` | durata del timer nell'unità scelta |
| `timerUnit(t)` | unità di tempo del timer |
| `currentTime(t)` | tempo corrente nella stessa unità |
| `expired(t)` | `true` se `currentTime(t) >= start(t) + duration(t)` |
| `r_reset_timer[t]` | riavvia il timer aggiornando `start` |
| `r_set_duration[t, ms]` | imposta/modifica la durata |
| `r_set_timer_unit[t, unit]` | imposta/modifica l'unità di tempo |

### Inizializzazione

```
static timerA: Timer
default init s0:
  function duration($t in Timer) = if $t = timerA then 10 endif
  function start($t in Timer) = currentTime($t)
  function timerUnit($t in Timer) = if $t = timerA then MIN endif
```

## Simulazione del tempo

Con TimeLibrary l'utente può scegliere tra tre meccanismi di simulazione (dalle preferenze del simulatore ASMETA in Eclipse):

- **Java time** — usa il clock della JVM; impraticabile per intervalli lunghi
- **Ask User** — l'utente inserisce il valore temporale step-by-step
- **Auto increment** — il tempo avanza automaticamente di un delta fisso nell'unità scelta; se l'unità è `auto`, il delta si applica alla prima funzione temporale incontrata

> **Esame**  
> Modello ASM con timer da specifica <br> Uso di `expired`, `r_reset_timer` e inizializzazione dei timer <br> Distinzione tra TimeLibrary e TimeLibrarySimple
