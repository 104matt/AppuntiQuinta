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

## PROTOCOLLI SFTP E FTPS

Il modo migliore per implementare un modo sicuro di trasferimento dati a FTP è appoggiarsi su SSH in modo che ci sia una cifratura a livello di dati. Per SFTP (Secure FTP) si usa la porta di SSH che è la 22 e non utilizza ne la modalità passiva ne quella attiva ma trasferisce i file che vengono cifrati da SSH.

**SFTP** è più utilizzato di **FTPS**. Inoltre **FTPS** prevede 2 versioni: quella implicità (che utilizza la porta 990) richiede l'utilizzo di SSL/TLS dal primo contatto con il server; quella esplicita prevede l'instaurazione della connessione come FTP non sicuro (porta 21) ed è il **server** che crea la **connessione sicura** sul canale dati (porta casuale). La **modalità esplicità** è piu utilizzata.

Nelle versioni sicure di un protocollo è stato introdotto tra livello **Application** e il livello **Transport** un livello chiamato **SSL/TLS**.

Nel livello ISO/OSI i livelli **Presentation e Session** gestiscono e coprono la parte di sicurezza.

## PROTOCOLLO HTTP

Per ogni protocollo abbiamo una tabella di informazioni generali che vanno sapute sempre.

| Acronimo                     | Utilità                                 | Porta | Paradigma     | Protocollo Transport | Sicurezza   |
| ---------------------------- | --------------------------------------- | ----- | ------------- | -------------------- | ----------- |
| Hyper Text Transfert Protcol | Trasferimento di contenuti multimediali | 80    | Client/Server | TCP                  | HTTPS (443) |

Lato **client** abbiamo il browser che pwò essere **Chrome** **NON GOOGLE** oppure **Firefox**, mentre a livello **server** come applicativo usiamo **Apache**, **IIS**.

{% hint style="warning" %}
**N.B.**

_Non bisogna confondere il **www** (Organizzazione) e **http** (Protocollo) oppure **Internet** (Insieme di router) e il **www**._
{% endhint %}

{% hint style="info" %}
**URL vs URI**

**URI** (Uniform Resource Identifier): stringa che **identifica** una risorsa, per nome o per posizione (es. `https://ispascalcomandini.com`).
**URL** (Uniform Resource Locator): tipo di URI che dice anche **dove** si trova la risorsa e **come** raggiungerla (es. `https://ispascalcomandini.com/pagine/presentazione`).
Ogni URL è un URI, ma non ogni URI è un URL.
{% endhint %}

### TIPI DI RICHIESTE

#### **HTTP Request C --> S**

All'interno contiene un:

* **Request Line**: GET (1)  /info.html (2) HTTP 1.1 (3)
  * **GET** può essere anche **POST**, **PUT**, **DELETE**, **OPTIONS**
  * **Risorsa/URL** (Uniform Resource Locator)
    * **File** che se non viene specificato va a cercare **index.php o index.html** ma se contiene invece una risorsa diventa un **URI** e ci porta ad essa. Nel caso in cui andiamo a cercare **index.php o index.html** l'URL coinciderà con l'URI e non ci sarà distinzione.
  * **Versione**:
    * Dalla 1.1 abbiamo una connessione persistente, ovvero non viene riavviata una connessione TCP ogni volta che apro una risorsa nel sito come nella 1.0
    * Nella versione 1.0 mi collegavo a un server web e mi scaricavo la prima pagina. Tutte le volte che su quel sito web aprivo risorse nuove veniva tirata giu una connessione TCP per esser tirata su nuovamente. **VEDI TABELLA HTTP**
* **Header**
	* Contiene informazioni aggiuntive riguardo al client.
* **Body**
	* Se faccio una richiesta **GET** sarà vuoto, ma se faccio una **POST** conterrà i dati che vogliamo inviare.
#### **HTTP Response S --> C**
La HTTP Response è composta da 3 parti:
- **Status Line**
	- Troviamo un codice a 3 cifre che ci dice com'è andata a finire la nostra request.
		- **2xx (Success)** la richiesta è stata soddisfatta/la risorsa è stata trovata.
			- 200 OK
		- **3xx (Redirection)** (Non c'è una risposta diretta ma la richiesta è ritenuta corretta e viene detto come ottenere la risposta)
			- 302 Risorsa disponibile su un URL diverso
		- **4xx Errore lato Client**  (La richiesta non può essere soddisfatta perchè sbagliata)
			- 400 Bad request, hai scritto male l'url o l'uri
			- 403 Forbidden, la risorsa esiste ma non hai i permessi per accedervi
			- 404 Not Found, la risorsa richiesta non esiste sul server
		- **5xx Errore lato Server** (La mia richiesta non può essere soddisfatta per un problema interno del server)
			- 500 Internal Server Error, il server ha avuto un errore generico mentre elaborava la richiesta
			- 501 Not Implemented, il server non supporta il metodo richiesto (es. un metodo HTTP che non conosce)
- **Header** 
- **Body** 

### VERSIONI DI HTTP


| Versione                              | Caratteristiche                                                                                              |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **HTTP 1.0**                          | Semplice ma connessioni non persistenti **(1996)**                                                           |
| **HTTP 1.1**                          | Connessioni persistenti (keep-alive), compatibilità con API (1999)                                           |
| **HTTP/2** *(Piu utilizzato ad oggi)* | Più utilizzato -> Multiplexing (Più request e response sulla stessa connessione) Overhead ridotto **(2015)** |
| **HTTP/3**                            | Utilizza QUIC che si appoggia su UDP quindi **piu veloce** e **piu sicuro** perchè integra TLS (2022)        |

#### ESEMPIO HTTP/3


| HTTP(3)       |                                                                                                                                         |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **QUIC**      | Nel mezzo viene messo QUIC (Protocollo ideato da Google) che nel modello ISO/OSI è gestito da application e presentation. Utilizza UDP. |
| **TRANSPORT** |                                                                                                                                         |


{% hint style="info" %}
L'**HTTP** è un protocollo **stateless** ovvero perde la memoria dei contenuti scambiati ad ogni richiesta, oggi riusciamo a mantenere lo stato grazie all'utilizzo dei **Cookie** rendendo il protocollo **stateful**.
{% endhint %}

#### I COOKIE

File di testo memorizzato lato client e tutte le volte che ti colleghi al server, questo viene inviato e interpretato per personalizzare la sessione/analizzare dati/mantenere dati di login etc...



# Proxy HTTP
Un qualcosa che si interpone tra l'applicativo client e l'applicativo server. Un router puo essere utilizzato come proxy, infatte viene chiamato default gateway. 
#### **Utilizzo**
- *connettività*: *Un*** proxy puo esser usato per permettere a una rete privata di accedere a una **pubblica**
- *privacy*: Un proxy puo **mascherare** il vero indirizzo IP del client. Infatti esistono proxy di tue tipologie: **NOA** ( NOn Anonymous Proxy Server), non applciano nessuna misura per mantere la privacy e arrivano al massimo a modificare certi header prima di fare richieste, **ANM** ( Anonymous Proxy Sever ), non trasmettono l'IP del client, ma modificano certi header rendendo riconoscibile il fatto che un proxy sia stato utilizzato. L'ultima categoria è **HIA** (High Anonymous Proxy ) che fa perfettamente il suo lavoro non trasmettendo l'IP del client senza farsi riconoscere. Un sotto-categoria sono i **proxy-distorcenti** che trasmettono un IP **casuale**.
- *caching*: Un proxy puo immagazzinare delle risposte HTTP per un certo tempo in modo che se un altro client richieda la stessa risorsa puo essere servito direttamente il contenuto della cache **senza** dover fare un altra richiesta al server.
- *monitoraggio*: Un proxy puo permettere di tenere traccia di tutto il traffico HTTP fatto (pagine visitate...)
- *amministrazione*: Un proxy puo impostare **regole** per il traffico in entrata e puo applicare anche filtri sulle pagine web visitate.

I **protocolli** che vediamo **sono**:


| Acronimo   | Utilità         | Porta   | Paradigma     | Protocollo Transport | Sicurezza                 |
| ---------- | --------------- | ------- | ------------- | -------------------- | ------------------------- |
| SMTP       | Invio Email     | 25      | Client/Server | TCP                  | SMTPS (465)               |
| POP3/IMAP4 | Ricezione email | 110/143 | Client/Server | TCP                  | POP3 (995) e IMAP4S (993) |
**SMTP**:
- Il client mittente si connette a SMTP porta 25 e la mail viene messa in una Mailbox Serverside che si collegherà via SMTP a un altra Mailbox Server alla quale si collegherà il client destinatario con IMAP porta 143 e riceverà la mail.

Il protocollo SMTP usa i seguenti termini per riferirsi al software utilizzato su client e server:
- **MSA**: Risiede sul mail server tramite il quale invia l'email il **MUA**
- 
- 