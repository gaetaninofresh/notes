# AsmetaL to NuSMV

## Boolean Domain
In NuSMV non c'è undef

## Enum domain
non essendoci undef NuSMV aggiunge a tutti i domini enum l'elemento ENUDOM_UDNEF 

## Interi
I sottoinsiemi di Integer vengono mappati 1:1, ma non può mappare domini infiniti (bisogna definirli sempre come sottoinsiemi).
Per supportare undef viene aggiunto min_int nell'insieme NuSMV.

Tutti gli altri tipi di dominio non sono supportati.
Conviene quindi definire un modello completo ed uno ridotto usato solamente per la verifica.

## Funzioni
Le funzioni 0-arie vengono mappate come variabili. Diversamente le funzioni vengono mappate instanziando quante funzioni quanti sono gli elementi del dominio e assegnandogli il corrispondente valore nel codominio.

Sono supportate solo funzioni controlled e monitored. 
Funzioni che prendono come argomento funzioni non sono rappresentabili, vanno ...

### Funzioni controllate
Per gestire il global update set ASMetaL esplora l'albero delle condizioni per determinare il valore delle funzioni secondo le condizioni e costruire il global uodate set.
Si sfrutta la costruzione di questo set per traslare le transizioni di stato in NuSMV.

> **NB**
> NuSMV non esclude aggiornamenti inconsistenti in quanto opera in maniera sequenziale e non in batch come ASMetaL (par ... endpar)

### Funzioni monitorate
...

### Funzioni statiche e derivate
Translate tramite DEFINE invece di istanziare una variabile (non nel BDD).

## Regole
### Forall rule
Dovendo applicare delle operazioni per più elementi di un dominio in parallelo viene rappresentato ripetendo il blocco next per ogni possibile valore nel dominio della variabile.

# Choose rule
...


# CTL/LTL in ASMetaL
Le regole CTL/LTL sono implementate tramite CTLSPEC, vanno specificate **subito prima** della main rule. Hanno mappatura 1:1 con NuSMV.
Anche gli invarianti vengono mappati in CTLSPEC come AG.
Si possono usare tutti i quantificatori nella definizione delle CTLSPEC in ASMetaL.



