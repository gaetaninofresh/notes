# Windows Forensics

## NTFS - New Technology Filesystem
> Filesystem: struttura logica che consente l'indicizzazione di settori/celle dei dischi

Standard Windows, gestisce i dati di sistema attraverso file speciali indicati dal `$`:
- `$Boot`: Settore di avvio posizionato all'inizio del volume, contiene il BIOS Parameter Block (BPB) e la posizione della MFT.
- `$MFT`: Database contente record di tutti i file/cartelle. I record hanno dimensione 1024 byte.
- `$Bitmap`: Mappa di allocazione dei cluster, traccia quale memoria è libera/occupata in formato binario.
- `$LogFile`: file di logging, registra tutte le modifiche ai metadati.
- `$AttrDef`: elenca i tipi di attributo, include attributi standard come `$STANDARD_INFORMATION`.
- `$Secure`: file contenente i descrittori di sicurezza, ACL e autorizzazioni per utenti e gruppi di volume.
- `$UsnJrnl`: Giornale delle modifiche che registra operazioni su file e directory.
- `$Volume`: contienen informazioni sul volume logico come serial number e label.
- `$Data`: contiene info sulle cartelle e i dati effettivi (residente o non residente).

### Organizzazione del disco
NTFS divide i dischi in cluster composti da settori.
La ricerca di un file fa riferimento a `$Bitmap` per trovare settori contigui liberi, segna le modifiche in `$MFT` e le logga in `$LogFile`.

L'attributo `$STANDARD_INFORMATION` è modificabile da utente, mentre `$FILE_NAME` non è alterabile dall'utente ed è gestito dal kernel.
Hanno quattro timestamp (MACE):
- **Modified**: Ultima modifica del contenuto.
- **Accessed**: Data di ultimo accesso (potrebbe essere alterata da operazioni di sistema, disattivato da Microsoft per motivi di performance).
- **Created**: Data di creazione nel volume.
- **Entry Modified**: Ultima modifica del record MFT.

> **Considerazione forense (Timestamping)**: Discrepanze tra `$STANDARD_INFORMATION` e `$FILE_NAME` indicano alterazioni. L'attributo `$FILE_NAME` garantisce maggiore affidabilità temporale.

## Cancellazione di file
Al momento della cancellazione:
- Viene aggiornata l'entry MFT marcandola come inutilizzata.
- I cluster/settori vengono marcati come liberi in `$Bitmap`.

> **Considerazione forense**: È quindi possibile recuperare i file finché la voce MFT e i cluster non vengono sovrascritti.

### Cestino
Il cestino corrisponde ai file NTFS in `\$Recycle.Bin\<SID>` o `\RECYCLER\<SID>` che conserva il record `$I[FILENAME]` (metadati e data cancellazione) e `$R[FILENAME]` (contenuto).

## Allocazione
Settori e cluster vengono allocati secondo necessità, se un cluster è in uso in maniera parziale lo spazio restante viene comunque considerato come allocato.

> **Slackspace**: spazio rimanente in un cluster che contiene un file. Differenziamo:
> - **RAM slack**: parte che va dalla fine del file alla fine del settore (precedentemente usato come swap space). Oggi è tipicamente azzerato con padding di zeri.
> - **Drive slack**: dalla fine dell'ultimo settore occupato fino alla fine del cluster. Non viene mai azzerato e offre materiale prezioso per il carving di frammenti persistenti.

## Carving
Tecnica di recupero
- **File carving**: usa header/footer per ricostruire il file ignorando il file system.
- **Data carving**: estrazione di dati da file (ignorando header/footer) per recuperare stringhe/dati strutturati da frammenti di cluster.

### Windows Shadow Copy - VSS
VSS crea snapshot di file/volumi al fine di garantire backup.
> **Considerazione forense**: Utilizzabili per recuperare versioni precedenti del Registro, web cache e file storici eliminati.

# Registro Windows
Database gerarchico (albero), ogni chiave è un nodo con figli, contiene configurazioni del sistema operativo, info sui componenti hardware, storia degli utenti e delle loro operazioni.

Il registro è diviso in hive, alcuni rilevanti:
- **SAM**: Hash NTLM, account locali, informazioni sui login.
- **SYSTEM**: Configurazione hardware, fuso orario.
- **SOFTWARE**: Applicazioni installate sul sistema.
- **NTUSER.DAT**: Artefatti e cronologia specifici per l'utente locale.
- **UsrClass.dat**: Usato per ShellBags e metriche di visualizzazione cartelle.

Ogni hive ha dei transaction log associati.

> **Considerazione forense (USBSTOR)**: La chiave USBSTOR (nel file SYSTEM) è essenziale per identificare numero di serie, vendor e i timestamp di primo e ultimo collegamento dei dispositivi USB connessi al PC.

### Artefatti di esecuzione e Navigazione
#### RecentDocs
Contiene i record degli ultimi 150 file aperti. Classifica i file per estensione. Viene conservato il record dell'ultima apertura (unreliable, parzialmente ricostruibile tramite cross-referencing).

#### Shellbags
Contiene preferenze utente di visualizzazione cartelle.
> **Considerazione forense**: Provano storicamente l'interazione con directory o file ZIP, anche su drive esterni e di rete attualmente disconnessi.

#### UserAssist
Contiene i record delle applicazioni GUI utilizzate dall'utente. Contiene un set molto ricco di dati ed è tra i più utili da consultare in contesti di informatica forense.
> **Considerazione forense**: Rivela contatori di esecuzione (run count) e tempi di permanenza in primo piano (focus time).

#### Prefetch e LNK Files
- **Prefetch (`.pf`)**: ottimizzano il caricamento app ma storicizzano l'esecuzione dei programmi eseguiti sul sistema.
- **LNK Files / Jump List**: file collegamento generati automaticamente e storici pinnati.
  > **Considerazione forense (Regola LNK)**: La creazione di un file LNK corrisponde alla prima apertura documentata del file originale; la sua data di modifica equivale all'ultimo accesso.

#### Browser Forensics
I database dei browser (SQLite) tracciano cronologia, cookie e cache.
> **Considerazione forense (Incognito)**: La modalità privata non memorizza dati in persistenza sui DB, ma fragmenti di cache (leak) possono essere recuperati nella RAM (memory carving), nel Prefetch e nei file LNK scaricati.
