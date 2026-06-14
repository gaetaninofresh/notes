> **Esame.**
>
> Domande + esercizi (carta) - 1.30h <br>
> Replica lab (proprio pc, open book) - 2h

# DNS Rebinding
Attacco che sfrutta la **same-origin policy**: permettiamo ad applet di scaricare contenuti esclusivamente da indirizzi che fanno parte del suo dominio (?).
Un attaccante potrebbe:
1. Associare un dominio ad un indirizzo da lui controllato 
2. Il client scarica l'applet dal dominio controllato dall'attaccante
3. L'attaccante rebinda il proprio dominio ad un ip interno della vittima (TTL entry DNS 0)
4. L'applet apre una connessione verso l'indirizzo interno della vittima
5. L'attaccante ottiene accesso ad indirizzi interni della vittima

Una misura preventiva è il **pinning del DNS** rifiutando il trasferimento ad un nuovo IP (potrebbe rompere VPN, proxy, ...). 
DNSSEC impedisce completamente questo tipo di attacco grazie alla firma digitale delle richieste/risposte DNS.
Introduce inoltre i **Resource Records** [...]
[IMMAGINE DNNSEC RECORD]



