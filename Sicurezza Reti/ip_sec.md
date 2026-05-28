# IPSec

La protezione può essere applicata a diversi livelli dello stack:
- Livello 1-2: protezione fisica del cavo; **WEP** (Wired Equivalent Privacy, autenticazione/confidenzialità/integrità deboli); **802.1x** (port-based Network Access Control)
- Livello 3 con **IPSec**: autenticazione mutua, access control, confidenzialità e integrità dei dati
- Livello 4+ con **SSL/TLS**: autenticazione mutua o one-way, confidenzialità e integrità

## IPSec
IPSec è un'estensione sicura di IPv4/IPv6 composta da tre protocolli principali:
- **AH** (Authentication Header) – IP protocol 51: autenticazione e integrità
- **ESP** (Encapsulating Security Payload) – IP protocol 50: cifratura, autenticazione, integrità
- **IKE** (Internet Key Exchange) – UDP porta 500: negoziazione parametri e gestione delle chiavi

IPSec garantisce:
- **Data origin authentication / connectionless integrity**: impossibile mascherare o modificare pacchetti senza che il ricevente se ne accorga
- **Replay protection**: impossibile ritrasmettere pacchetti già inviati
- **Confidenzialità**: impossibile esaminare il contenuto dei datagram
- **Security policy**: mittente e ricevente definiscono il livello di protezione; i nodi intermedi scartano i pacchetti non conformi

### Benefici
- Dati "firmati" dal sender e "firma" verificata dal ricevente
- **Anti-replay protection**: opzionale; il sender può fornirla ma può essere ignorata dal ricevente
- **Key management tramite IKE**: negoziazione e stabilimento di sessioni; le sessioni possono essere rigenerate o cancellate automaticamente; le chiavi segrete sono stabilite in maniera sicura e autenticata

### Modalità operative

**Transport Mode**: l'header IPSec viene inserito nel pacchetto IP esistente senza crearne uno nuovo. Si usa quando i *cryptographic endpoints* coincidono con i *communication endpoints* (comunicazione host-to-host). Funziona bene in reti dove la dimensione del pacchetto è un problema.

**Tunnel Mode**: l'intero pacchetto IP viene cifrato e incapsulato in un nuovo pacchetto IP. Si usa quando almeno un *cryptographic endpoint* non è un *communication endpoint* (tipicamente tra gateway, per VPN site-to-site).

#### Utilizzi
- **Transport mode**: i *cryptographic endpoints* (entità che generano o processano un IPSec header) coincidono con i *communication endpoints* (mittente e destinatario). I communication endpoints sono tipicamente host (workstation, server), ma non sempre: es. un gateway gestito in remoto via SNMP da una workstation.
- **Tunnel mode**: almeno un cryptographic endpoint non è un communication endpoint; permette ai gateway di rendere sicuro il traffico IP al posto di altre entità. Si usa tipicamente tra router/firewall per VPN site-to-site.

#### Distinguere Transport da Tunnel in AH
Se il campo `next-header` vale **IP** → il pacchetto incapsula un intero IP datagram (Tunnel mode). Qualsiasi altro valore (TCP, UDP, ICMP, ecc.) indica Transport mode (connessione endpoint-to-endpoint sicura).

## Misure di sicurezza
### Authentication Header (AH)

Fornisce autenticazione della sorgente, integrità dei dati e protezione contro replay attack tramite sequence number a 32 bit. **Non fornisce confidenzialità.**

Usa hash a 96 bit con crittografia simmetrica (HMAC-SHA-96, HMAC-MD5-96). È **incompatibile con NAT**, poiché autentica anche l'header IP che NAT modifica.

#### Formato del pacchetto AH
L'**AH Header** contiene:
- `Next Header` – tipo del prossimo header
- `AH Length` – lunghezza dell'header AH
- `Reserved`
- `SPI` (Security Parameters Index)
- `Sequence Number`
- `Authentication Data` (tipicamente hash MD5 o SHA-1, 96 bit)

In **Transport mode** l'AH header è inserito tra l'IP header originale e il TCP/UDP header; tutto (tranne i campi mutabili dell'IP header come TTL) è autenticato.

In **Tunnel mode** l'intero datagram originale (IP header + payload) è autenticato e incapsulato in un nuovo IP header; il pacchetto è "sigillato" con un ICV. Mittente e destinazione possono essere diversi da quelli del pacchetto originale. Quando il pacchetto arriva a destinazione: se supera il controllo di autenticazione, l'AH header viene rimosso, il campo Proto=AH viene rimpiazzato con "Next Protocol" e il datagram viene riportato al suo stato originale.

### Encapsulating Security Payload (ESP)

Aggiunge a quanto offerto da AH la **confidenzialità** tramite cifratura a chiave simmetrica. Usa un sequence number a 32 bit contro i replay attack e algoritmi di integrity check. A differenza di AH, è compatibile con NAT quando si usa solo la parte di cifratura (autenticazione e cifratura non incorporano l'IP header modificato da NAT).

#### Formato del pacchetto ESP
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|               Security Parameters Index (SPI)                 |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      Sequence Number Field                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    IV (Initialization Vector)                 |
~                  Payload Data (Variable) [ENCRYPTED]          ~
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Padding (0-255 Bytes)          | Pad Len | Next Hdr  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|               Authentication Data (Variable) [opzionale]      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

In **Transport mode** incapsula solo il payload del datagram (progettato per host-to-host); l'IP header originale è lasciato intatto (indirizzi sorgente e destinazione invariati).

In **Tunnel mode** la distinzione Transport/Tunnel non è visibile dall'esterno: il Tunnel mode (via `next=IP`) è parte del payload cifrato, non accessibile a chi ispeziona il pacchetto.

#### Riepilogo Transport vs Tunnel per AH e ESP

| Modalità | AH | ESP |
|---|---|---|
| **Transport** | Autentica IP hdr + payload (eccetto campi mutabili) | Cifra payload; autentica ESP hdr + payload cifrato |
| **Tunnel** | Autentica nuovo IP hdr + IP hdr originale + payload | Cifra IP hdr originale + payload; autentica ESP hdr + payload cifrato |

### Protezione contro Replay

Sia AH che ESP usano un *sliding window* di sequence number. Il ricevente:
- **scarta** i pacchetti a sinistra della finestra (troppo vecchi)
- **accetta** i pacchetti dentro la finestra (se ICV corretto)
- **accetta e scorre** la finestra per i pacchetti a destra

La dimensione minima della finestra è 32 pacchetti (default 64). Il sequence number è a 32 bit (esteso a 64 bit con ESN), con solo i 32 bit meno significativi trasmessi.

#### Dettagli Sequence Number
L'IP header non contiene un sequence number, quindi un replay di un pacchetto autenticato sarebbe possibile senza questa misura. Il counter è a 32 bit, inizia da 1 (valore 0 quando si inizializza una SA), e l'anti-replay è opzionale ma attivo per default. Con un link da 1 Gbps e pacchetti da 1500 byte, 2³² pacchetti corrispondono a circa 6270 GB, ovvero circa 14 ore di trasmissione — ragione per cui esiste l'**Extended Sequence Number (ESN)** a 64 bit, con trasmissione dei soli 32 bit meno significativi.

### Security Association (SA)

Concetto fondamentale di IPSec. Una SA è una collezione di parametri per una sessione sicura, identificata da:
- **SPI** (Security Parameters Index) – indice univoco a 32 bit (analogo al numero di porta in TCP/UDP); permette multiple SA tra gli stessi due host
- Indirizzo IP di destinazione
- Identificatore del protocollo di sicurezza (AH o ESP)

Le SA sono **unidirezionali**: serve una SA per il traffico in ingresso e una per quello in uscita. Coinvolge sia host-to-host, che host-to-gateway (security gateways), che gateway-to-gateway.

Con le SA si usano due database:
- **SPD** (Security Policy Database): definisce quali servizi di sicurezza applicare a quali pacchetti IP e in che modo
- **SADB** (Security Association Database): memorizza algoritmi, chiavi, durata e sequence number per ogni SA (es. quale cifratura, quali chiavi, durata della SA, sequence number counter). La SA deve essere presente nel SADB in entrambe le parti.

#### Esempio di SPD

| Protocol | Local IP | Port | Remote IP | Port | Action | Comment |
|---|---|---|---|---|---|---|
| UDP | 1.2.3.101 | 500 | * | 500 | BYPASS | IKE |
| ICMP | 1.2.3.101 | * | * | * | BYPASS | Error messages |
| * | 1.2.3.101 | * | 1.2.3.0/24 | * | PROTECT: ESP intransport-mode | Encrypt intranet traffic |
| TCP | 1.2.3.101 | * | 1.2.4.10 | 80 | PROTECT: ESP intransport-mode | Encrypt to server |
| TCP | 1.2.3.101 | * | 1.2.4.10 | 443 | BYPASS | TLS: avoid double encryption |
| * | 1.2.3.101 | * | 1.2.4.0/24 | * | DISCARD | Others in DMZ |
| * | 1.2.3.101 | * | * | * | BYPASS | Internet |

### ISAKMP

**Internet Security Association and Key Management Protocol** (RFC 2408): usato per stabilire Security Associations e chiavi crittografiche. Fornisce solo il *framework* per l'autenticazione e lo scambio delle chiavi. I protocolli per lo scambio effettivo delle chiavi sono IKE e KINK (Kerberized Internet Negotiation of Keys).

### IKE (Internet Key Exchange)

IKE negozia automaticamente i parametri di sicurezza e le chiavi, e si articola in **due fasi**. Usa la porta UDP 500. Esistono cinque modi per una negoziazione IKE: due modi (Aggressive e Main mode) e tre metodi di autenticazione (pre-shared key, public key encryption, public key signature).

#### IKE Fase 1
Stabilisce un canale sicuro (ISAKMP SA) tra i due endpoint, che fornisce: autenticazione della sorgente, integrità e confidenzialità, protezione contro replay attack. Il canale è poi usato per gli scambi futuri IKE e per negoziare le IPSec SA in fase 2.

Due modalità:

**Main Mode** (6 messaggi): protegge l'identità dei peer. Tre scambi:
1. *Primo scambio* – negoziazione degli algoritmi e hash da usare per le comunicazioni IKE (match tra ISAKMP SA dei peer)
2. *Secondo scambio* – scambio Diffie-Hellman per generare materiale segreto condiviso (keying material) + nonces firmati per verificare l'identità
3. *Terzo scambio* – verifica dell'identità dell'altra parte (IP address, FQDN, email, DNS o KEY ID, tutti cifrati)

Al termine si ha un matching di ISAKMP SA bidirezionale tra i peer. La ISAKMP SA specifica: metodo di autenticazione, algoritmi hash e cifratura, gruppo Diffie-Hellman, durata in secondi o kilobytes, valori per la chiave condivisa.

**Aggressive Mode** (3 messaggi): più veloce ma espone l'identità dei peer prima di avere un canale sicuro. Tutto è compresso in tre pacchetti: l'iniziatore invia proposta + chiave pubblica DH + nonce + identità; il responder risponde con tutti i parametri selezionati; il mittente riconferma lo scambio. È possibile sniffare la connessione e scoprire chi ha formato la SA.

#### IKE Fase 2
Opera all'interno del canale sicuro stabilito in fase 1. Ha un solo modo (*Quick Mode*) e genera le SA IPSec unidirezionali per proteggere il traffico di rete. Gli end point sono identificati da `<IP, port>` o da insiemi di pacchetti (es. tutto il traffico verso 128.124.100.0/24). Tutto il traffico è cifrato secondo la ISAKMP SA.

#### Politiche IKE

**Fase 1** — definite in termini di suite, con: algoritmo di cifratura, algoritmo hash, metodo di autenticazione, gruppo Diffie-Hellman, durata della politica.

**Fase 2** — definite in termini di proposte: AH, ESP, IPComp, con tutti gli attributi necessari (lunghezza chiave, durata, ecc.).

## Stabilire le Security Associations
Prima di processare ogni pacchetto bisogna stabilire una SA tra i due cryptographic endpoint. Le SA possono essere stabilite:
- **Manualmente**: solo in configurazioni molto ristrette (es. due firewall in una VPN); si configurano su ogni nodo i partecipanti, AH e/o ESP (tunnel o transport), algoritmi e chiavi
- **Dinamicamente**: tramite ISAKMP/IKE, il metodo standard per autenticazione e key management

## Problemi di IPSec
- Eccessivamente complesso, con molte modalità di configurazione
- Può essere configurato in modo non sicuro
- Problemi di sicurezza lato client
- Overhead in implementazioni IPv4

# VPN (Virtual Private Network)

Una VPN permette a computer in sedi fisiche diverse di comunicare su reti non dedicate tramite crittografia, eliminando la necessità di costose linee dedicate.

## Setup tipico
Si designa un host come VPN server sulla rete; i computer esterni devono passare attraverso il VPN server per raggiungere gli host interni tramite autenticazione. Il VPN server è esposto verso l'esterno, mentre i computer interni sono protetti tramite firewall o indirizzi IP riservati.

## Vantaggi e svantaggi

**Vantaggi**: eliminazione di costose linee affittate; riduzione tariffe interurbane per accesso remoto; trasferimento del supporto ai provider; flessibilità di crescita; efficienza con banda larga.

**Svantaggi**: richiedono comprensione approfondita della sicurezza della rete pubblica e corretta implementazione; disponibilità e prestazioni dipendono da fattori esterni; devono supportare protocolli diversi dall'IP e dalla tecnologia di rete interna esistente.

## Tipologie - Security

- **Trusted VPN**: il provider garantisce l'integrità del percorso (es. MPLS, L2TP); nessuna cifratura end-to-end, ma il percorso è riservato al cliente. Nessuno al di fuori del provider può influire sulla creazione/modifica del percorso o modificare i dati in transito.
- **Secure VPN**: tutto il traffico è cifrato e autenticato tramite tunnel (es. IPSec, OpenVPN, SSL/TLS). Tutto il traffico deve essere cifrato e autenticato; gli amministratori delle due estremità di ogni tunnel devono accordarsi sulle proprietà di sicurezza.
- **Hybrid VPN**: combinazione delle due; le parti sicure possono essere controllate dal cliente o dal provider. Secure VPN danno sicurezza ma non assicurano i percorsi; Trusted VPN assicurano proprietà dei percorsi (QoS) ma non la sicurezza da intrusioni.

## Topologie - Scope

- **Intranet VPN**: collega uffici remoti della stessa azienda come fossero in una LAN; riduzione sostanziale dei costi rispetto alle linee dedicate
- **Remote Access VPN (RAS)**: consente ai telelavoratori di accedere alla rete aziendale tramite VPN client; coinvolge solo un gateway VPN; garantisce accesso sicuro indipendentemente dalla posizione fisica
- **Extranet VPN**: collega partner aziendali esterni, limitando l'accesso a risorse specifiche tramite firewall e tunnel. I partner possono accedere solo a risorse specifiche (es. un fornitore che accede al database dell'inventario) mentre il resto della rete aziendale rimane inaccessibile.

## Incapsulamento dei pacchetti
Le VPN operano al livello 2 (frame Ethernet) o livello 3 (pacchetto IP) del modello OSI. Il tunneling consente ai sender di incapsulare i dati in pacchetti IP che nascondono l'infrastruttura di routing/switching di Internet.

## Protocolli VPN

| Protocollo | Livello | Note |
|---|---|---|
| **PPTP** | 2 | Veloce, facile da configurare; compromesso dall'NSA, sconsigliato |
| **L2F** | 2 | Proposto da Cisco (RFC 2341); nessuna cifratura/autenticazione/integrità; evoluto in L2TP |
| **L2TP** | 2 (session 5) | Non cifra da solo; si usa con IPSec (L2TP/IPSec) |
| **IPSec** | 3 | Standard per Secure VPN; potenzialmente compromesso su vecchi device Cisco |
| **SSL/TLS** | 4+ | Base di OpenVPN e SSTP |
| **OpenVPN** | app | Open source, altamente configurabile, considerato il più sicuro |
| **IKEv2** | – | Veloce e stabile, ottimo per dispositivi mobili |
| **SSTP** | – | Solo Windows, integrato con SSL v3; proprietà Microsoft |
| **MPLS** | – | Usato nelle Trusted VPN come alternativa a L2TP |

### L2F (Layer 2 Forwarding)
Proposto da Cisco (RFC 2341). Consente di incapsulare datagrammi PPP in frame L2F, ma non assicura confidenzialità, autenticazione né integrità. Opera al Data Link layer (OSI 2). Veniva usato per VPN Trusted e Secure insieme a L2TP/L2TPv3. Evoluto nel protocollo L2TP.

### L2TP (Layer 2 Tunneling Protocol)
Sviluppato da Cisco come successore di L2F; standard IETF dalla RFC 2661 (1999). Protocollo di livello 2 OSI (in realtà opera al livello 5 – session layer), usa la porta UDP 1701, invia sessioni PPP in tunnel L2TP. Non assicura né confidenzialità né autenticazione → usato nelle VPN trusted o in combinazione con IPSec. Evoluzione: L2TPv3 (2005, RFC 3931), alternativa a MPLS.

### L2TP/IPSec
Standardizzato nella RFC 3193 per sopperire alla mancanza di confidenzialità di L2TP. Tre passaggi fondamentali:
1. Negoziazione tramite IKE
2. Creazione di un canale sicuro tramite ESP in Transport mode
3. Negoziazione dei parametri L2TP sul canale sicuro IPSec e tunneling attraverso questo

Differenza fondamentale: L2TP invia pacchetti in modalità non sicura attraverso un canale cifrato creato da IPSec (distinzione tra canale sicuro e tunnel).

### IKEv2
Tecnicamente non è il nome del protocollo VPN: si riferisce a un'implementazione L2TP/IPSec che usa questo protocollo di key exchange più recente e sicuro al posto del vecchio IKE. Veloce, stabile, particolarmente adatto a dispositivi mobili (gestisce bene il cambio di connessione). Supporta cifrari come 3DES, AES 256.

### MPLS (Multi Protocol Label Switching)
Standard multiprotocollo che supporta in modo flessibile e scalabile servizi di VPN, Traffic Engineering e QoS. Usato nelle VPN Secure e Trusted come alternativa a L2TP, e per applicazioni particolari come il trasporto di IPv6 su reti IP/MPLS.

### PPTP (Point-to-Point Tunneling Protocol)
Pubblicato nella RFC 2637 (mai ratificato). Crea una VPN su reti dial-up; è stato il protocollo più usato dalle VPN sin dalle origini. Buone prestazioni e facilmente configurabile. Funziona con due sessioni: una sessione PPP via GRE + una sessione TCP sulla porta 1723 per gestire la sessione GRE (difficile da instradare tramite firewall). Autenticazione e cifratura tramite MSCHAP v2 (proprietà Microsoft) o TLS; ulteriore cifratura possibile via MPPE (RFC 3078). **Sconsigliato**: compromesso dall'NSA, vulnerabile (crack in pochi giorni), Microsoft raccomanda SSTP o L2TP.

### SSTP (Secure Socket Tunneling Protocol)
Introdotto da Microsoft su Windows Vista SP1. Disponibile anche per Linux e RouterOS, ma principalmente orientato a Windows. Usa SSL v3. Facile da usare, integrato con Windows. **Criticità**: prodotto proprietario Microsoft; voci di backdoor integrate; l'SSTP è considerato poco affidabile per via della storica collaborazione Microsoft-NSA.

### OpenVPN
Usa SSLv3/TLSv1 e la libreria OpenSSL. Altamente configurabile; da il suo meglio su UDP, ma può essere configurato su qualsiasi porta (rende molto difficile il blocco da parte di servizi come Google). Supporta numerosi algoritmi: 3DES, AES, Camellia, Blowfish, CAST-128 (i provider si affidano principalmente a Blowfish o AES). Solitamente più veloce di IPSec. Diventato lo standard per la maggior parte dei servizi VPN; supportato dalla maggior parte delle piattaforme (inclusi Android e iOS) tramite software di terze parti. Configurazione più complessa rispetto a L2TP/IPSec e PPTP.

## TUN/TAP

Per intercettare pacchetti IP a livello kernel, le applicazioni tunnel usano interfacce virtuali (i normali socket fornirebbero solo la parte dati di un pacchetto, senza header IP):
- **TUN**: lavora a livello OSI 3 (IP); l'invio di qualsiasi pacchetto a TUN comporta la consegna del pacchetto al programma nello userspace
- **TAP**: lavora a livello OSI 2 (Ethernet); usato per adattatori di rete virtuale per macchine guest che si connettono a un dispositivo fisico della macchina host

I sistemi operativi hanno due tipi di interfacce di rete: fisiche (corrispondono alla NIC fisica) e virtuali (rappresentazione virtualizzata, es. dispositivo di loopback).

### Routing verso il tunnel
Il traffico delle applicazioni sul host viene instradato verso l'interfaccia TUN dal routing stack; il programma TUN legge il pacchetto IP, lo cifra/incapsula e lo invia tramite socket verso l'altro endpoint del tunnel, dove viene de-incapsulato e consegnato alle applicazioni di destinazione.

## Sicurezza delle VPN

### Compromissione degli standard crittografici
La NSA è stata accusata di aver influenzato il NIST per indebolire gli standard di crittografia, introducendo backdoor invisibili. In particolare: lo standard NIST **Dual EC DRBG** è considerato probabilmente insicuro. RSA Security ha privatamente consigliato a clienti di smettere di usare certi algoritmi di crittografia. Silent Circle ha chiuso il proprio server di posta e abbandonato gli standard NIST nel 2013.

### Valutazione dei protocolli VPN
- **PPTP**: compromesso dall'NSA; sconsigliato.
- **L2TP/IPSec**: pesantemente compromesso dalla NSA; utile per dispositivi mobili dove OpenVPN non è disponibile, ma non per usi critici.
- **OpenVPN** e **IKEv2**: rimangono le opzioni più affidabili.
- **SSTP**: integrato con Windows, ma non affidabile per la collaborazione storica Microsoft-NSA.

### Vulnerabilità PIXPocket (Cisco PIX)
Sono emerse vulnerabilità nei dispositivi Cisco PIX usati per VPN IPSec. La vulnerabilità, chiamata **PIXPocket** (analisi del ricercatore Mustafa Al-Bassam), sfruttava il fatto che una richiesta IKE appositamente predisposta faceva scaricare al dispositivo parti della propria memoria; iterando il processo, un attaccante poteva ottenere le password dei dispositivi PIX e violare il firewall. Poiché i PIX venivano usati per VPN IPSec, le organizzazioni che li usavano avrebbero potuto essere spiate dalla NSA con facilità. Cisco ha smesso di vendere il PIX nel 2009; la fuga di notizie indicherebbe che tra il **2002 e il 2008** la NSA è stata in grado di violare la sicurezza di Cisco.
