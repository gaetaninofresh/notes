# ASM Multi Agente
Gli ASM multi agente consentono di rappresentare sistemi distribuiti tramite un insieme di *agenti* che operano in maniera concorrente con:
- Movimenti sincroni/asincroni
- Stati globali condivisi

I singoli agenti possono essere creati dinamicamente, hanno una visione parziale dello stato globale ed ognuno esegue il proprio programma.

Classifichiamo gli agenti come:
- **Sincroni**: eseguono il programma in modo parallelo e sono sincronizzati su un clock di sistema implicito
- **Asincroni**: eseguono il programma in modo parallelo ma hanno clock separati ed operano nei propri stati localii

> In AsmetaL: `[asyncr] asm ASM_name` <br> `asyncr` è un parametro opzionale e se omesso la macchina viene considerata sincrona.

Ogni agente ha visione parziale dello stato globale indicata con $View(a,M)$ (vista dell'agente $a$ dello stato globale dell'ASM $M$).

Gli agenti possono avere una **visione privata**. Per $f:A \to B$ la dichiarazione privata è $f:Agent \times A \to B$. <br> $f(self, x)$ denota la versione privata di $f(x)$ dell'agente corrente $self$.

##### Def.
Una sync/async ASM è un insieme di coppie $(a, ASM(a)))$ con:
- agenti $a \in Agent$ (dominio Agenti)
- programmi $ASM(a)$ ASM di base

> In caso di ASM multi-agente è utile definire l'header come:
>
> ```
> [ 
>   import m_1 [( id_11,...,id_1h1 )]
>   ...
>   import m_k [( id_k1,...,id_khk )]
> ]
> [ export id_1,...,id_e ] or [ export * ]
>
> signature :
>   [ dom_declarations ]
>   [ fun_declarations ]
> ```

## Computazione di ASM multi-agente
### ASM sincrone
Le ASM sincrone lavorano in parallelo e l'insieme opera negli **stati globali**, ottenuti dall'unione degli stati delle ASM.
Un ASM multi-agente con agenti sincroni ha una *quasi-sequential-run*: una sequenza di stati $S_0, S_1, ..., S_n$ in cui lo stato $S_i$ è ottenuto da $S_{i-1}$ eseguendo in parallelo le regole di tutti gli agenti.
> **???.** Lo step avviene solo se tutti gli agenti fanno un passo o anche se è solo un sottoinsieme a cambiare stato?

### ASM asincrone
Nelle ASM asincrone gli agenti possono eseguire con clock indipendenti e i loro step possono avere durate diverse.
Un  ASM multi-agente con agenti asincroni ha una *partially-ordered run*: un insieme parzialmente ordinato $(M, <)$ di mosse $m$ dei suoi agenti che soddisfano tre condizioni:
1. **Storia finita**: $\forall m \in M \{m' | m' < m \}$ è un insieme finito (ciascuna mossa ha un numero finito di predecessori).
2. **Sequenzialità degli agenti**: $\forall a \in Agent, \{m | m \in M, a$ esegue $m\}$ è linearmente ordinato per $<$.
3. **Coerenza**: $\forall m \in M, m$ è il risultato delle mosse di agenti (versione su slide, poco rilevante).

