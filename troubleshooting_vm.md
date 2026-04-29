# Troubleshooting VM — Soluzioni ai problemi ricorrenti

Aggiornato a ogni sessione. Prima di chiedere aiuto, cerca qui.

---

## Avvio VM

**Problema**: `vagrant up` fallisce con errore VirtualBox  
**Soluzione**: Verifica che VirtualBox sia aperto. Se la VM è già in stato "saved", prova `vagrant resume` invece di `vagrant up`.

**Problema**: La VM si avvia ma `vagrant ssh` non si connette  
**Soluzione**:
```bash
vagrant halt
vagrant up
vagrant ssh
```

---

## Pacchetti non trovati

**Problema**: `sudo apt install <pacchetto>` → `Unable to locate package`  
**Causa**: La lista dei pacchetti disponibili è vecchia o vuota.  
**Soluzione**:
```bash
sudo apt update
sudo apt install <pacchetto>
```
`apt update` va eseguito sempre prima di installare pacchetti, specialmente dopo un lungo periodo di inattività della VM.

---

## Comandi non trovati

**Problema**: `locate: command not found`  
**Soluzione**: `sudo apt install plocate && sudo updatedb`

**Problema**: `fuser: command not found`  
**Soluzione**: `sudo apt install psmisc`

**Problema**: `lsof: command not found`  
**Soluzione**: `sudo apt install lsof`

---

## Permessi e sudo

**Problema**: `Permission denied` su file in `/home/altroUtente/`  
**Causa**: I file degli altri utenti non sono leggibili da vagrant.  
**Soluzione**: Usare `sudo` per operazioni che richiedono accesso a file di altri utenti:
```bash
sudo tar cvpf archivio.tar /home/*
sudo cat /home/alice/.bashrc
```

**Problema**: `sudo echo "..." >> /path/file` → `Permission denied`  
**Causa**: Il redirect `>>` viene gestito dalla shell di vagrant, non da sudo.  
**Soluzione**: Usare `tee`:
```bash
echo "contenuto" | sudo tee -a /path/file
```

**Problema**: Utente aggiunto a un gruppo con `usermod -aG` ma il gruppo non risulta attivo  
**Causa**: La modifica ai gruppi è attiva solo alla prossima sessione di login.  
**Soluzione**: Fare logout e re-login, oppure:
```bash
newgrp nomegruppo
```

---

## Filesystem e directory

**Problema**: `tar: /newdisk: Cannot open: No such file or directory`  
**Causa**: La directory di destinazione non esiste.  
**Soluzione**: Crearla prima:
```bash
sudo mkdir /newdisk
tar -C /newdisk -xvpf archivio.tar
```

**Problema**: `rmdir: failed to remove 'dir': Directory not empty`  
**Soluzione**: Usare `rm -r nomedir` per cancellare ricorsivamente (attenzione: irreversibile).

---

## Terminale e editor

**Problema**: `crontab -e` → `Error opening terminal: xterm-kitty`  
**Causa**: Il terminale Kitty usa `TERM=xterm-kitty`, non riconosciuto dalla VM.  
**Soluzione rapida**:
```bash
TERM=xterm-256color crontab -e
```
**Soluzione permanente** (aggiungere al `.bashrc` sulla VM):
```bash
echo 'export TERM=xterm-256color' >> ~/.bashrc
source ~/.bashrc
```

---

## Processi in background

**Problema**: Processi rimasti in stato `Stopped` nel job control (visibili con `jobs`)  
**Causa frequente**: Comando avviato con `&` ma poi interrotto, oppure tentativo di usare un comando interattivo come `systemctl` senza argomenti in background.  
**Soluzione**: Killare i job fermi per indice:
```bash
kill %1 %2   # sostituire con i numeri mostrati da jobs
```

---

## Cartella condivisa /vagrant

**Problema**: `chmod +x /vagrant/script.sh` non funziona — il file resta non eseguibile  
**Causa**: `/vagrant` è montata con filesystem `vboxsf` che non supporta i permessi Unix.  
**Soluzione**: Copiare il file nella home prima di dargli i permessi, oppure eseguire direttamente con `bash`:
```bash
cp /vagrant/script.sh ~/
chmod +x ~/script.sh
# oppure, senza copiare:
bash /vagrant/script.sh argomento1 argomento2
```

---

## Errori comuni negli script bash

**Problema**: Script non eseguibile → `bash: ./script.sh: Permission denied`  
**Soluzione**:
```bash
chmod +x script.sh
./script.sh
```

**Problema**: `[: unexpected operator` o errori nei test  
**Causa frequente**: Spazio mancante dopo `[` o prima di `]`, o tra `!` e l'operatore.  
**Sbagliato**: `[!-d dir]` — **Corretto**: `[ ! -d dir ]`

**Problema**: `PID=!$` non cattura il PID del processo in background  
**Causa**: `!$` è history expansion (ultimo argomento del comando precedente), non il PID. La variabile corretta è `$!`.  
**Sbagliato**: `PID=!$` — **Corretto**: `PID=$!`

**Problema**: Incollare il corpo di uno script direttamente nella shell → variabili `$1`, `$THIS` vuote, comportamento inatteso  
**Causa**: Le variabili degli argomenti (`$1`, `$2`) sono vuote se il codice viene eseguito interattivamente invece che come script con argomenti.  
**Soluzione**: Salvare il codice in un file, renderlo eseguibile, e lanciarlo con gli argomenti corretti:
```bash
chmod +x script.sh
./script.sh argomento1 argomento2
```

**Problema**: `kill $PID` non termina un processo in stato `T` (Stopped)  
**Causa**: Un processo sospeso non può processare segnali finché non viene ripreso — SIGTERM viene recapitato ma resta in coda.  
**Soluzione**: Usare SIGKILL (termina anche i processi stopped) oppure riprendere prima il processo:
```bash
kill -KILL $PID           # opzione 1: forza terminazione immediata
kill -CONT $PID && kill $PID  # opzione 2: riprendi, poi termina ordinatamente
```

**Problema**: Script con `EOF` come ultima riga → `command not found: EOF`  
**Causa**: `EOF` è rimasto nel file come artefatto di un copy-paste da heredoc. Bash lo interpreta come comando.  
**Soluzione**: Aprire il file con `nano` e cancellare l'ultima riga contenente solo `EOF`.

**Problema**: Variabile passata a funzione senza `$` → il valore non viene scritto nel file atteso  
**Esempio**: `conta "$2" "FB" &` invece di `conta "$2" "$FB" &` — passa la stringa letterale `"FB"` invece del path del file temporaneo.  
**Soluzione**: Verificare che ogni variabile sia preceduta da `$`. Usare `bash -x script.sh` per vedere i valori espansi riga per riga.

**Problema**: Variabile vuota in una condizione causa errore  
**Causa**: La variabile non è tra virgolette.  
**Sbagliato**: `if [ -z $VAR ]` — **Corretto**: `if [ -z "$VAR" ]`
