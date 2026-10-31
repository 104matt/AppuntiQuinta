# SR Comandi Pratica

## Navigazione tra le modalità

```
Switch> enable                  ! (en) entra in modalità privilegiata
Switch# configure terminal      ! (conf t) entra in configurazione globale
Switch(config-if)# exit         ! torna indietro di un livello
Switch(config)# end             ! torna direttamente alla modalità privilegiata
```

## Impostazioni di base

```
Router(config)# hostname R0                          ! nome del dispositivo
R0(config)# banner motd "Messaggio di benvenuto"     ! messaggio all'accesso
R0(config)# enable secret Pascal2026!                ! password modalità privilegiata (cifrata)
R0(config)# service password-encryption              ! cifra le password in chiaro nella config
```

## Salvataggio e verifica

```
R0# copy run start      ! salva running-config in startup-config
R0# show users          ! utenti collegati
R0# show ip ssh         ! stato del servizio SSH
```

## Routing

**Statico**

```
Router(config)# ip route [rete_destinazione] [mask] [ip_next_hop]
```

**Dinamico (RIP)**: si dichiarano le reti direttamente connesse

```
Router(config)# router rip
Router(config-router)# network [rete_diretta_1]
Router(config-router)# network [rete_diretta_2]
Router(config-router)# exit
```

## TELNET su router

```
R0(config)# line vty 0 4                  ! linee per accessi remoti (0-4)
R0(config-line)# password TenetLab!       ! password di accesso
R0(config-line)# login                    ! richiede la password
R0(config-line)# transport input telnet   ! accetta solo telnet
R0(config-line)# exec-timeout 5 0         ! disconnette dopo 5 min e 0 s di inattività
```

## TELNET su switch

**IP di gestione sulla VLAN 1**

```
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.0.253 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit
Switch(config)# ip default-gateway 192.168.0.254
```

**Linee VTY**

```
Switch(config)# line vty 0 4
Switch(config-line)# password cisco
Switch(config-line)# login
Switch(config-line)# transport input telnet
Switch(config-line)# exec-timeout 5 0
Switch(config-line)# exit
```

## SSH su router

**Prerequisiti**: hostname + dominio + utente + chiavi

```
Router(config)# hostname R0
R0(config)# ip domain-name lab.pascal
R0(config)# username studente privilege 15 secret SshLab2026!    ! 15 = privilegio massimo
R0(config)# crypto key generate rsa general-keys modulus 2048
```

**Parametri SSH**

```
R0(config)# ip ssh version 2
R0(config)# ip ssh time-out 60                 ! secondi per completare l'accesso
R0(config)# ip ssh authentication-retries 2    ! tentativi consentiti
```

**Linee VTY**

```
R0(config)# line vty 0 4
R0(config-line)# login local            ! usa gli utenti creati con username
R0(config-line)# transport input ssh    ! accetta solo SSH
R0(config-line)# exec-timeout 5 0
R0(config-line)# end
```

## Connessione dal PC client

```
C:\> telnet 192.168.1.1              ! TELNET (porta 23)
C:\> ssh -l studente 192.168.0.1     ! SSH verso router (porta 22)
C:\> ssh nome.cognome@biagio         ! SSH verso server
```

| Protocollo | Porta |
| --- | --- |
| FTP | 21 |
| SSH | 22 |
| TELNET | 23 |
