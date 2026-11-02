# Decisioni

Registro riaperto il 2026-11-02 con la pubblicazione dello strumento. Le decisioni prese sui singoli incarichi stanno nel livello privato del progetto.

## ADR-010 - Pubblicazione come repository pubblico con storia nuova

Data: 2026-11-02. Lo strumento si pubblica come `teams-chat-evidence`, pubblico, con licenza MIT intestata all'autore. Il repository riparte da un solo commit iniziale e non eredita storia, perché la memoria precedente raccontava i singoli incarichi e una storia pubblica conserva ogni versione. Gli script di esportazione generica precedenti al percorso con verbale sono rimossi, perché non applicavano i controlli su incarico, firma e istruzione; il README dichiara i soli permessi Graph che il percorso rimasto usa.

## ADR-009 - Strumento astratto, livello privato fuori dal repository e fuori dal disco locale

Data: 2026-11-02. Direttiva dell'utente: anonimizzazione e conformità ISO 27001 e GDPR sono prioritarie, e sul disco locale non restano tracce delle richieste. I file tracciati descrivono lo strumento e non contengono nomi di persone, identificativi del tenant, dell'applicazione o delle chat, indirizzi interni, nomi di condivisioni né il racconto degli incarichi; dove serve un esempio si usano segnaposto fra parentesi angolari. Ordini, documenti, strumenti di caso, registri, mappa dei segnaposto e file dei pattern del controllo vivono in una cartella privata indicata dall'utente, il cui percorso sta solo nel file di ripresa ignorato da git. Sul disco locale restano il codice, le credenziali in `config.local.json` e il file di ripresa; `output/` esiste solo durante un'acquisizione. Prima di ogni commit `tools/Test-Anonymization.py --patterns <file dei pattern privato>` deve uscire pulito.

## ADR-008 - Un messaggio senza nome del mittente si dichiara, non si ricostruisce

Data: 2026-10-12. Graph può restituire `displayName` nullo per il mittente di un messaggio di un partecipante verificato. Il nome non si deduce dall'identificativo: le viste dichiarano «(mittente non disponibile)», il verbale conta quei messaggi e l'identificativo dell'autore resta confrontato con la fonte senza eccezioni. La fixture sintetica riproduce il caso.

## ADR-007 - Un documento da firmare per ciascun interessato e periodo

Data: 2026-10-12. Un incarico vale per una coppia di partecipanti e un periodo. Al cambiare dell'uno o dell'altro si produce un documento nuovo, con la propria richiesta e, se serve, la propria istruzione anticipata. Un documento esteso a più perimetri renderebbe indistinguibili sottoscrizioni e revoche.

## ADR-006 - Esecuzione su istruzione esplicita prima della firma

Data: 2026-10-09. Il percorso ordinario attende il documento firmato. Quando il titolare ordina per iscritto di procedere prima, l'ordine si registra in un'istruzione separata con stato `ExecuteBeforeSignature`, vincolata mediante hash al documento e alla richiesta. La richiesta resta `Draft`, il verbale dichiara il documento non firmato e nessuna approvazione o verifica giuridica viene simulata.

## ADR-005 - Un solo fascicolo da compilare

Data: 2026-10-09. Incarico, istruzioni e fonti normative stanno in un solo documento compilabile, con le istruzioni accanto ai campi e le sottoscrizioni in fondo; il modello è `docs/Fascicolo-export.template.txt`. La consegna dell'acquisizione si riduce a cinque file, con archivio tecnico e verbale unico.

## ADR-004 - Prove separate dai dati reali

Data: 2026-10-09. Un solo albero di sorgenti, prove con HTTP simulato e dati sintetici, nessuna conversazione reale usata come fixture. L'uso reale avviene manualmente, con dati e credenziali privati.

## ADR-003 - Periodo esplicito e contesto della conversazione

Data: 2026-10-09. Si conservano entrambi i mittenti della chat diretta, in ordine cronologico, senza filtro su un solo autore. Il periodo è esplicito, con inizio incluso, fine esclusa e fuso obbligatorio; l'intera cronologia non è un'impostazione predefinita.

## ADR-002 - Incarico firmato prima dell'accesso

Data: 2026-10-09. La richiesta nasce `Draft` e il percorso ordinario si ferma prima delle credenziali finché l'incarico non è approvato, completo e firmato, con l'impronta del documento verificata. La firma autorizza un incarico delimitato e non certifica da sola la liceità del trattamento, che resta del titolare.

## ADR-001 - Adozione selettiva del sistema di progetto

Data: 2026-10-09. Si adottano dal template istruzioni condivise, memoria versionata, regole di base, strumenti locali e skill normative. Nessun pacchetto di stack, server MCP aggiuntivo o modifica degli account. La provenienza è in `.claude/template-reference.json`.
