# DHCP
Il DHCP offre in broadcast l'inidirizzo IP da assegnare a client che si connettono alla rete.
Step DHCP:
...

# Vulnerabilità DHCP
Essendo l'unica forma identificativa il MAC in assenza di forme di autenticazione il server DHCP è vulnerabile ad attacchi di starvation (migliaia di richieste con MAC spoofati).
In combinazione l'attacante può istituire un server DHCP sotto il proprio controllo e sostituirsi all'originale. In questo modo può imporre configurazioni di rete malevole ie - forzare un DNS sotto suo controllo.

# Misure preventive
## DHCP Snooping
Filtro per in messaggi DHCP, si isitutisce un DHCP snooping binding database (MAC, IP, lease time, binding type, VLAN number, port ID).
Si limitano le porte autorizzate ad originare messaggi di offerta DHCP, in caso di DHCPOFFER da porte untrusted:
* Viene chiusa la porta
* Si usa DHCP option 82

#
