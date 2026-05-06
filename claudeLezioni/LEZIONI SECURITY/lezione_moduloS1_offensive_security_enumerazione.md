# Lezione S1 — Offensive Security I: Enumerazione

**Corso**: Lab Sicurezza Informatica T  
**Modulo**: S1  
**Fonti**: `05_05__va_pt.pdf`, `__ LAB __ Enumerazione [25 febbraio] _ Virtuale.pdf`, `Istruzioni per la configurazione delle VM _ Virtuale.pdf`, `02_03__lab-intro-2026.pdf`

---

## 0. Setup della VM — Prima di tutto

### 0.1 Configurazione di VirtualBox

**Creare la rete host-only** (una volta sola):
```
VirtualBox → File → Tools → Network Manager → Crea vboxnet0
```
- Indirizzo: 192.168.56.1/24
- DHCP: lasciare abilitato o configurare manualmente

### 0.2 Creare la VM attaccante (Kali/Parrot)

**Sul laboratorio fisico** (sequenza esatta):
```bash
vboxmanage setproperty machinefolder "/home/LABS/$(whoami)/large/VirtualBox VMs"
```

Poi da VirtualBox GUI:
1. `Machine → New`
2. RAM: **8 GB**, CPU: **4 core**, Video: **128 MB**
3. Scheda di rete: **NAT** (per internet) + **Host-only vboxnet0** (per i target)
4. Disco: seleziona il file `.vdi` da `/opt/owa/`

**A casa** — due opzioni:
- **Opzione 1**: importa l'appliance con `File → Import Appliance`
- **Opzione 2**: scarica Kali o Parrot base e installa i pacchetti del corso:

```bash
sudo apt install docker-compose aide suricata gdb gcc-multilib linux-headers-amd64 netcat-traditional
git clone https://github.com/Mebus/cupp.git
sudo systemctl enable --now ssh
```

### 0.3 Snapshot obbligatorio prima di ogni esercizio

**Regola del corso**: prima di avviare qualsiasi attività offensiva, scatta uno snapshot.

```
VirtualBox → seleziona la VM → Ctrl+Shift+S → dai un nome (es. "prima-di-S1-lab")
```

Per **ripristinare** uno snapshot: `Ctrl+Shift+R`

Lo snapshot funziona tramite un file delta: il `.vdi` originale rimane read-only, tutte le modifiche vanno nel delta. Il ripristino azzera il delta e ricomincia da capo.

### 0.4 Credenziali e accesso

| Sistema | Username | Password |
|---------|----------|----------|
| Parrot OS | `parrot` | `parrot` |
| Kali Linux | `kali` | `kali` |

Per comandi amministrativi: `sudo <comando>`.

**Rilascio mouse/tastiera dalla VM**: tasto CTRL destro, oppure CTRL+ALT sinistro.

### 0.5 Rete della VM

```bash
ip a
```

La VM ha due interfacce (il nome esatto può variare — `eth0`/`eth1` o `enp0s3`/`enp0s8`):
- **Prima interfaccia** → `10.0.2.15` — NAT, uscita verso internet
- **Seconda interfaccia** → `192.168.56.N` — Host-only, comunica con i target del lab

---

## Contesto: perché fare offensive security

Per difendersi efficacemente occorre conoscere le tecniche di attacco. Le due attività principali sono:

- **Vulnerability Assessment (VA)**: identificare le vulnerabilità di un sistema — ampio, meno profondo, output = lista di rischi
- **Penetration Test (PT)**: verificare se le vulnerabilità sono sfruttabili in pratica — mirato, più profondo, output = catena di compromissione dimostrata

Entrambi richiedono **autorizzazione esplicita** prima di procedere.

Le fasi di un attacco reale seguono la **Kill Chain**:
```
Reconnaissance → Weaponization → Delivery → Exploitation → Installation → C2 → Actions
```
Questo lab copre la fase di **Reconnaissance**: raccogliere informazioni senza ancora compromettere nulla.

---

## Esercizio 1 — Predisporre i target

Crea le 3 VM target da VirtualBox:

```
File → Import Appliance (o Machine → New con disco esistente)
Seleziona i .vdi: Appliance-disk001.vdi, 002.vdi, 003.vdi (in /opt/owa/)
Linux Debian 64-bit | 1 core | 1024 MB RAM
Rete: host-only vboxnet0
```

Scatta uno snapshot per ciascuna prima di avviarle: `Ctrl+Shift+S → "pre-lab-S1"`

Avvia tutte e 3 le VM. Non hai bisogno di fare login su di esse — girano in background.

---

## Teoria — Host discovery: trovare chi è in rete

Prima di attaccare, devi sapere **quali host sono attivi** nella subnet.

**Ping scan con Nmap**: invia pacchetti ICMP a ogni IP del range e aspetta risposta.
```bash
nmap -sn 192.168.56.0/24   # -sn = solo ping, nessuna scansione porte
```

Problema: alcuni host ignorano i ping (firewall blocca ICMP). Su reti locali si può usare:
```bash
arping 192.168.56.32        # usa ARP invece di ICMP — più affidabile su LAN
```

---

## Esercizio 2 — Scoprire gli host attivi

```bash
nmap -sn 192.168.56.0/24
```

Dovresti trovare 3 host attivi: `.32`, `.33`, `.34` (più il tuo gateway `.1`).

---

## Teoria — Porte e servizi: cosa ascolta un host

Ogni servizio in esecuzione su un host è in ascolto su una **porta** TCP o UDP. Le porte well-known (0–1023) sono standardizzate:

| Porta | Servizio |
|-------|----------|
| 22/TCP | SSH |
| 25/TCP | SMTP |
| 53/TCP+UDP | DNS |
| 80/TCP | HTTP |
| 443/TCP | HTTPS |
| 3306/TCP | MySQL |
| 5432/TCP | PostgreSQL |

File locale con il mapping completo: `/etc/services`

**Tipi di scan Nmap**:

| Flag | Tipo | Come funziona |
|------|------|---------------|
| `-sT` | TCP connect | Completa il three-way handshake — visibile nei log |
| `-sS` | SYN scan | Manda solo SYN, non completa — più stealth |
| `-sV` | Version detection | Banner grabbing: legge la stringa che il servizio invia all'apertura |
| `-O` | OS detection | Fingerprinting del SO |
| `-p-` | All ports | Scansiona tutte le 65535 porte (default: solo le 1000 più comuni) |

---

## Esercizio 3 — Scansione porte e servizi

**Step 1**: scansiona tutte le porte TCP dei 3 target
```bash
nmap -sT 192.168.56.32-34 -p-
```

**Step 2**: identifica le versioni dei servizi sulle porte aperte trovate
```bash
nmap -sV 192.168.56.32-34 -p [porte_trovate]
```

**Risultato atteso**:

| Host | IP | Servizi |
|------|----|---------|
| t-1 | 192.168.56.32 | 22/SSH, 80/HTTP, 3306/MySQL(MariaDB), 5432/PostgreSQL |
| t-2 | 192.168.56.33 | 22/SSH, 25/SMTP, 110/POP3, 143/IMAP, 993/IMAPS, 995/POP3S, 1337/SSH |
| t-3 | 192.168.56.34 | 22/SSH, 53/DNS, 80/HTTP, 139+445/Samba, 8000/login-prompt, 8001/Werkzeug |

**Nota su t-2 porta 1337**: nmap la mostra come "waste" ma è SSH camuffato su porta non-standard. Lo verifichi nella sezione successiva.

---

## Teoria — Banner grabbing e misconfigurazioni

Quando un servizio accetta una connessione, spesso invia un **banner**: una stringa di testo con informazioni sul servizio (versione, nome, a volte messaggi personalizzati).

```bash
nc <ip> <porta>   # apre una connessione TCP raw e mostra il banner
```

Una **misconfiguration** comune: un database esposto direttamente su rete senza controllo di accesso. Un database MySQL/PostgreSQL in ascolto su 3306/5432 raggiungibile da qualsiasi IP è una vulnerabilità critica — chiunque può connettersi.

---

## Esercizio 4 — Misconfigurazioni

**Banner grabbing SMTP** su t-2:
```bash
nc 192.168.56.33 25
```
Leggi il banner — rivela un'informazione utile sul database Postgres.

**Connessione diretta a PostgreSQL** esposto su rete (t-1):
```bash
psql -U admin -h 192.168.56.32 -W -l
# password: admin
```
Una volta dentro:
```sql
\dt                       -- lista le tabelle
SELECT * FROM accounts;   -- dump degli account
```

---

## Teoria — Accesso ai servizi e brute force

Dopo l'enumerazione si tenta l'accesso ai servizi scoperti con le credenziali trovate.

Se la password non è nota ma si conosce il formato (es. PIN a 4 cifre numeriche), si usa il **brute force**: tentare sistematicamente tutte le combinazioni possibili.

**Hydra** è il tool standard per brute force su protocolli di rete (SSH, FTP, HTTP, ecc.):

```bash
hydra -l <username> -x <min>:<max>:<charset> <protocollo>://<host>:<porta>
```

Il parametro `-x min:max:charset`:
- `4:4` → lunghezza fissa 4 caratteri
- `1` → charset = solo cifre (0–9)
- Prova tutte le combinazioni: 0000, 0001, ..., 9999

---

## Esercizio 5 — Accesso con credenziali e brute force

**Prova SSH su tutti gli endpoint** con le credenziali trovate nel database:
```bash
ssh turing@192.168.56.32
ssh turing@192.168.56.33
ssh turing@192.168.56.33 -p 1337
ssh turing@192.168.56.34
```

Una volta dentro, cerca il file nota:
```bash
cat /home/turing/note.txt   # contiene un suggerimento: PIN a 4 cifre
```

**Brute force SSH** sulla porta 1337 (PIN a 4 cifre, solo numeri):
```bash
hydra -l root -x 4:4:1 ssh://192.168.56.33:1337
```

---

## Teoria — Hash cracking e /etc/shadow

Linux non salva le password in chiaro. Le salva come **hash** in `/etc/shadow` (leggibile solo da root):

```
/etc/passwd  → username + info utente (world-readable)
/etc/shadow  → password hash (solo root)
```

Formato di una riga in shadow: `$6$VjDM2ItuaSNPBxfO$fingerprint...`
- `$6$` → algoritmo SHA-512
- Parte tra secondo e terzo `$` → **salt** (valore casuale aggiunto prima dell'hash per impedire rainbow tables)
- Resto → hash della password salata

Il **cracking** funziona così: per ogni parola della wordlist, calcola `hash(parola + salt)` e confronta con l'hash nel file. Se coincide, hai trovato la password.

**CUPP** genera wordlist personalizzate basate su informazioni personali del target (nome, data di nascita, ecc.) — efficace perché le persone usano password prevedibili.

```bash
cd ~/cupp && python3 cupp.py -i   # interactive: risponde a domande sul target
```

**John the Ripper** esegue il cracking:
```bash
unshadow passwd.bak shadow.bak > combined.txt   # combina i due file
john --wordlist=wordlist.txt combined.txt        # attacco con wordlist
john --show combined.txt                         # mostra le password trovate
```

---

## Esercizio 6 — Hash cracking

```bash
# Trova i file backup nella home di root
ls /root/
# Attesi: passwd.bak, shadow.bak

# Genera wordlist personalizzata con CUPP
cd ~/cupp && python3 cupp.py -i

# Combina e cracka
unshadow passwd.bak shadow.bak > combined.txt
john --wordlist=wordlist.txt combined.txt
john --show combined.txt
```

---

## Appendice — Strumenti di ricognizione passiva

Tecniche OSINT (Open Source Intelligence): raccolta informazioni da fonti pubbliche, **senza interagire con i sistemi target** → non lascia tracce nei log.

**Google Dorking**:
| Operatore | Esempio |
|-----------|---------|
| `site:` | `site:unibo.it filetype:pdf` |
| `filetype:` | `filetype:xls password` |
| `intitle:` | `intitle:"index of" /backup` |

**Enumerazione DNS**:
```bash
dig target.com MX          # record mail server
dig axfr target.com @ns1   # zone transfer (misconfiguration)
dnsrecon -d target.com     # enumerazione automatica
```

**Strumenti online**: Shodan.io, dnsdumpster.com, crt.sh (Certificate Transparency per subdomini)

**MITRE ATT&CK**: framework che cataloga tattiche e tecniche degli attaccanti reali. Ogni tecnica ha un ID (es. T1566 = Phishing), descrizione, mitigazioni e detection rules.

---

## Autoverifica

1. Qual è la differenza tra VA e PT? Quando si usa l'uno e quando l'altro?
2. In quale fase della kill chain si colloca l'enumerazione DNS?
3. Cosa rivela un zone transfer DNS mal configurato?
4. Perché `nmap -sS` è più "stealth" di `nmap -sT`?
5. Come funziona il meccanismo di snapshot in VirtualBox? Cosa succede al `.vdi` originale?
6. Se trovi la porta 1337/TCP aperta con banner "waste", come determini che in realtà è SSH?
7. Qual è il formato del hash in `/etc/shadow`? Identifica salt e hash nella stringa `$6$VjDM2ItuaSNPBxfO$abc123...`
8. Perché Certificate Transparency è utile per la subdomain enumeration?
