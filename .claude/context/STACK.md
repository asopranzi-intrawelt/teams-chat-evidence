---
generated-from-commit: PENDING-FIRST-COMMIT
generated-from-branch: main
generated-date: 2026-11-02
covers-paths:
  - '*.ps1'
  - 'config.json'
  - 'export-request.example.json'
  - 'tests/**'
  - 'tools/teams_evidence.py'
  - 'tools/teams_attachments.py'
  - 'tools/Remove-LocalEvidence.ps1'
  - 'requirements-evidence.txt'
last-verified-commit: PENDING-FIRST-COMMIT
---

# Stack applicativo

Gli script usano Windows PowerShell 5.1, `Invoke-RestMethod` e Microsoft Graph v1.0 con client credentials, senza il modulo Microsoft.Graph. Il token resta in memoria e viene inviato solo agli endpoint HTTPS di Graph previsti. `config.json` è il modello; il file compilato è `config.local.json`, ignorato da git. I permessi applicativi usati sono `User.Read.All` e `Chat.Read.All`; `Files.Read.All` serve solo al recupero degli allegati.

`Export-TeamsConversation.ps1` esegue il recupero di una conversazione diretta. Verifica la richiesta prima delle credenziali, risolve i due utenti anche con l'ordine nome-cognome invertito, pagina le chat con i membri espansi e verifica i membri completi prima di leggere i messaggi. Legge i messaggi per creazione decrescente e interrompe il recupero prima dell'inizio del periodo; gli artefatti sono ordinati in senso crescente. Produce HTML con testo escaped, TXT, CSV, copia tecnica JSON, riepilogo e impronte; con `-EvidencePath` conserva risposte Graph e registro HTTP senza token.

`Export-TeamsEvidence.ps1` è l'orchestratore. Chiama l'esportatore e poi `tools/teams_evidence.py` per confronto dei messaggi, archivio tecnico, verbale PDF e manifesti. Consegna con `-DestinationRoot`, rilegge i cinque file dalla destinazione e, se la consegna è verificata, sovrascrive e cancella copia locale e cartella di lavoro, salvo `-ConservaCopiaLocale`. Restituisce percorsi, conteggio, impronta del manifesto ed esito della rimozione.

`tools/teams_evidence.py` richiede Python 3.9+ e PyMuPDF 1.25.2+, dichiarati in `requirements-evidence.txt`. `tools/teams_attachments.py` usa la sola libreria standard: legge `Acquisizione_tecnica.zip` del pacchetto consegnato, inventaria gli allegati per mese locale e, con `--scarica`, li recupera tramite l'endpoint `shares` di Graph. `tools/Remove-LocalEvidence.ps1` elenca, preserva e distrugge il materiale rimasto sotto `output/` e `_notes/`.

Gli altri strumenti Python sotto `tools/` vengono dal template e servono a memoria, controlli documentali e anonimizzazione; non sono necessari all'acquisizione.
