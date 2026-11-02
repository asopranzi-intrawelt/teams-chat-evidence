# Pendenze

Registro riaperto il 2026-11-02 con la pubblicazione dello strumento. Le pendenze dei singoli incarichi, compresi presupposti e firme di competenza aziendale, stanno nel livello privato del progetto.

## P-001 - Strumenti di preparazione e di esecuzione di un incarico

Stato: aperta dal 2026-11-02. La preparazione del documento da firmare, della richiesta e dell'istruzione, e il lancio dell'acquisizione con l'inventario degli allegati, passano ancora da due script privati che portano nel codice valori del caso. Vanno portati nel repository come strumenti parametrici: firmatario, destinazione e testi si leggono da un file di caso nel livello privato, e servono prove sintetiche. Finché non lo sono, un incarico si esegue dalla copia privata.

## P-002 - Recupero degli allegati

Stato: aperta dal 2026-11-02, decisione per incarico. Un file allegato a una chat vive nello spazio OneDrive del mittente, e il messaggio ne conserva solo nome e collegamento. Con i permessi del percorso di acquisizione l'inventario funziona e il download no: `tools/teams_attachments.py --scarica` richiede `Files.Read.All` o `Sites.Read.All`. Ampliare i permessi allarga il perimetro dai messaggi ai file personali dei partecipanti, e va deciso caso per caso, insieme al perimetro scritto nel documento da firmare.

## P-003 - Permessi dell'applicazione Graph ridotti al necessario

Stato: aperta dal 2026-11-02, azione dell'utente in Entra ID. Con la rimozione degli script precedenti, il percorso rimasto usa soltanto `User.Read.All` e `Chat.Read.All`. I permessi sui team e sui canali, se l'applicazione li ha ancora, non servono più e si possono revocare.
