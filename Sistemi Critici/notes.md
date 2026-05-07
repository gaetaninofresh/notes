# Alberi di decisione binaria
**Def.** Un albero di decisione binaria è la prappresentazione di una funzione booleana. I nodi interni sono etichettati con var booleane e le foglie con valori 0/1.
Gli archi tratteggiati rappresentano transizioni con valori della variabile nel nodo di origine 0.

### Es.

Data $f(x_1, x_2, x_3)$ con tavola di verità:

| x1 | x2 | x3 | f |
| -- | -- | -- | - |
| 0  | 0  | 0  | 0 |
| 0  | 0  | 1  | 0 |
| 0  | 1  | 0  | 0 |
| 0  | 1  | 1  | 1 |
| 1  | 0  | 0  | 0 |
| 1  | 0  | 1  | 1 |
| 1  | 1  | 0  | 0 |
| 1  | 1  | 1  | 1 |

Possiamo esprimerla in forma disgiuntiva $f(x_a, x_2, x_3) = (\neg x_1 \land x_2 \land x_3) \lor ( x_1 \land \neg x_2 \land x_3) \lor ( x_1 \land  x_2 \land x_3)$

[Riduzione Albero - ADD IMAGE]

La riduzione consiste nell'eliminazione di tutti i nodi i cui rami finiscono nello stesso nodo.
- **C1 - Rimozione terminali duplicati**: più nodi terminali con la stessa etihetta possono essere collassati in un unico nodo condiviso
- **C2 - Rimozione nodi con uscite ridondanti**: se tutti gli archi uscenti da $n$ puntano a $m$ eliminiamo $n$ e portiamo i suoi archi interni su $m$.

- **C3 - Rimozione nodi non-terminali duplicati**: se due sottoalberi di $T$ sono identici possiamo condividerne uno ed eliminare l'altro.

**Teo.** Data una funzione $f$ essa ha un unico ROBDD $\leftrightarrow$ L'applicazione ripetuta di C1-C3 in qualsiasi ordine fino a quando possibile porta allo stesso ROBDD.

Questo garantisce:
- Assenza di var ridondanti
- Test di equivalenza semantica
- Test di validità
- Test di implicazione [LOW PRIO]
- Test di soddisfacibilità [HIGH PRIO]

**NB.** Possiamo ottenere $\neg f$ invertendo le etichette delle foglie.

# Algoritmi ROBDD
#### reduce
#### apply
applica un operatore ad una coppia ROBDD e calcola il ROBDD di $f op g$
#### restrict
calcola il ROBDD di $f$ fissando il valore di una var
#### exist 
calola il ROBDD che rappresenta $\exists x.f$. Per definizione:
$\exists x.f := f[0/x] \lor f[1/x]$

**NB.** è possibile calcolare il ROBDD tramite apply e restrict [...]

### Applicazione a Kripke
I ROBDD possono essere usati in verifica formale per rappresentare:
- insiemi
- modelli

Le operazioni possono essere implementate con gli algoritmi ROBDD.




