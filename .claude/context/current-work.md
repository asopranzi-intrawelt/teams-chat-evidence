---
generated-from-commit: PENDING-FIRST-COMMIT
generated-from-branch: main
generated-date: 2026-11-02
covers-paths:
  - '*.ps1'
  - 'docs/**'
  - 'tests/**'
  - 'export-request.example.json'
  - 'tools/teams_evidence.py'
  - 'tools/teams_attachments.py'
  - 'tools/Remove-LocalEvidence.ps1'
  - 'requirements-evidence.txt'
  - 'README.md'
last-verified-commit: PENDING-FIRST-COMMIT
---

# Lavoro corrente

Il 2026-11-02 lo strumento viene pubblicato come repository pubblico `teams-chat-evidence`, con licenza MIT e storia che parte dal commit iniziale (ADR-010). Il README spiega a un lettore esterno che cosa il programma produce, quali controlli impone, quali permessi chiede e che cosa non attesta. Gli script di esportazione generica precedenti sono rimossi.

Memoria e schede descrivono lo strumento e non gli incarichi (ADR-009). Il controllo `tools/Test-Anonymization.py`, con il file dei pattern del livello privato, si esegue prima di ogni commit e deve uscire pulito sulle categorie bloccanti.

Il prossimo lavoro tecnico è P-001: la preparazione del documento da firmare, della richiesta e dell'istruzione, e il lancio dell'acquisizione con l'inventario degli allegati, oggi affidati a script privati, diventano strumenti parametrici del repository con prove sintetiche.
