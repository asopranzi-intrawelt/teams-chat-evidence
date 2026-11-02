---
generated-from-commit: 664c4f09bc5585ad32efe445f730ac08c53fa5ca
generated-from-branch: main
generated-date: 2026-11-02
covers-paths:
  - '*.ps1'
  - '.gitignore'
  - 'docs/**'
  - 'export-request.example.json'
  - 'tools/teams_evidence.py'
  - 'tools/teams_attachments.py'
  - 'tools/Remove-LocalEvidence.ps1'
last-verified-commit: 664c4f09bc5585ad32efe445f730ac08c53fa5ca
---

# Design e sicurezza

Le istruzioni dell'operatore restano separate dalla liceità del trattamento. Il percorso ordinario richiede il documento firmato e i presupposti registrati (ADR-002). L'esecuzione anticipata richiede un ordine scritto separato, vincolato mediante hash al documento e alla richiesta; la richiesta resta `Draft` e nessuna firma o valutazione viene simulata (ADR-006). L'eccezione documenta la decisione tecnica e non certifica la liceità.

Il recupero seleziona le sole chat dirette con esattamente i due membri richiesti e conserva i messaggi del solo periodo approvato, di entrambi i mittenti. Le viste leggibili conservano contesto e timestamp; il corpo originale resta nella copia tecnica. I contenuti non vengono inviati a servizi diversi da Graph.

Il tool verifica ogni messaggio fra risposte Graph, copia tecnica, CSV e HTML e conserva codice e manifesti nell'archivio. Un duplicato discordante interrompe la preparazione. La consegna rilegge i file dalla destinazione e ne controlla le impronte; il confronto con un'impronta del manifesto conservata separatamente è disponibile nel verificatore. Allegati, versioni precedenti e cancellazioni non vengono acquisiti; i tempi locali non sono una marcatura qualificata.

Il materiale acquisito vive solo nella destinazione indicata dal richiedente. Dopo una consegna verificata il tool sovrascrive e cancella la copia locale, e `tools/Remove-LocalEvidence.ps1` fa lo stesso per ciò che resta, scrivendo il registro sulla destinazione prima di cancellare e rifiutando di procedere se la destinazione non è raggiungibile. Su unità a stato solido la sovrascrittura non attesta l'irrecuperabilità fisica, e gli strumenti lo dichiarano.

I file tracciati non contengono dati degli incarichi (ADR-009). `output/`, `_notes/` e `config.local.json` sono ignorati da git; l'esclusione non cifra i file e non definisce i permessi d'accesso, che restano presupposti operativi del titolare. Il modello e i limiti della valutazione giuridica sono in `docs/privacy-export.md`; nessuna certificazione di conformità viene promessa.
