

## Guida Utente
## Introduzione
MapClient è il componente frontend del sistema per la generazione e l'interrogazione di
alberi di regressione. Il client si connette al server remoto attraverso la rete e offre
un'interfaccia a riga di comando per interagire con l'utente: permette di avviare
l'apprendimento di un albero di regressione da una tabella del database, caricare un albero
precedentemente salvato sul server e navigare l'albero per effettuare predizioni su nuovi
dati.
Una volta avviato, il client si connette al server specificato e presenta un menu testuale per
scegliere l'operazione da eseguire. Tutte le elaborazioni avvengono lato server: il client si
occupa di raccogliere i dati dall'utente, inviarli e visualizzare i risultati.
Requisiti di sistema
Per eseguire il client serve Java 21 o superiore. Il jar è già compilato e include tutte le
dipendenze necessarie, quindi non servono librerie esterne da installare.
Il client deve potersi connettere a un'istanza di MapServer già in esecuzione e raggiungibile
sulla rete. Prima di avviare il client, verificare che il server sia attivo e in ascolto sulla porta
corretta.
Installazione e configurazione
Nella cartella di lavoro devono essere presenti due file:
MapClient.jar — l'eseguibile del client
EseguiClient.bat — uno script per l'avvio su Windows
Il client richiede due parametri da riga di comando per stabilire la connessione, i quali sono
già fissati nel comando incluso nel file EseguiClient.bat:
ParametroDescrizioneEsempio
HostIndirizzo IP o hostname del server127.0.0.1 o localhost
PortaPorta su cui il server è in ascolto8080

Questi parametri devono corrispondere alla configurazione del server a cui ci si vuole
connettere.
## Avvio
Per lanciare il client bisogna posizionarsi nella cartella che contiene il jar ed eseguire da
terminale:
java -jar MapClient.jar <host> <porta>
Ad esempio, per connettersi a un server in locale sulla porta predefinita:
java -jar MapClient.jar localhost 8080
Su Windows si può anche fare doppio clic su EseguiClient.bat, che lancia il comando con i
parametri predefiniti localhost 8080.
L'output iniziale mostra le informazioni sulla connessione stabilita:
## Socket[addr=localhost/127.0.0.1,port=8080,localport=xxxxx]
Quando compare questo messaggio la connessione è attiva e il client è pronto per ricevere i
comandi dell'utente.
## Funzionamento
Dopo la connessione, il client presenta un menu per scegliere l'operazione da eseguire:
Learn Regression Tree from data [1]
Load Regression Tree from archive [2]
L'utente deve digitare 1 o 2 e premere Invio. Inserimenti diversi, incluso testo non numerico,
vengono scartati senza interrompere l'esecuzione: il menu viene semplicemente riproposto
finché non si inserisce un valore valido.
Apprendimento da database (opzione 1)
Selezionando l'opzione 1, il client chiede il nome della tabella su cui addestrare l'albero:

File name:
Digitare il nome della tabella (ad esempio tabellaClienti) e premere Invio. Il client invia la
richiesta al server e resta in attesa mentre l'elaborazione avviene da remoto. Se l'operazione
ha successo, compare:
Starting data acquisition phase!
Starting learning phase!
In caso di errore (tabella inesistente, problemi di connessione al database, ecc.), il server
restituisce un messaggio di errore che viene visualizzato direttamente sul client.
Caricamento da archivio (opzione 2)
Selezionando l'opzione 2, il client chiede il nome del file archivio salvato sul server:
File name:
Digitare il nome del file e premere Invio. Il client resta in attesa della risposta e, se
l'operazione va a buon fine, passa automaticamente alla fase di predizione.
Se il file non esiste o c'è un errore di lettura, viene visualizzato il messaggio di errore ricevuto
dal server.
Fase di predizione
Dopo aver caricato o addestrato un albero, il client avvia automaticamente la fase di
predizione:
Starting prediction phase!
Il server invia una serie di domande relative agli attributi dell'albero. Per ogni domanda, il
client mostra il testo ricevuto e attende l'inserimento di un valore numerico intero che
rappresenta il percorso da seguire nell'albero:
<domanda ricevuta dal server>

L'utente digita il valore richiesto e preme Invio; se il valore inserito non è un numero intero,
la stessa domanda viene riproposta senza che il programma si interrompa. Il processo
continua finché il server non ha raccolto tutte le informazioni necessarie.
Quando la predizione è completa, il client mostra il risultato:
Predicted class: <valore_predetto>
Dopo ogni predizione, il client chiede se si vuole effettuarne un'altra:
Would you repeat ? (y/n)
Premere y e Invio per effettuare una nuova predizione sullo stesso albero
Premere n e Invio per terminare la sessione e chiudere il client
Risoluzione dei problemi più comuni
Il client non si avvia e mostra un errore di connessione
Il server non è raggiungibile all'indirizzo o sulla porta specificati. Verificare che:
MapServer sia effettivamente in esecuzione
L'indirizzo IP sia corretto (se si usa localhost, il server deve girare sulla stessa macchina)
La porta sia quella su cui il server è in ascolto (predefinita: 8080)
Non ci sia un firewall che blocca la connessione
La connessione cade durante una predizione
Se la connessione con il server si interrompe improvvisamente, il client termina con un
messaggio di errore. Questo può capitare se:
Il server viene arrestato mentre il client è connesso
Si verifica un problema di rete
In questi casi basta riavviare il client e ricollegarsi.
Il client mostra un errore dopo l'inserimento del nome tabella
L'errore proviene dal server. Le cause più comuni sono:
La tabella specificata non esiste nel database

Il database non è raggiungibile dal server
La tabella ha una struttura incompatibile (manca l'attributo target numerico come ultima
colonna)
Se si digita qualcosa che non corrisponde al formato atteso, ad esempio del testo quando è
richiesto un numero, il client non si blocca e non si chiude: scarta semplicemente l'input non
valido e ripropone la stessa richiesta, finché non viene inserito un valore accettabile. Non
serve quindi preoccuparsi di eventuali errori di battitura: basta correggere l'input alla
richiesta successiva.
## Arresto
Per chiudere il client prima che la sessione termini naturalmente, premere Ctrl+C nella
finestra della console. Questo comando interrompe immediatamente il processo e chiude la
connessione con il server.
Se si sta effettuando una predizione, è preferibile rispondere n alla domanda "Would you
repeat?" per chiudere la connessione in modo pulito.