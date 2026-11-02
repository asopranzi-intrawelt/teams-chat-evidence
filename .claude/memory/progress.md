# Registro di avanzamento

Registro riaperto il 2026-11-02 con la pubblicazione dello strumento. Le tappe precedenti, che descrivevano i singoli incarichi, stanno nel livello privato del progetto.

## 2026-11-02 - Pubblicazione dello strumento

Stato del codice alla pubblicazione. `Export-TeamsEvidence.ps1` orchestra acquisizione, verifiche, verbale, archivio, manifesto, consegna verificata e rimozione della copia locale; `Export-TeamsConversation.ps1` esegue il recupero Graph della sola chat diretta fra due partecipanti verificati; `tools/teams_evidence.py` verifica ogni messaggio fra risposta, copia tecnica, CSV e HTML e produce PDF e manifesti; `tools/teams_attachments.py` inventaria gli allegati dal pacchetto consegnato e conta a parte le citazioni di messaggi, che Graph presenta come allegati di tipo `messageReference`; `tools/Remove-LocalEvidence.ps1` elenca, preserva e distrugge il materiale locale, scrivendo il registro sulla destinazione prima di cancellare.

Le due prove sintetiche sono verdi. La prova del tool completo verifica anche la rimozione della copia locale dopo la consegna, con una corsa dedicata che cade su un tool mutato che non rimuove.

Rimossi gli script di esportazione generica precedenti, che non applicavano i controlli su incarico e firma (ADR-010). Scritti il README per un lettore esterno e la licenza MIT. Memoria e schede riscritte in forma astratta (ADR-009); controllo di anonimizzazione pulito sulle categorie bloccanti.

**Template:** L-001, L-002
