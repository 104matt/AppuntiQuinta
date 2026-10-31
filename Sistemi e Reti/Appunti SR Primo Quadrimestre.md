# Appunti 12/10/26

**Domande a inizio lezione di ripasso:**

* Descrivi il paradigma P2P e dimmi 2 protocolli P2P: Un client funge anche da server, esempi di Protocolli sono Torrent e Gnutella (Pure VoiP)
* Qual'è lo scopo originario del protocollo telnet: Accedere da remoto a un computer attraverso una console. Fa una attività di emulazione di terminale che può essere o un computer o un apparato di rete.
* Qual'è la versione sicura di Telnet: SSH
* Su quale porta girano: Telnet 23 e SSH 22
* A cosa serve FTP e che porte usa: Protocollo di trasferimento file da un client a un server e gira sulla porta 20 (Porta dati) e 21 (Porta comandi)
* Quai sono le modalità di crezione di un collegamento FTP: Attiva e Passiva, oggi usiamo la modalità passiva.
* Quante sono le modalità d'accesso a un server FTP: Quella anonima che non prevede l'uso di password e quella utente che richiede l'uso di password
* A cosa serve SFTP e FTPS: Sono FTP sicure e SFTP utilizza la porta 22 e non utilizza ne la modalità passiva ne quella attiva ma trasferisce i file che vengono cifrati da SSH


## CLIENT/SERVER VS PEER TO PEER

| Client/Server | Peer to Peer (P2P) |
| ------------- | ------------------ |
| Ruoli **fissi**: il **server** offre il servizio ed è sempre acceso, il **client** fa le richieste | Ogni nodo (**peer**) è **sia client che server**: chiede e offre risorse |
| Il server ha un indirizzo **noto e fisso** | I peer si collegano e scollegano in continuazione, indirizzi variabili |
| Se il server cade il servizio non è più disponibile (**single point of failure**) | Non c'è un punto centrale: se cade un peer il servizio continua |
| Il server può diventare un **collo di bottiglia** con tanti client | Più peer ci sono, più risorse ci sono (**scalabile**) |
| Esempi: HTTP, FTP, SMTP, Telnet, SSH | Esempi: **BitTorrent**, **Gnutella**, eMule |

{% hint style="info" %}
Nel P2P **puro** (es. Gnutella) non esiste nessun server centrale. Nel P2P **ibrido** (es. BitTorrent con i **tracker**) c'è un server che tiene solo l'elenco di chi possiede cosa, ma i file vengono scambiati direttamente tra i peer.
{% endhint %}

## TELNET E SSH

| Acronimo | Utilità | Porta | Paradigma | Protocollo Transport | Sicurezza |
| -------- | ------- | ----- | --------- | -------------------- | --------- |
| TELNET (TELetype NETwork) | Accesso remoto a un terminale (emulazione di terminale) | 23 | Client/Server | TCP | SSH (22) |

* **Telnet**: permette di aprire una **console remota** su un computer o un apparato di rete (router, switch) e dare comandi come se fossimo davanti alla macchina. Il problema è che **tutto viaggia in chiaro**, comprese username e password: chiunque intercetti il traffico (sniffing) le legge.
* **SSH (Secure SHell)**: fa la stessa cosa di Telnet ma **cifra** tutta la comunicazione. Prima di tutto client e server si scambiano le chiavi e il server si **autentica** al client (con la sua chiave pubblica), poi l'utente si autentica con **password** oppure con una **coppia di chiavi pubblica/privata**. Oltre al terminale remoto SSH permette anche il trasferimento sicuro di file (**SFTP**, **SCP**) e il **tunneling** di altri protocolli.

**PuTTY** (pag. 366): è il **client** più usato su Windows per collegarsi in **SSH** (e anche Telnet) a una macchina remota. Si inserisce l'**indirizzo IP/hostname** del server, la **porta** (22 per SSH) e il tipo di connessione; al primo collegamento PuTTY mostra l'**impronta (fingerprint)** della chiave del server da accettare, poi chiede username e password.

{% hint style="warning" %}
I comandi di Telnet/SSH **non vengono chiesti**.
{% endhint %}

## FTP

| Acronimo | Utilità | Porta | Paradigma | Protocollo Transport | Sicurezza |
| -------- | ------- | ----- | --------- | -------------------- | --------- |
| File Transfer Protocol | Trasferimento di file tra client e server | 21 (comandi) / 20 (dati) | Client/Server | TCP | SFTP (22) / FTPS (990) |

FTP usa **due connessioni TCP separate**:
* **Connessione di controllo** (porta **21**): resta aperta per tutta la sessione e ci passano i **comandi** (USER, PASS, LIST, RETR, STOR...) e le risposte del server.
* **Connessione dati** (porta **20** in attiva): viene aperta **ogni volta** che bisogna trasferire un file o un elenco di file e poi chiusa.

{% hint style="info" %}
**TFTP** (Trivial FTP): versione semplificata di FTP che usa **UDP** (porta 69), senza autenticazione. Solo accennato, **non viene chiesto**.
{% endhint %}

### MODALITÀ ATTIVA E PASSIVA (pag. 368)

La differenza sta in **chi apre la connessione dati**.

* **Modalità attiva**
  1. Il client si collega dalla sua porta casuale N alla porta **21** del server (controllo).
  2. Il client comunica al server, con il comando **PORT**, il suo IP e una porta N+1 su cui si mette in ascolto.
  3. È il **server** che apre la connessione dati, dalla sua porta **20** verso la porta N+1 del **client**.
  * **Problema**: la connessione dati arriva **dall'esterno verso il client**, quindi viene quasi sempre **bloccata dal firewall o dal NAT** del client.
* **Modalità passiva** *(quella usata oggi)*
  1. Il client si collega alla porta **21** del server (controllo).
  2. Il client manda il comando **PASV**; il server risponde con il suo IP e una **porta casuale** (> 1023) su cui si mette in ascolto.
  3. È il **client** che apre anche la connessione dati verso quella porta del server.
  * **Vantaggio**: entrambe le connessioni partono **dal client**, quindi passano senza problemi dal firewall/NAT del client. Il server deve però tenere aperto un range di porte.

| | Chi apre la connessione dati | Porta dati lato server | Problemi con firewall |
| - | - | - | - |
| **Attiva** | Server → Client | 20 | Sì, lato client |
| **Passiva** | Client → Server | Casuale (> 1023) | No (lato client) |

### MODALITÀ DI ACCESSO A UN SERVER FTP (pag. 369)

* **Accesso anonimo**: si entra con username **anonymous** (o ftp) e come password, per convenzione, il proprio indirizzo email (o niente). Di solito si può solo **scaricare** da una cartella pubblica. Usato per distribuire file pubblici.
* **Accesso con utente (autenticato)**: serve un **account** con username e password definito sul server. L'utente ha permessi di lettura/scrittura sulle proprie cartelle.

### CLIENT FTP: FILEZILLA E BROWSER

* **FileZilla**: client FTP grafico (esiste anche FileZilla **Server**). Si inseriscono **Host**, **Nome utente**, **Password** e **Porta** e si usa la "**Connessione rapida**". La finestra è divisa in due: a sinistra il **file system locale**, a destra quello **remoto**, e si trascinano i file da una parte all'altra. Supporta FTP, FTPS e SFTP.
* **Browser**: si può accedere a un server FTP scrivendo l'URL nella barra degli indirizzi:
  * accesso anonimo: `ftp://ftp.server.it`
  * accesso con utente: `ftp://utente:password@ftp.server.it` (la password è visibile in chiaro nell'URL!)
  * Con il browser di solito si può solo **navigare e scaricare**, non caricare. I browser moderni (Chrome, Firefox) hanno ormai **rimosso** il supporto a FTP.
* **Riga di comando**: comando `ftp` da terminale.

## PROTOCOLLI SFTP E FTPS

Il modo migliore per implementare un modo sicuro di trasferimento dati a FTP è appoggiarsi su SSH in modo che ci sia una cifratura a livello di dati. Per SFTP (Secure FTP) si usa la porta di SSH che è la 22 e non utilizza ne la modalità passiva ne quella attiva ma trasferisce i file che vengono cifrati da SSH.

**SFTP** è più utilizzato di **FTPS**. Inoltre **FTPS** prevede 2 versioni: quella implicità (che utilizza la porta 990) richiede l'utilizzo di SSL/TLS dal primo contatto con il server; quella esplicita prevede l'instaurazione della connessione come FTP non sicuro (porta 21) ed è il **server** che crea la **connessione sicura** sul canale dati (porta casuale). La **modalità esplicità** è piu utilizzata.

Nelle versioni sicure di un protocollo è stato introdotto tra livello **Application** e il livello **Transport** un livello chiamato **SSL/TLS**.

Nel livello ISO/OSI i livelli **Presentation e Session** gestiscono e coprono la parte di sicurezza.

## PROTOCOLLO HTTP

Per ogni protocollo abbiamo una tabella di informazioni generali che vanno sapute sempre.

| Acronimo                     | Utilità                                 | Porta | Paradigma     | Protocollo Transport | Sicurezza   |
| ---------------------------- | --------------------------------------- | ----- | ------------- | -------------------- | ----------- |
| Hyper Text Transfer Protocol | Trasferimento di contenuti multimediali | 80    | Client/Server | TCP                  | HTTPS (443) |

Lato **client** abbiamo il browser che può essere **Chrome** (e **non "Google"**, che è il motore di ricerca) oppure **Firefox**, mentre a livello **server** come applicativo usiamo **Apache**, **IIS** (o **Nginx**).

{% hint style="warning" %}
**N.B.**

_Non bisogna confondere il **www** (Organizzazione) e **http** (Protocollo) oppure **Internet** (Insieme di router) e il **www**._
{% endhint %}

### HTTP, HTML E BROWSER (pag. 371)

* **HTTP**: è il **protocollo**, cioè le regole con cui client e server si scambiano richieste e risposte.
* **HTML** (HyperText Markup Language): è il **linguaggio** con cui sono scritte le pagine web, cioè il **contenuto** che HTTP trasporta. Non è un protocollo e non è un linguaggio di programmazione.
* **Browser**: è il **programma client** (user agent) che fa le richieste HTTP, riceve la pagina HTML e la **interpreta e visualizza**.
* **Web server**: il programma lato server (Apache, IIS, Nginx) che riceve le richieste HTTP e restituisce le risorse.
* **WWW**: l'insieme delle pagine/risorse collegate tra loro tramite **ipertesto** (link), che girano sopra Internet usando HTTP.

{% hint style="info" %}
**URL vs URI**

**URI** (Uniform Resource Identifier): stringa che **identifica** una risorsa, per nome o per posizione (es. `https://ispascalcomandini.com`).
**URL** (Uniform Resource Locator): tipo di URI che dice anche **dove** si trova la risorsa e **come** raggiungerla (es. `https://ispascalcomandini.com/pagine/presentazione`).
Ogni URL è un URI, ma non ogni URI è un URL.

Struttura di un URL: `protocollo://host:porta/percorso/risorsa?query#frammento`
es. `http://www.sito.it:80/cartella/pagina.php?nome=mario&eta=18`
{% endhint %}

{% hint style="info" %}
**Protocollo stateless**: un protocollo è **stateless** quando il server **non conserva nessuna informazione** sulle richieste precedenti dello stesso client: ogni richiesta è indipendente dalle altre. HTTP è stateless (vedi Cookie più sotto).
{% endhint %}

### TIPI DI RICHIESTE

#### **HTTP Request C --> S**

All'interno contiene un:

* **Request Line**: GET (1)  /info.html (2) HTTP 1.1 (3)
  * **GET** può essere anche **POST**, **PUT**, **DELETE**, **OPTIONS**, **HEAD**
  * **Risorsa/URL** (Uniform Resource Locator)
    * **File** che se non viene specificato va a cercare **index.php o index.html** ma se contiene invece una risorsa diventa un **URI** e ci porta ad essa. Nel caso in cui andiamo a cercare **index.php o index.html** l'URL coinciderà con l'URI e non ci sarà distinzione.
  * **Versione**:
    * Dalla 1.1 abbiamo una connessione persistente, ovvero non viene riavviata una connessione TCP ogni volta che apro una risorsa nel sito come nella 1.0
    * Nella versione 1.0 mi collegavo a un server web e mi scaricavo la prima pagina. Tutte le volte che su quel sito web aprivo risorse nuove veniva tirata giu una connessione TCP per esser tirata su nuovamente. **VEDI TABELLA HTTP**
* **Header**
	* Contiene informazioni aggiuntive riguardo al client e alla richiesta, nella forma `Nome: valore`. Esempi:
		* **Host**: `www.sito.it` (il nome del server, obbligatorio dalla 1.1)
		* **User-Agent**: il browser e il sistema operativo del client (es. `Mozilla/5.0 (Windows NT 10.0) Firefox/120.0`)
		* **Accept**: i tipi di contenuto che il client accetta (es. `text/html`)
		* **Accept-Language**: la lingua preferita (es. `it-IT`)
		* **Connection**: `keep-alive` (connessione persistente) oppure `close`
		* **Cookie**: i cookie che il client rimanda al server
* **Body**
	* Se faccio una richiesta **GET** sarà vuoto, ma se faccio una **POST** conterrà i dati che vogliamo inviare.

Esempio di request:
```
GET /info.html HTTP/1.1
Host: www.sito.it
User-Agent: Mozilla/5.0
Accept: text/html
Connection: keep-alive

```

#### **GET, POST E QUERY STRING**

* **Query string**: è la parte dell'URL dopo il **?**, formata da coppie `nome=valore` separate da **&**. Serve a passare dati al server. es. `cerca.php?parola=rete&pagina=2`
* **GET**: serve a **richiedere** una risorsa. Gli eventuali dati vengono passati nella **query string**, quindi sono **visibili nell'URL**, finiscono nella cronologia e nei preferiti e hanno un **limite di lunghezza**. Body vuoto. Si usa per ricerche, link, ecc.
* **POST**: serve a **inviare** dati al server (es. form di login, upload). I dati viaggiano nel **body** della richiesta, quindi **non sono visibili nell'URL** e non hanno limiti pratici di dimensione.

{% hint style="warning" %}
**N.B.** POST **non è sicuro** di per sé: i dati non si vedono nell'URL ma viaggiano **in chiaro** nel body. Per cifrarli serve **HTTPS**.
{% endhint %}

Altri metodi: **PUT** (carica/sostituisce una risorsa), **DELETE** (cancella una risorsa), **HEAD** (come GET ma il server risponde solo con l'header, senza body), **OPTIONS** (chiede quali metodi supporta il server).

#### **HTTP Response S --> C**
La HTTP Response è composta da 3 parti:
- **Status Line**: versione + codice + messaggio, es. `HTTP/1.1 200 OK`
	- Troviamo un codice a 3 cifre che ci dice com'è andata a finire la nostra request.
		- **1xx (Informativo)** la richiesta è stata ricevuta e l'elaborazione continua
			- 100 Continue
		- **2xx (Success)** la richiesta è stata soddisfatta/la risorsa è stata trovata.
			- 200 OK
			- 201 Created, la risorsa è stata creata (es. dopo una POST/PUT)
		- **3xx (Redirection)** (Non c'è una risposta diretta ma la richiesta è ritenuta corretta e viene detto come ottenere la risposta)
			- 301 Moved Permanently, la risorsa è stata spostata definitivamente su un altro URL
			- 302 Found, risorsa disponibile (temporaneamente) su un URL diverso
			- 304 Not Modified, la risorsa non è cambiata: il client può usare la copia in cache
		- **4xx Errore lato Client**  (La richiesta non può essere soddisfatta perchè sbagliata)
			- 400 Bad request, la richiesta è scritta male (sintassi errata)
			- 401 Unauthorized, serve autenticarsi
			- 403 Forbidden, la risorsa esiste ma non hai i permessi per accedervi
			- 404 Not Found, la risorsa richiesta non esiste sul server
		- **5xx Errore lato Server** (La mia richiesta non può essere soddisfatta per un problema interno del server)
			- 500 Internal Server Error, il server ha avuto un errore generico mentre elaborava la richiesta
			- 501 Not Implemented, il server non supporta il metodo richiesto (es. un metodo HTTP che non conosce)
			- 503 Service Unavailable, il server è sovraccarico o in manutenzione
- **Header**: informazioni sulla risposta e sul server. Esempi:
	- **Server**: il software del server (es. `Apache/2.4`)
	- **Date**: data e ora della risposta
	- **Content-Type**: tipo del contenuto nel body (es. `text/html; charset=UTF-8`)
	- **Content-Length**: dimensione del body in byte
	- **Last-Modified**: data dell'ultima modifica della risorsa
	- **Set-Cookie**: il server chiede al client di salvare un cookie
	- **Location**: il nuovo URL (usato con i codici 3xx)
- **Body**: il contenuto vero e proprio (la pagina HTML, un'immagine...).

### VERSIONI DI HTTP


| Versione                              | Caratteristiche                                                                                              |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **HTTP 1.0**                          | Semplice ma connessioni non persistenti: una connessione TCP **per ogni** risorsa **(1996)**                  |
| **HTTP 1.1**                          | Connessioni persistenti (keep-alive), header **Host** obbligatorio (più siti sullo stesso server), pipelining, compatibilità con API (1997/1999) |
| **HTTP/2** *(Piu utilizzato ad oggi)* | Più utilizzato -> Multiplexing (Più request e response sulla stessa connessione), protocollo **binario**, compressione degli header, Overhead ridotto **(2015)** |
| **HTTP/3**                            | Utilizza QUIC che si appoggia su UDP quindi **piu veloce** e **piu sicuro** perchè integra TLS (2022)        |

#### ESEMPIO HTTP/3


| HTTP(3)       |                                                                                                                                         |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **QUIC**      | Nel mezzo viene messo QUIC (Protocollo ideato da Google) che nel modello ISO/OSI è gestito da application e presentation. Utilizza UDP. |
| **TRANSPORT** | UDP                                                                                                                                     |


{% hint style="info" %}
L'**HTTP** è un protocollo **stateless** ovvero perde la memoria dei contenuti scambiati ad ogni richiesta, oggi riusciamo a mantenere lo stato grazie all'utilizzo dei **Cookie** rendendo il protocollo **stateful**.
{% endhint %}

#### I COOKIE

File di testo memorizzato lato client e tutte le volte che ti colleghi al server, questo viene inviato e interpretato per personalizzare la sessione/analizzare dati/mantenere dati di login etc...

Funzionamento: alla prima risposta il server manda l'header **Set-Cookie**, il browser salva il cookie e da quel momento lo rimanda in ogni richiesta allo stesso sito con l'header **Cookie**.



# Proxy HTTP
Un qualcosa che si interpone tra l'applicativo client e l'applicativo server: il client manda la richiesta al proxy, il proxy la inoltra al server **a nome suo**, riceve la risposta e la restituisce al client. Per il server il client è il proxy.

{% hint style="warning" %}
**N.B.** Non va confuso con il **default gateway**: il router si limita a **inoltrare pacchetti** (livello 3), il proxy invece lavora a livello **applicazione**, legge e può modificare le richieste HTTP.
{% endhint %}

#### **Utilizzo / Vantaggi**
- *connettività*: Un proxy puo esser usato per permettere a una rete privata di accedere a una **pubblica**
- *privacy*: Un proxy puo **mascherare** il vero indirizzo IP del client, perchè il server vede solo l'IP del proxy.
- *caching*: Un proxy puo immagazzinare delle risposte HTTP per un certo tempo in modo che se un altro client richieda la stessa risorsa puo essere servito direttamente il contenuto della cache **senza** dover fare un altra richiesta al server (risparmio di banda e risposte più veloci).
- *monitoraggio*: Un proxy puo permettere di tenere traccia di tutto il traffico HTTP fatto (pagine visitate...)
- *amministrazione*: Un proxy puo impostare **regole** per il traffico in entrata e puo applicare anche filtri sulle pagine web visitate (es. bloccare siti nella rete scolastica/aziendale).

{% hint style="info" %}
Tipi di proxy anonimi (**non va saputo** per la verifica): **NOA** (Non Anonymous), **ANM** (Anonymous), **HIA** (High Anonymous), proxy **distorcenti** (trasmettono un IP casuale).
{% endhint %}

#### **PROXY E HTTPS**

Con **HTTPS** la comunicazione tra browser e server è **cifrata end-to-end** con TLS, quindi un proxy normale **non può leggere né mettere in cache** il contenuto. Il browser manda al proxy il metodo **CONNECT** (`CONNECT www.sito.it:443`) e il proxy apre un **tunnel**: si limita a passare i byte cifrati senza vederli (vede solo a quale host ci si collega).

Se un'azienda vuole comunque ispezionare il traffico HTTPS, il proxy deve fare da "**man in the middle**": decifra, controlla e ricifra, ma per farlo i client devono **fidarsi del certificato del proxy** (installato apposta sui PC aziendali).

# LA POSTA ELETTRONICA

I **protocolli** che vediamo **sono**:


| Acronimo   | Utilità         | Porta   | Paradigma     | Protocollo Transport | Sicurezza                 |
| ---------- | --------------- | ------- | ------------- | -------------------- | ------------------------- |
| SMTP (Simple Mail Transfer Protocol) | Invio Email     | 25      | Client/Server | TCP                  | SMTPS (465) / STARTTLS (587) |
| POP3/IMAP4 | Ricezione email | 110/143 | Client/Server | TCP                  | POP3S (995) e IMAPS (993) |

**SMTP**:
- Il client mittente si connette a SMTP porta 25 e la mail viene messa in una Mailbox Serverside che si collegherà via SMTP a un altra Mailbox Server alla quale si collegherà il client destinatario con IMAP porta 143 e riceverà la mail.

Il protocollo SMTP usa i seguenti termini per riferirsi al software utilizzato su client e server:
- **MUA** (Mail User Agent): il **client di posta** usato dall'utente per scrivere e leggere le mail (Outlook, Thunderbird, Gmail via web).
- **MSA** (Mail Submission Agent): Risiede sul mail server tramite il quale invia l'email il **MUA**; riceve la mail dal MUA e la passa all'MTA.
- **MTA** (Mail Transfer Agent): il software che **trasferisce** la mail da un mail server all'altro tramite SMTP (es. Postfix, Sendmail). Per sapere a quale server mandare la mail fa una query **DNS** di tipo **MX** sul dominio del destinatario.
- **MDA** (Mail Delivery Agent): sul server del destinatario, **deposita** la mail nella **casella (mailbox)** dell'utente.

Percorso completo:
```
MUA mittente --SMTP--> MSA/MTA (server mittente) --SMTP--> MTA (server destinatario) --> MDA --> Mailbox --POP3/IMAP--> MUA destinatario
```

### COMANDI SMTP

Il dialogo SMTP è **testuale**: il client manda comandi e il server risponde con un **codice a 3 cifre** (simile a HTTP).

| Comando | Significato |
| ------- | ----------- |
| **HELO / EHLO** | Il client si presenta al server (EHLO = Extended SMTP) |
| **MAIL FROM:** | Indirizzo del mittente |
| **RCPT TO:** | Indirizzo del destinatario (uno per ogni destinatario) |
| **DATA** | Inizio del messaggio (header + corpo), che termina con una riga contenente solo un **punto** `.` |
| **QUIT** | Chiude la connessione |

Codici di risposta: **2xx** ok (es. **220** servizio pronto, **250** OK), **3xx** in attesa di altri dati (**354** inizia a scrivere il messaggio), **4xx** errore temporaneo, **5xx** errore permanente (**550** casella inesistente).

Il messaggio è diviso in **header** (From, To, Cc, Subject, Date) e **body**, separati da una riga vuota.

### LA SICUREZZA DI SMTP

In SMTP chiunque può spedire una mail a chiunque e il mittente **non viene verificato** (si può scrivere qualunque indirizzo in MAIL FROM, **spoofing**): questa è una debolezza in quanto si può tranquillamente **spammare** la mail di qualcuno.

Difatti in **SMTP** non esisteva la crittografia e difatti viene introdotta la **SMTPS** (SMTP su SSL/TLS) e l'autenticazione del client (**SMTP AUTH**).

Nelle comunicazioni tra mail server viene usata la porta 25 mentre per l'invio dal client al proprio server la 465 (SMTPS) o la 587 (submission con STARTTLS).

Nonostante tutto l'SMTP è un protocollo datato e vulnerabile.

### MIME (parte iniziale pag. 382)

Inizialmente SMTP poteva trasportare solo testo **ASCII a 7 bit**: niente lettere accentate, niente immagini, niente allegati. Col tempo si è passati alla codifica **MIME (Multipurpose Internet Mail Extensions)**, che aggiunge degli header al messaggio per descriverne il contenuto:
- **MIME-Version**: 1.0
- **Content-Type**: il tipo di contenuto nella forma `tipo/sottotipo` (es. `text/plain`, `text/html`, `image/jpeg`, `application/pdf`, `multipart/mixed` per messaggi con allegati)
- **Content-Transfer-Encoding**: come è stato codificato il contenuto per farlo diventare ASCII (es. **base64** per i file binari)

I tipi MIME sono usati anche da **HTTP** nell'header Content-Type.

# LA RICEZIONE (PROTOCOLLO POP E IMAP)

### POP3
Ci ricordiamo che siamo nella fase finale, la mail è nella casella di posta e il client la deve scaricare. Mentre l'**SMTP** è un **protocollo push**, **POP e IMAP** sono un protocollo **pull**.

Nel momento in cui configuro **POP** e scarico qualcosa dalla casella di posta **quest'ultima viene svuotata** (comportamento predefinito: download e cancellazione dal server). Il pro è che posso consultare la mail offline e non occupo spazio sul server, il contro è che se voglio consultare la casella di posta da un altro computer non posso in quanto se scarico la mail si svuota la casella.

**POP3** (Post Office Protocol versione **3**, porta **110**) è un **protocollo autenticato** (USER e PASS) ma **non cifrato**: la versione cifrata è **POP3S** (porta **995**).

Fasi di una sessione POP3:
1. **Autorizzazione**: `USER` e `PASS`
2. **Transazione**: `STAT` (numero messaggi), `LIST` (elenco), `RETR n` (scarica il messaggio n), `DELE n` (segna da cancellare)
3. **Aggiornamento**: con `QUIT` il server cancella davvero i messaggi segnati

### IMAP4

**IMAP4** (Internet Message Access Protocol, porta **143**, cifrato **IMAPS** porta **993**) lascia le mail **sul server**: con IMAP4 viene rimosso lo svuotamento della casella di posta. Caratteristiche (pag. 385):
- Le mail restano sul server, quindi posso leggerle da **più dispositivi** (PC, telefono, webmail) e vedo sempre la stessa situazione (**sincronizzazione**).
- Organizzazione in **cartelle** sul server (creare, rinominare, spostare).
- Stato dei messaggi (**flag**): letto/non letto, risposto, importante, eliminato.
- Posso scaricare **solo l'intestazione** o solo una parte del messaggio (es. senza allegati).
- **Ricerca** dei messaggi direttamente sul server.
- Più client possono essere collegati contemporaneamente alla stessa casella.
- Contro: serve essere **online** e occupa **spazio sul server**.

| | POP3 | IMAP4 |
| - | - | - |
| Porta / sicura | 110 / 995 | 143 / 993 |
| Dove restano le mail | Scaricate sul client e cancellate dal server | Restano sul server |
| Più dispositivi | No | Sì, sincronizzati |
| Cartelle sul server | No | Sì |
| Uso offline | Sì | Solo i messaggi già scaricati |
| Spazio sul server | Poco | Tanto |



# DOMANDE VERIFICA

- **DOMANDE A CROCETTE SU TCP/IP**
- Domande a **RISPOSTA APERTA** sul protocollo **application** e **DOMANDE A CROCETTE**.

## COSE DA SAPERE

- Differenza tra client server e paradigma peer to peer (torrent e ignutella)
- Telnet e SSH
	- Comandi non vengono chiesti
	- Putty ed SSH si (Pag 366)
- FTP
	- TFTP solo accennato, non lo chiede
- Tutta pag 368 (Modalità attiva e modalità passiva)
- Pag 369 con le modalità di accesso in fondo
- Filezilla e mod di acesso del browser
- Pag 371 differenze tra http, html, browser versioni etc...
- Tutta la pag 372
	- L'RFC non va saputo
	- Va saputo cosa aggiunge ogni versione
	- Cos'è l'URL cos'è l'URI cos'è un protocollo stateless
- HTTP request e HTTP response
	- Query String, POST, GET
	- Ricordarsi gli esempi di campi dell'header
	- Codici della response (Vanno saputi)
		- E alcuni esempi
- Proxy HTTP pag 376 (Vantaggi di proxy HTTP e sicurezza con HTTPS)
	- Senza la parte finale dei proxy anonimi
- Tutta la pag 378, tutta la pag 379 e tutta la pagina 380, 381. Saltare la 382 tranne la parte iniziale di mime e saltare pag 383 **studiare pagina 384 e 385**.

## DA REPERIRE (pagine del libro da passare)

Le sezioni sopra sono state completate con informazioni generali, ma **non sono verificate sul libro**. Servono queste pagine (foto o testo) per allinearle a quello che c'è scritto:

- [ ] **Pag. 366**: parte su **PuTTY** e SSH (il prof la chiede: controllare cosa dice il libro sulla configurazione e sull'autenticazione con chiavi).
- [ ] **Pag. 368**: **modalità attiva e passiva** (controllare se il libro usa lo schema con le porte N / N+1 e i comandi PORT/PASV).
- [ ] **Pag. 369**: **modalità di accesso** a un server FTP (parte in fondo alla pagina) + parte su **FileZilla** e **accesso da browser**.
- [ ] **Pag. 371-372**: differenze HTTP/HTML/browser e **cosa aggiunge ogni versione** di HTTP secondo il libro (date e caratteristiche).
- [ ] **Pagine su HTTP request/response** (373-375?): **esempi di campi dell'header** che usa il libro (il prof ha detto di ricordarseli) e l'elenco dei **codici di risposta** riportati.
- [ ] **Pag. 376**: **proxy HTTP** e **sicurezza con HTTPS** (controllare cosa dice il libro su proxy + HTTPS).
- [ ] **Pag. 378-381**: tutta la parte su **SMTP** (MUA/MSA/MTA/MDA, comandi, sicurezza). Negli appunti la definizione degli agenti si interrompeva dopo MSA: verificare che i termini coincidano con il libro.
- [ ] **Pag. 382**: parte iniziale su **MIME**.
- [ ] **Pag. 384-385**: **POP3 e IMAP4** (da studiare tutte: controllare comandi POP3 e l'elenco delle caratteristiche di IMAP a pag. 385).
- [ ] Argomento **TCP/IP** per le domande a crocette: non ci sono appunti su TCP/IP in questo file, servono le pagine/argomenti indicati dal prof.
