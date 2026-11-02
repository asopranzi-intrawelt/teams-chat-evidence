# Lezioni per il template

> Le lezioni nate in questo progetto che valgono oltre il progetto, cioè che passano le due domande della sezione 16 di `.claude/PROJECT-SYSTEM.md`: restano vere tolti i nomi propri, e non sono già nella testa di chi lavora bene. Una voce per lezione, creata con `python .claude/templates/tools/lezioni-template.py nuova` nel giro di lavoro in cui la lezione nasce. Lo stato non si scrive qui: una lezione è recepita quando il template la elenca in `.claude/templates/lezioni-recepite.json`, che arriva con l'allineamento.

Adottato il: 2026-11-02

## L-001 - 2026-11-02 - Bonifica: il biglietto all'assistenza su un piano senza assistenza va previsto, con la via self-service

In un progetto istanziato il biglietto della procedura BONIFICA-STORIA è stato chiuso dall'assistente automatico un minuto dopo l'invio: per il piano dell'account le sole risorse disponibili sono quelle self-service. La procedura descrive il rifiuto ma l'unica contromossa che offre è rispondere nel biglietto. Per un repository senza fork, pull request e issue esiste una via self-service che rimuove davvero i vecchi oggetti: cancellare il repository sulla piattaforma, ricrearlo vuoto con lo stesso nome e la stessa visibilità, e spingere la storia riscritta. Si perdono solo le impostazioni. Proposta: aggiungere questa via al paragrafo 5, con le condizioni in cui vale, e indicare di distruggere il bundle con la storia originale solo dopo.

## L-002 - 2026-11-02 - Un cambio di comportamento di uno strumento aggiorna nello stesso commit la prova che lo esercita

In un progetto istanziato uno strumento ha cominciato a rimuovere da solo la copia locale dopo la consegna, e la sua prova sintetica, che leggeva quella copia, è rimasta rossa per settimane senza che nessuno la rilanciasse, perché i controlli di chiusura non eseguono le prove del progetto. È emerso solo rilanciandole a mano dopo un incidente. Proposta: i controlli di chiusura eseguono anche le prove dichiarate nella scheda dev-testing, oppure segnalano quando un commit tocca uno strumento coperto da una prova che non è stata rilanciata.
