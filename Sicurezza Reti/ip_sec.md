# IPSec

Il protocollo IP non offre autenticazione, integrità, confidenzialità né non-ripudio, ed è vulnerabile ad attacchi come source spoofing e replay.

La protezione può essere applicata a diversi livelli dello stack: a livello 2 (WEP, 802.1x), a livello 3 con **IPSec**, a livello 4+ con **SSL/TLS**.

## IPSec
IPSec è un'estensione sicura di IPv4/IPv6 composta da tre protocolli principali:
- **AH** (Authentication Header) – IP protocol 51: autenticazione e integrità
- **ESP** (Encapsulating Security Payload) – IP protocol 50: cifratura, autenticazione, integrità
- **IKE** (Internet Key Exchange) – UDP porta 500: negoziazione parametri e gestione delle chiavi

### Obiettivi
IPSec garantisce:
- **Data origin authentication / connectionless integrity**: impossibile mascherare o modificare pacchetti senza che il ricevente se ne accorga
- **Replay protection**: impossibile ritrasmettere pacchetti già inviati
- **Confidenzialità**: impossibile esaminare il contenuto dei datagram
- **Security policy**: mittente e ricevente definiscono il livello di protezione; i nodi intermedi scartano i pacchetti non conformi

### Modalità operative

**Transport Mode**: l'header IPSec viene inserito nel pacchetto IP esistente senza crearne uno nuovo. Si usa quando i *cryptographic endpoints* coincidono con i *communication endpoints* (comunicazione host-to-host).

**Tunnel Mode**: l'intero pacchetto IP viene cifrato e incapsulato in un nuovo pacchetto IP. Si usa quando almeno un *cryptographic endpoint* non è un *communication endpoint* (tipicamente tra gateway, per VPN site-to-site).

## Misure di sicurezza
### Authentication Header (AH)

Fornisce autenticazione della sorgente, integrità dei dati e protezione contro replay attack tramite sequence number a 32 bit. **Non fornisce confidenzialità.**

Usa hash a 96 bit con crittografia simmetrica (HMAC-SHA-96, HMAC-MD5-96). È **incompatibile con NAT**, poiché autentica anche l'header IP che NAT modifica.

### Encapsulating Security Payload (ESP)

Aggiunge a quanto offerto da AH la **confidenzialità** tramite cifratura a chiave simmetrica. Usa un sequence number a 32 bit contro i replay attack e algoritmi di integrity check. A differenza di AH, è compatibile con NAT quando si usa solo la parte di cifratura.

### Protezione contro Replay

Sia AH che ESP usano un *sliding window* di sequence number. Il ricevente:
- **scarta** i pacchetti a sinistra della finestra (troppo vecchi)
- **accetta** i pacchetti dentro la finestra (se ICV corretto)
- **accetta e scorre** la finestra per i pacchetti a destra

La dimensione minima della finestra è 32 pacchetti (default 64). Il sequence number è a 32 bit (esteso a 64 bit con ESN), con solo i 32 bit meno significativi trasmessi.

### Security Association (SA)

Concetto fondamentale di IPSec. Una SA è una collezione di parametri per una sessione sicura, identificata da:
- **SPI** (Security Parameters Index)
- Indirizzo IP di destinazione
- Identificatore del protocollo di sicurezza (AH o ESP)

Le SA sono **unidirezionali**: serve una SA per il traffico in ingresso e una per quello in uscita.

Con le SA si usano due database:
- **SPD** (Security Policy Database): definisce quali servizi di sicurezza applicare a quali pacchetti
- **SADB** (Security Association Database): memorizza algoritmi, chiavi, durata e sequence number per ogni SA

### IKE (Internet Key Exchange)

IKE negozia automaticamente i parametri di sicurezza e le chiavi, e si articola in **due fasi**:

##### IKE Fase 1
Stabilisce un canale sicuro (ISAKMP SA) tra i due endpoint, con due modalità:
- **Main Mode** (6 messaggi): protegge l'identità dei peer; negozia algoritmi, esegue Diffie-Hellman, verifica l'identità cifrata
- **Aggressive Mode** (3 messaggi): più veloce ma espone l'identità dei peer prima di avere un canale sicuro

##### IKE Fase 2
Opera all'interno del canale sicuro stabilito in fase 1. Ha un solo modo (*Quick Mode*) e genera le SA IPSec unidirezionali per proteggere il traffico di rete.

## Problemi di IPSec
- Eccessivamente complesso, con molte modalità di configurazione
- Può essere configurato in modo non sicuro
- Problemi di sicurezza lato client
- Overhead in implementazioni IPv4


# VPN (Virtual Private Network)

Una VPN permette a computer in sedi fisiche diverse di comunicare su reti non dedicate tramite crittografia, eliminando la necessità di costose linee dedicate.

### Tipologie - Security

- **Trusted VPN**: il provider garantisce l'integrità del percorso (es. MPLS, L2TP); nessuna cifratura end-to-end, ma il percorso è riservato al cliente
- **Secure VPN**: tutto il traffico è cifrato e autenticato tramite tunnel (es. IPSec, OpenVPN, SSL/TLS)
- **Hybrid VPN**: combinazione delle due; le parti sicure possono essere controllate dal cliente o dal provider

### Topologie - Scope

- **Intranet VPN**: collega uffici remoti della stessa azienda come fossero in una LAN
- **Remote Access VPN**: consente ai telelavoratori di accedere alla rete aziendale tramite VPN client
- **Extranet VPN**: collega partner aziendali esterni, limitando l'accesso a risorse specifiche tramite firewall e tunnel

### Protocolli VPN

| Protocollo | Livello | Note |
|---|---|---|
| **PPTP** | 2 | Veloce, facile da configurare; compromesso dall'NSA, sconsigliato |
| **L2TP** | 2 (session 5) | Non cifra da solo; si usa con IPSec (L2TP/IPSec) |
| **IPSec** | 3 | Standard per Secure VPN; potenzialmente compromesso su vecchi device Cisco |
| **SSL/TLS** | 4+ | Base di OpenVPN e SSTP |
| **OpenVPN** | app | Open source, altamente configurabile, considerato il più sicuro |
| **IKEv2** | – | Veloce e stabile, ottimo per dispositivi mobili |
| **SSTP** | – | Solo Windows, integrato con SSL v3; proprietà Microsoft |
| **MPLS** | – | Usato nelle Trusted VPN come alternativa a L2TP |

### TUN/TAP

Per intercettare pacchetti IP a livello kernel, le applicazioni tunnel usano interfacce virtuali:
- **TUN**: lavora a livello OSI 3 (IP); consegna pacchetti IP all'applicazione nello userspace
- **TAP**: lavora a livello OSI 2 (Ethernet); usato per adattatori di rete virtuale per macchine guest

### Sicurezza delle VPN

PPTP è considerato insicuro (compromesso dall'NSA; Microsoft raccomanda SSTP o L2TP). L2TP/IPSec è praticamente compromesso. OpenVPN e IKEv2 rimangono le opzioni più affidabili. Sono emerse vulnerabilità anche in dispositivi Cisco PIX usati per VPN IPSec (falla PIXPocket, sfruttabile tra il 2002 e il 2008).
