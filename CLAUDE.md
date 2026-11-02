# teams-chat-evidence

Strumenti PowerShell e Python per acquisire una conversazione diretta Teams tramite Microsoft Graph, con verbale, archivio tecnico, manifesto e consegna verificata. Il README è la documentazione per chi usa lo strumento. Questo progetto adotta selettivamente il sistema del template: istruzioni condivise, memoria e contesto, regole di base, skill normative e strumenti locali di verifica. La provenienza e il perimetro sono in `.claude/template-reference.json`; non è dichiarata la copia integrale del bundle.

## Ripresa

Eseguire `python tools/sync-codex-skills.py --check` e `python tools/verifica-ripresa.py`. Leggere `.claude/memory/index.md`, poi `.claude/context/current-work.md` e le sole schede pertinenti. Leggere `.claude/rules/chat-non-e-memoria.md`, `.claude/rules/interaction-style.md` e `.claude/rules/git-commands-format.md` anche in Codex.

## Memoria e contesto

La memoria canonica è `.claude/memory/index.md`, con registro cronologico in `progress.md`, decisioni in `decisions.md` e pendenze in `pending.md`. Le schede tecniche sono `.claude/context/STACK.md`, `design-and-security.md`, `deployment.md`, `dev-testing.md`, `current-work.md` e `roadmap.md`. Aggiornare memoria e contesto nel giro in cui nasce il contenuto. Gli appunti privati e il prompt di ripresa vivono sotto `_notes/`, ignorato da git. Prima della consegna rileggere i contenuti scritti e dichiarare i file aggiornati. Lo standard di riferimento è `.claude/PROJECT-SYSTEM.md`.

## Regole del progetto

La finalità dell'estrazione e la base giuridica competono al titolare del trattamento. L'operatore IT esegue istruzioni documentate nel perimetro richiesto. Bozze, ruoli dirigenziali e permessi Graph non sostituiscono la verifica dei presupposti. Il percorso ordinario attende la firma. L'ordine esplicito dell'utente di procedere prima della firma si registra in un'istruzione privata separata: la richiesta resta `Status=Draft`, senza dichiarare verifiche o firme non acquisite. Decisione ADR-006.

`Export-TeamsConversation.ps1` è il percorso per le nuove estrazioni dirette. Senza parametri aggiuntivi controlla il fascicolo firmato e i presupposti prima delle credenziali. `-ExecutionInstructionPath` richiede invece l'ordine esplicito di esecuzione anticipata, collegato mediante hash al documento e alla richiesta; registra lo stato non firmato. `-EvidencePath` conserva risposte Graph e registro HTTP, senza token.

`Export-TeamsEvidence.ps1` riunisce recupero, verifiche sui messaggi, verbale PDF, archivio tecnico, manifesti e consegna con rilettura di cinque file. Lo stato della singola acquisizione è privato; codice, dipendenza PDF e prove sintetiche sono versionabili. Per uso e limiti leggere `docs/teams-evidence.md`.

Il progetto è uno strumento astratto e il repository è pubblico dal 2026-11-02, con storia che parte dal commit iniziale (ADR-010). I file tracciati descrivono lo strumento e mai un incarico: niente nomi di persone, identificativi del tenant, dell'applicazione o delle chat, indirizzi interni, nomi di condivisioni, periodi, conteggi o vicende di un'acquisizione reale, né nel codice né nella memoria (ADR-009). Dove serve un esempio si usano segnaposto fra parentesi angolari, secondo `.claude/rules/anonymization.md`. Contenuti estratti, documenti, ordini, strumenti di caso, registri degli incarichi, mappa dei segnaposto e pattern di anonimizzazione vivono nella cartella privata indicata dall'utente, il cui percorso sta solo in `_notes/RESUME-PROMPT.md`. Sul disco locale restano il codice, le credenziali in `config.local.json` e il file di ripresa senza dati delle richieste; `output/` esiste solo durante un'acquisizione e lo strumento lo rimuove dopo la consegna. Prima di ogni commit `python tools/Test-Anonymization.py --patterns <file dei pattern nella cartella privata>` deve uscire pulito, e un lavoro su un incarico aggiorna il registro privato, non la memoria tracciata. Le prove usano esclusivamente dati sintetici e HTTP simulato. Il CSV conserva UTC; le viste leggibili riportano ora italiana e offset, con entrambi i mittenti in ordine cronologico.

## Skill caricate quando servono

Per verificare il drift delle schede usare `sync-context`; per riprendere il lavoro `riprendi`; per una fotografia del repository `repo-status`. Per scrivere prove o aggiungere guardie leggere `prove-che-misurano` e il suo `RIFERIMENTO.md`. Se una fonte non è recuperabile leggere `fonti-non-recuperabili`. Se cambiano gli alberi di lavoro usare `alberi-di-lavoro`. Se cambia la separazione fra prove e uso reale usare `separazione-ambienti`. Per identità git e remoti usare `identita-git`.

## Chiusura

Eseguire le verifiche pertinenti e aggiornare `_notes/RESUME-PROMPT.md`; registrare l'impronta con `python tools/verifica-ripresa.py --registra`. Preparare `_notes/COMMIT-MSG.txt`, una riga di massimo 72 caratteri. Le operazioni git e i rilasci sono sempre manuali. L'identità git e gli account della macchina mantengono la configurazione esistente.

Norme caricate su richiesta, una riga per situazione con le parole con cui si presenta, così che il caricamento non dipenda dal ricordare che la norma esista.

- Va prodotto, modificato o convertito un artefatto visivo o documentale, cioè un documento, una presentazione, un PDF, un'immagine, un'animazione, un audio o un impaginato, oppure si sta per scrivere uno script o installare una libreria per farlo: skill `craft-apps`, che prima verifica in locale le applicazioni Craft installate e le usa senza rete né token.
