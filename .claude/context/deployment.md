---
generated-from-commit: PENDING-FIRST-COMMIT
generated-from-branch: main
generated-date: 2026-11-02
covers-paths:
  - '*.ps1'
  - '.gitignore'
  - 'tests/**'
  - 'tools/teams_evidence.py'
  - 'requirements-evidence.txt'
last-verified-commit: PENDING-FIRST-COMMIT
---

# Uso reale e ambienti

## Modello di separazione

Il progetto è un insieme di strumenti locali Windows, senza server applicativo, pipeline di deploy o ambiente di staging. Sorgenti in un solo repository, esecuzione manuale, configurazione reale privata. Le prove usano dati sintetici e HTTP simulato e non accedono all'applicazione Graph reale né a conversazioni reali (ADR-004).

## Esecuzione

Richiesta, documento firmato ed eventuale istruzione stanno nel livello privato. Il comando ordinario è:

```powershell
.\Export-TeamsEvidence.ps1 -RequestPath <richiesta.json> -DestinationRoot '\\<server>\<cartella>'
```

Ogni esecuzione usa una cartella nuova; una cartella già esistente viene rifiutata prima di qualunque richiesta HTTP. Le dipendenze si verificano prima di Graph. Un errore interrompe il flusso, lascia il registro di lavoro con stato `Failed` e non viene dichiarato consegnato. Il titolare predispone cifratura, accessi e custodia della destinazione prima di procedere. La verifica offline di un pacchetto si esegue con `tools/teams_evidence.py verify`, eventualmente confrontando l'impronta del manifesto conservata separatamente.
