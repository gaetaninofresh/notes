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
