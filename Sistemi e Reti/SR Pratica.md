# SR Pratica

## Livello Application

**Funzione:** fornisce servizi di rete direttamente alle applicazioni utente.

Per accedere a un servizio esterno di solito si passa dal browser, ma se la parte web non è disponibile si può accedere tramite protocolli di livello Application come **TELNET** e **SSH**.

TELNET e SSH sono protocolli per gestire un computer da remoto:

- **TELNET** trasmette tutto in chiaro ed è poco sicuro.
- **SSH** cifra tutto il traffico (i pacchetti non possono essere letti se intercettati / "sniffati").

Con questi protocolli si può accedere anche ad apparati di rete (router, switch) per configurarli, proteggerli, ecc. Sono comodi quando le macchine hanno sistemi operativi diversi; se hanno lo stesso SO si può usare il **Desktop remoto**, un applicativo che serve per collegarsi graficamente a un'altra macchina.

Altri applicativi simili sono ad esempio **TeamViewer**: se entrambi gli host lo installano, uno può accedere all'altro e lavorare sul suo PC.

---

# SSH e TELNET

## Introduzione ai protocolli di gestione remota

SSH e TELNET sono due protocolli di livello Application del modello TCP/IP utilizzati per accedere alla riga di comando (CLI) di apparati remoti (router, switch, server).

| Protocollo | Porta di default | Livello di sicurezza | Descrizione |
| --- | --- | --- | --- |
| TELNET | 23 | Non sicuro | Trasmette la sessione e le credenziali in chiaro (testo visibile sul canale). |
| SSH (Secure Shell) | 22 | Elevato | Cifra tutto il traffico tramite chiavi crittografiche (generate a partire da Nome Dominio + Hostname). |

> Le porte "well-known" come 22 e 23 sono standardizzate e assegnate dalla **IANA**.

### Come funziona la cifratura in SSH

Quando i pacchetti viaggiano in rete la password è cifrata. Il pacchetto cifrato si ottiene combinando il pacchetto con una **chiave** (una sequenza di caratteri casuali), generata a partire dal **nome dominio** (es. `lab.pascal`, per indicare tutti i laboratori) a cui viene aggiunto l'**hostname**.

```
Packet (TelnetLab!) + Key (h7bc4a...) => Packet encrypted
Key = Funz(Nome Dominio + Hostname)
```

## Linee VTY

**VTY** (Virtual TeleType / Virtual Terminal) sono le "porte" virtuali usate da chi si collega da remoto. `line vty 0 4` configura le 5 linee (da 0 a 4), quindi fino a 5 connessioni remote contemporanee. La linea è aperta, ma chi si collega deve inserire la password (se impostata).

## Procedura completa di laboratorio

### 0) Routing

Impostare il routing (statico o dinamico, a scelta) all'interno dei router presenti. Vedi la sezione [Routing](#routing).

### 1) Impostazioni di base: hostname, password privilegiata, banner

```
Router(config)# hostname R0
R0(config)# banner motd "Accesso consentito solo a quelli del pascal"
R0(config)# enable secret Pascal2026!
R0(config)# service password-encryption
```

### 2) Attivazione del servizio TELNET

```
R0(config)# line vty 0 4
R0(config-line)# password TenetLab!
R0(config-line)# login
R0(config-line)# transport input telnet
R0(config-line)# exec-timeout 5 0
```

### 3) Collegarsi a un router tramite TELNET da un PC

```
C:\> telnet [ip router]
C:\> telnet 192.168.1.1
```

### 4) Attivazione del servizio SSH

> Il router deve avere un hostname impostato (comando `hostname nomeRouter`), altrimenti non è possibile generare le chiavi.

```
R0(config)# ip domain-name lab.pascal
R0(config)# username studente privilege 15 secret SshLab2026!
R0(config)# crypto key generate rsa general-keys modulus 2048
R0(config)# ip ssh version 2
R0(config)# ip ssh time-out 60
R0(config)# ip ssh authentication-retries 2
R0(config)# line vty 0 4
R0(config-line)# login local
R0(config-line)# transport input ssh
R0(config-line)# end
```

- `ip ssh time-out 60` → si hanno 60 secondi per completare l'accesso.
- `ip ssh authentication-retries 2` → numero di tentativi di autenticazione consentiti.
- `login local` → l'autenticazione usa gli utenti creati con `username` sul router.

### 5) Collegarsi a un router con SSH da un PC

```
C:\> ssh -l studente 192.168.0.1
```

Inserire la password dell'utente `studente` impostata sopra. Una volta dentro:

```
R0# show users      ! mostra gli utenti collegati
R0# show ip ssh     ! mostra lo stato del servizio SSH
```

## Configurazione SSH su apparati Cisco (riepilogo)

Per abilitare SSH su un router o switch Cisco è necessario configurare un nome dominio, generare le chiavi RSA e definire un utente con i relativi privilegi.

```
! Impostazione nome dominio e hostname
Router(config)# hostname RouterLab
RouterLab(config)# ip domain-name lab.pascal

! Creazione utente con livello di privilegio massimo (15)
RouterLab(config)# username admin privilege 15 secret MiaPasswordSicura

! Generazione delle chiavi crittografiche RSA
RouterLab(config)# crypto key generate rsa
! Suggerimento: impostare una lunghezza chiave di almeno 1024 o 2048 bit

! Configurazione delle linee VTY per accettare solo SSH
RouterLab(config)# line vty 0 4
RouterLab(config-line)# login local
RouterLab(config-line)# transport input ssh
RouterLab(config-line)# exec-timeout 5 0
RouterLab(config-line)# exit
```

## Configurazione TELNET su switch Cisco

Gli switch Layer 2 non hanno un indirizzo IP sulle porte: per poter essere raggiunti da remoto bisogna assegnare un IP a un'interfaccia virtuale (**SVI**, di solito la VLAN 1).

```
! Assegnazione IP alla VLAN 1 (interfaccia di gestione)
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.1.2 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit

! Configurazione del default gateway (in configurazione globale)
Switch(config)# ip default-gateway 192.168.1.1

! Configurazione linee VTY per TELNET
Switch(config)# line vty 0 4
Switch(config-line)# password PasswordTelnet
Switch(config-line)# login
Switch(config-line)# exec-timeout 5 0
Switch(config-line)# exit
```

> `no shutdown` (abbreviato `no shut`) serve ad attivare l'interfaccia VLAN.

### Esempio reale da Packet Tracer

```
Switch>en
Switch#conf t
Switch(config)#hostname Switch4
Switch4(config)#interface vlan 1
Switch4(config-if)#ip address 192.168.0.3 255.255.255.0
Switch4(config-if)#no shut
%LINK-3-UPDOWN: Interface Vlan1, changed state to down
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
Switch4(config-if)#exit
Switch4(config)#ip default-gateway 192.168.0.254
Switch4(config)#line vty 0 4
Switch4(config-line)#password cisco
Switch4(config-line)#login
Switch4(config-line)#transport input telnet
Switch4(config-line)#exit
Switch4(config)#enable secret class
Switch4(config)#end
Switch4#copy run start
Destination filename [startup-config]?
Building configuration...
[OK]
```

> `copy run start` salva la configurazione in esecuzione (running-config) nella memoria permanente (startup-config).

## Comandi utili e parametri aggiuntivi

- `banner motd "Messaggio di benvenuto"` → configura un messaggio visualizzato all'accesso.
- `exec-timeout 5 0` → disconnette la sessione inattiva dopo **5 minuti e 0 secondi**. Si inserisce dentro `line vty 0 4`:

  ```
  Router(config)# line vty 0 4
  Router(config-line)# exec-timeout 5 0
  ```

- `privilege 0-15` → livelli di privilegio Cisco (**0 = minimo**, **15 = massimo / amministratore**):

  ```
  Router(config)# username admin privilege 15 secret MiaPassword
  ```

## Modalità di connessione dal client

Per connettersi da un computer client a un apparato di rete (router, switch, server) si può usare la riga di comando oppure un software grafico.

### Connessione tramite terminale / Prompt dei comandi (CMD)

Metodo diretto, senza installare software aggiuntivi.

**Connessione SSH**

- Sintassi: `ssh nome_utente@host` (host = hostname o indirizzo IP)
- Esempio: `ssh marco.ravaioli@biagio`
- Procedura: si digita il comando specificando utente e host di destinazione. Il sistema chiede la password dell'utente e stabilisce la sessione cifrata.

**Connessione TELNET**

- Sintassi: `telnet indirizzo_ip`
- Esempio: `telnet 192.168.1.2`
- Procedura: si inserisce l'IP del dispositivo. A differenza di SSH, le credenziali (username e password) vengono richieste in un secondo momento e trasmesse **in chiaro**.

### Connessione tramite client grafico (es. PuTTY)

1. **Avvio e indirizzo:** aprire PuTTY e digitare l'indirizzo IP (o hostname) del dispositivo nel campo *Host Name (or IP address)*.
2. **Protocollo e porta:**
   - selezionando **SSH** la porta si imposta automaticamente a **22**;
   - selezionando **Telnet** la porta si imposta automaticamente a **23**.
3. **Avvio della sessione:** cliccare su **Open** e inserire le credenziali nella finestra di terminale che si apre.

### Accesso al server "biagio"

- **Da PuTTY:** inserire come hostname `biagio` ed entrare. Accettare la chiave, poi uscire e rientrare (**solo la prima volta**). Fare l'accesso con username e password.
- **Da CMD:** scrivere `ssh nome.cognome@biagio`, rispondere `yes` e inserire la password.

---

# Routing

Per collegare più reti:

- inserire gli IP degli apparati nei PC;
- impostare come **default gateway** l'IP della porta del router che si affaccia sulla rete interna;
- assegnare gli IP "pubblici" alle porte che vanno verso gli altri router;
- configurare il routing.

## Routing statico

In Packet Tracer: scheda **Config → Static** del router, inserendo indirizzo di rete e subnet mask della rete **di destinazione** e il **next hop** (IP del prossimo router). Oppure da CLI:

```
Router(config)# ip route [rete_destinazione] [mask] [ip_next_hop]
```

## Routing dinamico (RIP)

Si dichiarano le reti **direttamente connesse** al router:

```
Router(config)# router rip
Router(config-router)# network [indirizzo_rete_diretta_1]
Router(config-router)# network [indirizzo_rete_diretta_2]
Router(config-router)# exit
```

---

# FTP

Configurazione di un server FTP con **FileZilla** su macchine virtuali.

> Per far navigare online le VM bisogna andare nelle impostazioni **Rete** della macchina virtuale e selezionare la scheda di rete adatta.

## Macchina server

1. Avviare **"Amministra FileZilla Server"**.
2. Assicurarsi di avere la **rete interna** o la **scheda con bridge**.
   > Quando si cambia scheda di rete bisogna reinserire gli IP manualmente, perché quelli impostati prima vengono persi.
3. **Configurazione:** *Connetti al server* → **OK** → *Server* → *Configura* → **FTP and FTP over TLS** → *Users* → **Add** (in fondo a sinistra).
4. Aggiungere nome utente e password.
5. Copiare il percorso della cartella FTP creata prima e inserirlo in **Native path**; in **Virtual path** inserire `/`.

## Macchina client

1. Scaricare e aprire **FileZilla Client**.
2. Compilare:
   - **Host:** IP del server
   - **Porta:** `21`
   - **Nome utente e password:** quelli dell'utente configurato sul server
3. Cliccare su **Connessione rapida** e premere **OK** in entrambe le finestre che compaiono.
4. Nella sezione **Sito remoto** dovrebbe comparire il percorso delle cartelle impostato sul server.
5. Per trasferire un file basta **trascinarlo** nella cartella desiderata.

> Da riga di comando FTP si può usare `dir` per verificare che il file sia stato caricato.

---

# Analisi del traffico dal browser

1. Aprire la pagina del laboratorio (sezione *Sistemi e Reti*).
2. Tasto destro → **Ispeziona** → schede **Network** / **Performance**.
3. Attivare **Persist logs** (*keep log*) e **Disable cache**.
4. Cliccando su una richiesta compaiono i dettagli del traffico che è passato.
5. Tasto destro sulla richiesta → **Edit and Resend** / **Copy as fetch** per modificarla e reinviarla.

> Si usa l'indirizzo `10.1.1.1` perché bisogna collegarsi al server, e l'IP del server è `10.1.1.1`.
