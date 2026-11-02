# teams-chat-evidence

Strumenti PowerShell e Python per acquisire una conversazione diretta di Microsoft Teams tramite Microsoft Graph e consegnarla con verbale PDF, archivio tecnico, manifesto SHA-256 e verifica file per file della copia consegnata.

Sono pensati per chi gestisce i sistemi informativi e riceve dal titolare del trattamento l'incarico documentato di estrarre una conversazione. I controlli scattano prima che il programma legga le credenziali o tocchi un messaggio. Il verbale dichiara anche ciò che il programma non può attestare. Il programma non decide se l'estrazione sia lecita: la finalità e la base giuridica restano del titolare, e il codice lo impone invece di presumerlo.

## Che cosa produce

Ogni acquisizione produce una cartella di cinque file.

| File | Contenuto |
|---|---|
| `Conversazione.html` | Vista leggibile e stampabile: entrambi i mittenti in ordine cronologico, ora italiana con offset, riferimenti alle risposte, testo con escape HTML |
| `Conversazione.csv` | Gli stessi messaggi in forma tabellare, con istanti UTC e identificativi degli autori |
| `Acquisizione_tecnica.zip` | Risposte Graph della sola conversazione, registro delle richieste senza token, richiesta e istruzione, versione del codice usato, verifiche e manifesto interno |
| `Verbale_acquisizione.pdf` | Provenienza, periodo, operatore, strumenti, controlli eseguiti, riscontro della consegna e limiti dichiarati |
| `SHA256.csv` | Impronte dei quattro file precedenti |

Il programma restituisce anche l'impronta di `SHA256.csv`. Va conservata separatamente dal pacchetto: è il riferimento indipendente che rileva una modifica fatta insieme ai file e al manifesto.

## Come funziona

```
richiesta JSON (Draft)        il programma si ferma qui finché l'incarico non è firmato
  │                           oppure finché non esiste un'istruzione esplicita vincolata per hash
  ▼
controlli sulla richiesta     campi completi, nessun segnaposto, periodo con fuso, documento
  │                           firmato presente e con l'impronta dichiarata
  ▼
credenziali e Graph           risoluzione dei due utenti, ricerca della sola chat diretta
  │                           con esattamente quei due membri, messaggi del solo periodo
  ▼
viste e verifiche             ogni messaggio confrontato fra risposta Graph, copia tecnica,
  │                           CSV e HTML: identificativi, autori, testi, istanti, ordine
  ▼
pacchetto                     archivio tecnico, verbale PDF, manifesto
  │
  ▼
consegna                      copia nella cartella di destinazione, rilettura dei cinque file
  │                           e confronto delle impronte con gli originali
  ▼
pulizia locale                la copia locale viene sovrascritta e cancellata: il materiale
                              resta solo nella destinazione indicata dal richiedente
```

Le chat di gruppo, i canali e le conversazioni con altre persone vengono scartati per costruzione. Il recupero conserva i messaggi di entrambi i partecipanti e non filtra per mittente. Gli allegati restano riferimenti e non vengono scaricati: per inventariarli c'è uno strumento a parte.

## Requisiti

- Windows con PowerShell 5.1 o successivo.
- Python 3.9 o successivo, con PyMuPDF per il verbale: `python -m pip install -r requirements-evidence.txt`.
- Un'applicazione registrata in Microsoft Entra ID con permessi di tipo *Application* e consenso amministrativo.

| Permesso | Perché serve |
|---|---|
| `User.Read.All` | Risolvere i due partecipanti dal nome |
| `Chat.Read.All` | Elencare le chat dell'utente con i membri e leggere i messaggi |
| `Files.Read.All` | Solo per scaricare gli allegati con `tools/teams_attachments.py --scarica`; senza, l'inventario funziona e il download riporta 403 |

Il programma non chiede altri permessi. Se l'applicazione ne ha di più, conviene toglierli.

## Configurazione

Le credenziali stanno in `config.local.json`, accanto agli script e ignorato da git. `config.json` è il modello con i soli segnaposto.

```json
{
    "TenantId":     "<TENANT_ID>",
    "ClientId":     "<CLIENT_ID>",
    "ClientSecret": "<CLIENT_SECRET>"
}
```

L'incarico si descrive in un JSON di richiesta. Il modello è `export-request.example.json`: copiarlo fuori dal repository, oppure sotto `_notes/`, che git ignora. La richiesta nasce con `Status` uguale a `Draft`, e finché resta così il percorso ordinario si ferma.

| Campo | Significato |
|---|---|
| `Participant1`, `Participant2` | I due partecipanti, come nome visualizzato. Il nome si risolve anche in ordine cognome-nome; un'omonimia interrompe l'operazione |
| `StartDate`, `EndDateExclusive` | Periodo autorizzato, con inizio incluso e fine esclusa. Il fuso è obbligatorio, per esempio `2025-01-01T00:00:00+01:00` |
| `Purpose`, `LegalBasis` | Finalità e base giuridica, dichiarate dal titolare |
| `Recipients`, `RetentionUntil` | Destinatari della consegna e termine di conservazione, che deve essere futuro |
| `ApprovalReference`, `AuthorizationDate` | Riferimento e data dell'incarico |
| `SignedAuthorizationPath`, `SignedAuthorizationSha256` | Documento firmato e sua impronta: il programma la ricalcola e si ferma se non coincide |
| `PrerequisitesReviewed` | Booleano `true` solo dopo la verifica dei presupposti; una stringa non basta |
| `Status` | `Approved` per il percorso ordinario |

Un campo vuoto, o che contiene ancora un segnaposto fra parentesi angolari, ferma il programma prima delle credenziali. Il modello del documento da far firmare, con le istruzioni accanto a ogni campo, è [docs/Fascicolo-export.template.txt](docs/Fascicolo-export.template.txt).

## Uso

Il percorso ordinario richiede la richiesta approvata e il documento firmato:

```powershell
.\Export-TeamsEvidence.ps1 -RequestPath <richiesta.json> -DestinationRoot '\\<server>\<cartella>'
```

`-DestinationRoot` è la cartella, locale o di rete, in cui il richiedente vuole il materiale. Il programma vi crea una sottocartella nuova, rifiuta di sovrascrivere e, dopo aver verificato la consegna, rimuove la copia locale. `-ConservaCopiaLocale` la tiene, `-OutputPath` sceglie dove prepararla. Senza destinazione la copia locale è l'unico esito e resta.

### Ordine di procedere prima della firma

Se il titolare ordina per iscritto di procedere prima di sottoscrivere, l'ordine si registra in un'istruzione separata. L'istruzione è vincolata per impronta al documento da firmare e alla richiesta, che resta `Draft`. Il verbale riporta che il documento non è firmato: nessuna firma, approvazione o verifica giuridica viene simulata.

```json
{
  "Status": "ExecuteBeforeSignature",
  "Instruction": "<testo esatto dell'ordine ricevuto>",
  "RecordedAtUtc": "<data di registrazione UTC con offset>",
  "DocumentPath": "<percorso del documento da sottoscrivere>",
  "DocumentSha256": "<SHA-256 del documento>",
  "RequestSha256": "<SHA-256 del JSON della richiesta>"
}
```

```powershell
.\Export-TeamsEvidence.ps1 -RequestPath <richiesta.json> -ExecutionInstructionPath <istruzione.json> -DestinationRoot '\\<server>\<cartella>'
```

Una modifica successiva alla richiesta o al documento invalida il collegamento, e il programma si ferma.

### Verifica indipendente di un pacchetto

```powershell
python tools/teams_evidence.py verify --output '<cartella del pacchetto>' --expected-manifest-sha256 '<impronta conservata>'
```

### Inventario degli allegati

```powershell
python tools/teams_attachments.py --pacchetto '<cartella consegnata>' --output '<cartella nuova>' --anno 2025 --mesi 1,2,3
```

Legge l'archivio consegnato, non una copia locale, ed elenca nome, data, mittente e collegamento di ogni file allegato nei mesi indicati. Le citazioni di altri messaggi, che Graph presenta come allegati, sono contate a parte. Con `--scarica` e `--config` tenta il download e registra byte e impronta di ogni file, senza aprirne il contenuto.

### Pulizia del disco locale

```powershell
.\tools\Remove-LocalEvidence.ps1
.\tools\Remove-LocalEvidence.ps1 -Destinazione '\\<server>\<cartella>' -Preserva
.\tools\Remove-LocalEvidence.ps1 -Destinazione '\\<server>\<cartella>' -Distruggi
```

Senza parametri elenca il materiale rimasto sotto `output/` e `_notes/` e non tocca niente. `-Preserva` copia sulla destinazione i documenti da firmare e ne verifica le impronte. `-Distruggi` sovrascrive, rinomina e cancella. Prima scrive sulla destinazione il registro di ciò che sta per rimuovere, poi lo chiude con l'esito. Si rifiuta di procedere se la destinazione non è raggiungibile.

## Che cosa il programma non fa

- Non appone firme digitali né marche temporali qualificate. Le impronte attestano la corrispondenza dei file, non la data certa né il valore probatorio, che restano da valutare nel caso concreto.
- Non ricostruisce messaggi cancellati, versioni precedenti o contenuti che Graph non restituisce. La lettura non congela il servizio: una modifica concorrente può far fallire i controlli.
- Non scarica allegati né immagini durante l'acquisizione.
- Non verifica permessi, cifratura o backup della cartella di destinazione.
- La sovrascrittura prima della cancellazione non garantisce che il dato sia irrecuperabile su un'unità a stato solido. Il programma lo dichiara a ogni esecuzione.
- Non invia i contenuti a servizi esterni diversi da Microsoft Graph, e il token raggiunge solo gli endpoint Graph previsti.

I presupposti giuridici italiani, cioè GDPR, Codice privacy e Statuto dei lavoratori, con le fonti e i limiti dell'analisi, sono in [docs/privacy-export.md](docs/privacy-export.md). Il flusso tecnico completo è in [docs/teams-evidence.md](docs/teams-evidence.md). Né l'uno né l'altro sono un parere legale.

## Prove

Le prove usano solo dati sintetici e una funzione HTTP simulata che rifiuta qualunque URL non previsto. Non leggono credenziali reali né conversazioni reali.

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File tests/Test-TeamsConversation.ps1
powershell -NoProfile -ExecutionPolicy Bypass -File tests/Test-TeamsEvidence.ps1
```

Le prove coprono:
- i blocchi su bozza e campi incompleti prima delle credenziali;
- il periodo con inizio incluso e fine esclusa, e il cambio dell'ora legale;
- la paginazione, i duplicati e l'esclusione di gruppi e altre chat;
- l'escape HTML e il mittente senza nome;
- il PDF, l'archivio e la consegna;
- il divieto di sovrascrittura;
- il rifiuto di un CSV manomesso;
- la rimozione della copia locale dopo la consegna.

Alcune asserzioni sono state verificate anche su copie mutate del codice, per controllare che falliscano quando la guardia manca.

## Struttura

```
Export-TeamsEvidence.ps1        orchestratore: acquisizione, verifiche, pacchetto, consegna, pulizia
Export-TeamsConversation.ps1    recupero Graph della conversazione diretta e viste
tools/
  teams_evidence.py             verificatore, verbale PDF, archivio e manifesti
  teams_attachments.py          inventario ed eventuale recupero degli allegati
  Remove-LocalEvidence.ps1      elenco, preservazione e distruzione del materiale locale
  Test-Anonymization.py         controllo che i file pubblicabili non contengano dati identificanti
tests/                          prove sintetiche
docs/                           flusso tecnico, presupposti privacy, modello di incarico
config.json                     modello delle credenziali
export-request.example.json     modello della richiesta
```

Le cartelle `.claude/` e `.agents/`, `CLAUDE.md` e `AGENTS.md` contengono le istruzioni, le regole e la memoria di progetto usate dagli agenti di sviluppo (Claude Code e Codex). Vengono da un template di sistema di progetto e non servono per usare gli strumenti. La memoria descrive lo strumento, non le singole acquisizioni: i dati degli incarichi non entrano mai nel repository.

## Licenza

[MIT](LICENSE).
