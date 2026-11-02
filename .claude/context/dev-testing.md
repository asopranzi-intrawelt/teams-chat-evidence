---
generated-from-commit: 664c4f09bc5585ad32efe445f730ac08c53fa5ca
generated-from-branch: main
generated-date: 2026-11-02
covers-paths:
  - 'tests/**'
  - 'Export-TeamsConversation.ps1'
  - 'Export-TeamsEvidence.ps1'
  - 'requirements-evidence.txt'
  - 'tools/**'
last-verified-commit: 664c4f09bc5585ad32efe445f730ac08c53fa5ca
---

# Verifiche

Le prove sono offline, con nomi e credenziali sintetici e una funzione HTTP sostitutiva che rifiuta qualunque URL non previsto. Creano soltanto cartelle sotto `_notes/tests/`, ignorato da git. Nessuna conversazione reale è usata come fixture.

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File tests/Test-TeamsConversation.ps1
powershell -NoProfile -ExecutionPolicy Bypass -File tests/Test-TeamsEvidence.ps1
```

La prova dell'esportatore controlla:

- prima della configurazione: i blocchi sulla bozza e sui campi incompleti, la verifica del documento e della sua impronta, la revisione preliminare che deve essere un booleano vero e non una stringa, i periodi senza fuso;
- sul recupero: l'ordine cronologico, entrambi gli autori, la paginazione e la deduplicazione, l'esclusione di gruppi e altre chat, il periodo con inizio incluso e fine esclusa, l'arresto del recupero;
- sulle viste: l'ora italiana e il cambio di ora legale, i paragrafi e l'escape HTML, il riferimento alla risposta, le impronte dei file.

Le fixture producono quattro messaggi inclusi, e i dati fuori periodo restano esclusi anche dalla copia tecnica. Uno dei messaggi non ha il nome del mittente nella risposta: l'esportatore lo dichiara «(mittente non disponibile)», conserva l'identificativo dell'autore e conta i due interlocutori per identificativo (ADR-008).

La prova del tool completo esercita:

- i percorsi ordinario e anticipato;
- la generazione del PDF, dell'archivio e dei manifesti;
- la consegna dei cinque file e il divieto di sovrascrittura prima di HTTP;
- il rifiuto di un CSV modificato, con il ripristino controllato mediante hash;
- il confronto del manifesto con un'impronta indipendente;
- il conteggio dei messaggi senza nome del mittente in `verifiche.json`.

La corsa principale passa `-ConservaCopiaLocale`, perché le prove di manomissione lavorano sulla copia locale. Una corsa dedicata verifica il comportamento predefinito: dopo la consegna verificata la copia locale e la cartella di lavoro non esistono più, e la destinazione conserva i cinque file. Con un tool mutato che non rimuove, la prova cade su quell'asserzione.

Mutazioni verificate: la prova dell'esportatore cade se si rimuove il blocco sulla bozza; quella del tool completo cade sulle copie che omettono il confronto della richiesta con l'istruzione, la verifica del checksum esterno e la rimozione della copia locale.

Per la documentazione e il sistema di progetto si eseguono unwrap, controlli delle tabelle e dei comandi e il controllo di anonimizzazione; per i PowerShell anche il parser.
