le proprietà fondamentali sono:
    - **Reachability**: date certe condizioni un evento può verificarsi, espressa in CTL come EF \phi. Può anche essere espressa in forma condizionale: E(n \neq 0) U cond.
    - **Safety** (cose brutte non succedono): date certe condizioni un evento non può verificarsi, espressa in CTL come AG \phi. Può essere espressa in forma condizionale: A [ \phi U \psi ]
    - **Liveness** (cose buone succedono): date certe condizioni un evento si verifica, espressa in CTL come AG + AF -> (AG(req \to AF sat)).   
    - **Assenza di Deadlock**: il sistema non può trivarsi in condizioni che non consentono di fare un passo.
    - **Fairness**: sotto certe condizioni un evento accade infinitamente spesso, non può essere espressa in CTL.

**Esame**
Modello ASM  da specifica
Model checking con Labeling e SAT
Costruzione di formule temporali
