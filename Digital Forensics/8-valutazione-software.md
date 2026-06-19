# Valutazione Forense del Software
La valutazione forense del software ha come obiettivo la determinazione del valore di mercato o del costo di riproduzione di un asset intangibile. A differenza di una stima ordinaria, la Digital Forensics (DF) impone un accertamento preliminare rigoroso sull'esistenza materiale, la consistenza tecnica, l'integrità e la titolarità del codice.

## Contesti di Applicazione e Inquadramento Civilistico
La stima del software è rilevante in molteplici scenari:
- **Fusioni e Acquisizioni (M&A)**: per la due diligence tecnica.
- **Rivalutazione di asset**: obbligo di iscrizione tra le *Immobilizzazioni Immateriali* seguendo principi contabili.
- **Stima del danno e Fallimentare**: quantificazione dei danni per plagio/pirateria o valorizzazione dell'asset da parte di curatori fallimentari (distinguendo tra valore di liquidazione e di continuità).

Un'errata valorizzazione espone al reato di falso in bilancio. In caso di conferimenti in S.p.A., è obbligatoria una *perizia asseverata* nominata dal Tribunale.

## Fasi Preliminari e Certificazione Forense
Prima di ogni stima economica, il perito deve certificare tre elementi fondamentali:
1. **Autore**: identificazione di chi ha scritto il codice analizzando i log (Git, SVN).
2. **Data Certa**: determinazione della marcatura temporale certificata di specifiche release.
3. **Integrità**: verifica della corrispondenza sorgenti/binari tramite hash crittografico (SHA-256).

## Standard e Metodologie di Stima
La valutazione deve basarsi su standard internazionali, come **ISO/IEC 19770** (Software Asset Management) e **ISO/IEC 25010** (SQuaRE - per definire la qualità oggettiva del software in 8 caratteristiche).

### Criterio del Costo (Cost Approach)
Valuta quanto costerebbe riprodurre il software da zero (ore/uomo, Function Points, costi di licenze e infrastrutture). 
*Il ruolo della DF*: validare le ore dichiarate confrontandole con lo storico dei *commit*, per smascherare "gonfiamenti" e distinguere codice nuovo da quello riciclato o copiato.

### Criterio di Mercato (Market Approach)
Confronta l'asset con transazioni simili recenti calcolando multipli di valore (es. EV/Revenue). 
*Il ruolo della DF*: fornire i parametri tecnici (stack, scalabilità, qualità) per giustificare gli aggiustamenti rispetto ai *comparables*.

### Criterio Finanziario (Income Approach - DCF)
Stima il valore tramite i flussi di cassa futuri attualizzati.
*Il ruolo della DF*: valutare la qualità architetturale. Un codice disorganizzato (*spaghetti code*) alza i costi di manutenzione e accresce il rischio di obsolescenza, riducendo drasticamente il valore dei flussi netti attesi.

## Analisi Tecnica e Metriche di Qualità
Le metriche oggettive impattano direttamente sul valore economico:
- **Metriche di Halstead**: misurano la complessità computazionale del codice.
- **Indice di Manutenibilità**: quantifica la facilità di intervento sul codice.
- **Debito Tecnico**: il costo implicito delle scorciatoie di sviluppo accumulate, considerato una *passività latente*.
- **Metadati di sviluppo**: l'analisi di pipeline CI/CD o file di dipendenza rileva backdoor occulte, credenziali hard-coded e librerie obsolete.

## Fattori di Rischio e Svalutazione dell'Asset
Il perito deve ponderare i seguenti elementi legali e tecnici che alterano il valore della stima:

### Licenze Open Source e SCA
L'uso improprio di componenti "copyleft" (es. licenza GPL/AGPL) in software proprietario costringe al rilascio pubblico dei sorgenti derivati, potenzialmente azzerando il valore dell'asset. La prevenzione passa tramite una sistematica *Software Composition Analysis (SCA)*.

### Cybersecurity e Privacy (GDPR)
Vulnerabilità gravi (standard OWASP Top 10) rappresentano sconti diretti sul valore. L'assenza di principi *Privacy by Design* espone a sanzioni amministrative fino al 4% del fatturato, imponendo fondi rischi o pesanti svalutazioni strategiche.

### Proprietà Intellettuale (Diritti Morali vs Patrimoniali)
La mancanza di chiari contratti di cessione dei diritti patrimoniali da parte di dipendenti o collaboratori freelance genera incertezza giuridica e conseguente deprezzamento della stima.

## Contesti Valutativi Particolari
- **Quantificazione dei danni**: somma di lucro cessante, danno emergente ed eventuale retroversione degli utili illeciti, dimostrati tramite log di installazioni illegittime e analisi di somiglianza.
- **Software Legacy**: la scarsità di professionisti per sistemi datati ma vitali (es. COBOL bancario) ne accresce enormemente il valore di continuità operativa.
- **Patent Box**: per l'accesso agli incentivi fiscali, la DF è necessaria per tracciare la "lineage" del codice e dimostrare il nesso causale tra costi di R&S sostenuti e asset prodotto.

## Struttura della Perizia e Responsabilità
La relazione tecnica si compone di quattro sezioni logiche: Identificazione Forense, Analisi Legale, Analisi Tecnica e Calcolo Economico.

Il perito estimatore assume responsabilità civile e penale. Vi è il preciso obbligo etico e legale di segnalare manipolazioni ad hoc del codice (codice ridondante, refactoring cosmetico pre-stima o falsi commit) orchestrate per incrementare artificialmente le metriche e le dimensioni del software.

## Frontiere Future
- **Intelligenza Artificiale**: la stima di un modello di AI impone metodologie doppie che separino il valore intrinseco del dataset di addestramento da quello dell'architettura generativa.
- **Blockchain**: l'uso di timestamping su blockchain (o NFT) diventerà lo standard per ottenere una prova di esistenza delle release software crittograficamente immutabile.
