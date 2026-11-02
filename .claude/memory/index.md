# Snapshot del progetto

Branch: `main`. Commit di riferimento: primo commit del repository pubblico, da registrare dopo la pubblicazione. Data: 2026-11-02. Provenienza del sistema: `.claude/template-reference.json`, adozione selettiva.

## Stato delle schede

| Scheda | Riferimento | Stato |
|---|---|---|
| STACK.md | PENDING-FIRST-COMMIT | Riscritta sul codice pubblicato |
| design-and-security.md | PENDING-FIRST-COMMIT | Controlli, livello privato e limiti |
| deployment.md | PENDING-FIRST-COMMIT | Uso locale manuale e prove sintetiche |
| dev-testing.md | PENDING-FIRST-COMMIT | Prove sintetiche, compresa la rimozione della copia locale |
| current-work.md | PENDING-FIRST-COMMIT | Pubblicazione e prossimi lavori |
| roadmap.md | PENDING-FIRST-COMMIT | Priorità dello strumento |

## Punto di ripresa

Lo strumento è pubblicato come repository pubblico `teams-chat-evidence`, con licenza MIT e storia che parte dal commit iniziale (ADR-010). Il repository descrive lo strumento e non contiene dati degli incarichi, che vivono nel livello privato indicato nel file di ripresa (ADR-009). Prima di ogni commit si esegue il controllo di anonimizzazione con il file dei pattern privato.

Lavori aperti: strumenti parametrici di preparazione ed esecuzione di un incarico (P-001), decisione per incarico sul recupero degli allegati (P-002), riduzione dei permessi dell'applicazione Graph a quelli usati (P-003). Commit e push competono all'utente.
