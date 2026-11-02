# Snapshot del progetto

Branch: `main`. Commit di riferimento: `664c4f0`, commit iniziale del repository pubblico. Data: 2026-11-02. Provenienza del sistema: `.claude/template-reference.json`, adozione selettiva.

## Stato delle schede

| Scheda | Riferimento | Stato |
|---|---|---|
| STACK.md | 664c4f0 | Riscritta sul codice pubblicato |
| design-and-security.md | 664c4f0 | Controlli, livello privato e limiti |
| deployment.md | 664c4f0 | Uso locale manuale e prove sintetiche |
| dev-testing.md | 664c4f0 | Prove sintetiche, compresa la rimozione della copia locale |
| current-work.md | 664c4f0 | Pubblicazione e prossimi lavori |
| roadmap.md | 664c4f0 | Priorità dello strumento |

## Punto di ripresa

Lo strumento è pubblicato come repository pubblico `teams-chat-evidence`, con licenza MIT e storia che parte dal commit iniziale (ADR-010). Il repository descrive lo strumento e non contiene dati degli incarichi, che vivono nel livello privato indicato nel file di ripresa (ADR-009). Prima di ogni commit si esegue il controllo di anonimizzazione con il file dei pattern privato.

Lavori aperti: strumenti parametrici di preparazione ed esecuzione di un incarico (P-001), decisione per incarico sul recupero degli allegati (P-002), riduzione dei permessi dell'applicazione Graph a quelli usati (P-003). Commit e push competono all'utente.
