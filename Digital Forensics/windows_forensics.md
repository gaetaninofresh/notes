# Windows Forensics

## NTFS - New  Technology Filesystem
> Filesystem: struttura logica che consente l'indicizzazione di settori/celle dei dischi

Standard Windows, gestisce i dati di sistema attraverso file speciali indicati dal $:
- `$Boot`: Settore di avvio posizionato all'inizio del volume, contiene 
- `$MFT`: Database contente record di tutti i file/cartelle (). I record hanno dimensione 1024 byte.  
- `$Bitmap`: Mappa di allocazione dei cluster, traccia quale memoria è libera/occupata.
- `$LogFile`: file di logging, registra tutte le modifiche ai metadati
- `$AttrDef`: elenca i tipi di attributo, include attributi standard come ...
- `$Secure`: file contenente i descrittori di sicurezza, ACL e autorizzazioni per utenti e gruppi di volume
- `$UsrJrnl`: ...
- `$Volume`: contienen informazioni sul volume ...
- `$Data`: contiene info sulle cartelle [?]
### Organizzazione del disco
NTFS divide i dischi in cluster (default ) composti da settori (default ).
La ricerca di un file fa riferimento a `$Bitmap` per trovare settori contigui liberi, segna le modifiche in `$MFT` e le logga in `$LogFile`.

...

FILEINFORMATION è modificabile da utente, FILENAME non è alterabile dall'utente.
Hanno quattro timestamp (MACE):
- **Modified**: Ultima modifica
- **Accessed**: Data di ultimo accesso (potrebbe essere alterata da operazioni di sistema, disattivato da Microsoft per motivi di performance)
- **Created**: Data di creazione
- **Entry Modified**: Ultima modifica del record MFT 

## Cancellazione di file
Al momento della cancellazione:
- Viene aggiornata l'entry MFT marcandola come inutilizzata
- I cluster/settori vengono marcati come liberi

è quindi possibile recuperare i file finchè la voce MFT e i cluster non vengonon sovrascritti.

### Cestino
Il cestino corrisponde ai file NTFS `` che conserva il record `$I[FILENAME]` e `$I[FILENAME]` ...

## Allocazione
Settori e cluster vengono allocati secondo necessità, se un cluster è in uso in maniera parziale lo spazio restante viene comunque considerato come allocato.


> **Slackspace**: spazio rimanente in un cluster che contiene un file 
> Differenziamo 
> - RAM slack: parte che va dalla fine del file alla fine del settore (precedentemente usato come swap space)
> - Drive slack: dalla fine dell'ultimo settore occupato fino alla fine del cluster

## Carving
Tecnica di recupero
- File carving: usa header/footer per ricostruire il file
- Data carving: estrazione di dati da file (ignorando header/footer) per reuperare stringhe/dati strutturati/... 

### Windows Shadow Copy - VSS
VSS crea snapshot  di file/volumi al fine di garantire backup.

# Registro Windows
Database gerarchico (albero), ogni chiave è un nodo con figli, contiene configurazioni del sistema operativo, info sui componenti hardware, storia degli utenti e delle loro operazioni.

Il registro è divisio in hive, alcuni rilevanti:
[TABELLA HIVE]

Ogni hive ha dei transaction log associati.

#### RecentDocs
Contiene i record degli ultimi 150 file aperti. Classifica i file per estensione. Viene conservato il record dell'ulltima apertura (unreliable, parzialmente ricostruibile tramite cross-referencing ).

#### Shellbags
Contiene ...

#### UserAssist
Contiene i record delle applicazioni GUI utilizzate dall'utente. Contiene un set molto ricco di dati ed è tra i più utili da consultare in contesti di informatica forense.



