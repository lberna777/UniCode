---
description: "Crea la lezione strutturata per un modulo dai PDF Virtuale. Uso: /lezione <ID>  (es. /lezione 3A, /lezione D1, /lezione S4)"
argument-hint: "ID modulo — SysAdmin: 0A-3F | Security: S1-S12 | Diritto: D1-D8"
---

Il modulo richiesto è: $ARGUMENTS

**Rileva il tipo di modulo dal prefisso dell'ID:**
- ID inizia con `D` → modulo **Diritto** (teoria, nessuna VM)
- ID inizia con `S` → modulo **Security** (lab Kali Linux)
- ID inizia con cifra → modulo **SysAdmin** (lab Vagrant/Debian)
- ID vuoto o non riconosciuto → mostra i formati validi e fermati

---

**1. Leggi la master map**
Leggi `/home/lorenzo/UniCode/master_map_studio.md` e individua il modulo `$ARGUMENTS`.
Estrai: nome completo, corso, PDF richiesti, concetti chiave.
Se il modulo non esiste nella master map, comunicalo e fermati.

---

**2. Verifica i PDF**

- Moduli SysAdmin: cerca in `/home/lorenzo/UniCode/SLIDE TEORIA/SysAdmin/` e `/home/lorenzo/UniCode/SLIDE LAB/`
- Moduli Security: cerca in `/home/lorenzo/UniCode/SLIDE TEORIA/SysAdmin/` e `/home/lorenzo/UniCode/SLIDE LAB/`
- Moduli Diritto: cerca in `/home/lorenzo/UniCode/SLIDE TEORIA/DIRITTO INFORMATICO/`

Se uno o più PDF richiesti dal modulo mancano: **fermati qui**, elenca i nomi esatti e chiedi a Lorenzo di caricarli. Non creare contenuto didattico senza il PDF corrispondente.

---

**3. Leggi i PDF**
Leggi integralmente tutti i PDF rilevanti trovati al passo precedente.

---

**4. Crea il file lezione**

Path: `/home/lorenzo/UniCode/claudeLezioni/lezione_modulo$ARGUMENTS_<nome_breve>.md`
`<nome_breve>` = identificatore conciso del contenuto (es. `systemd_servizi`, `diritto_autore`, `web_security`).

---

### Template SysAdmin (ID numerico)

```
# Lezione — Modulo $ARGUMENTS: <Nome Completo>
**Corso**: Lab Amministrazione di Sistemi T
**Materiale**: <titoli PDF usati>
**Prerequisiti**: <moduli precedenti rilevanti>

---

## Obiettivo
Una frase: cosa Lorenzo deve saper fare sulla VM al termine di questa lezione.

## Concetti Chiave
Per ogni concetto: definizione, perché esiste, come si usa in pratica su Debian.

## Comandi di Riferimento
| Comando | Sintassi | Descrizione | Output atteso |
|---------|----------|-------------|---------------|

## Esercizi Guidati
Sequenza numerata di comandi da eseguire sulla VM, con output atteso dopo ogni comando significativo.

## Connessioni
- Con il modulo precedente: ...
- Con Security: quale superficie d'attacco introduce ...

## Riepilogo
- Concetto 1
- Concetto 2
- Concetto 3
```

---

### Template Security (prefisso S)

```
# Lezione — Modulo $ARGUMENTS: <Nome Completo>
**Corso**: Lab Sicurezza Informatica T
**Materiale**: <titoli PDF usati>
**Prerequisiti**: <moduli SysAdmin e Security rilevanti>

---

## Obiettivo
Una frase: cosa Lorenzo deve saper fare/riconoscere al termine.

## Contesto e Threat Model
Perché questo attacco o difesa esiste. Dal punto di vista dell'attaccante e del difensore.

## Concetti Chiave
Per ogni concetto: definizione tecnica, come si manifesta, esempio reale documentato.

## Tool e Comandi
| Tool | Comando | Scopo | Output tipico |
|------|---------|-------|---------------|

## Esercizi Guidati
Sequenza su Kali Linux.
> ⚠️ Esegui uno snapshot della VM prima di iniziare.

## Connessioni
- Con SysAdmin: quale configurazione errata viene sfruttata
- Con moduli Security precedenti/successivi: ...

## Riepilogo
- Concetto 1
- Concetto 2
- Concetto 3
```

---

### Template Diritto (prefisso D)

> **Regola vincolante per i moduli Diritto**: l'esame verte strettamente sugli argomenti e sulle spiegazioni contenute nei PDF della professoressa. Le definizioni, le classificazioni e le formulazioni devono rispecchiare fedelmente il linguaggio usato nel PDF — non riformulare, non parafrasare, non integrare con fonti esterne. Se la professoressa definisce un istituto in un certo modo, quella è la definizione da usare. Segnala esplicitamente con `[fonte: PDF]` ogni affermazione tratta direttamente dalle slide. Il registro è accademico-giuridico: preferire paragrafi discorsivi alle liste dove il PDF lo fa, e usare la terminologia tecnica esatta del testo.

```
# Lezione — Modulo $ARGUMENTS: <Nome Completo>
**Corso**: Diritto dell'Informatica T
**Materiale**: <titolo PDF usato>
**Normative di riferimento**: <leggi e decreti citati nel PDF>

---

## Obiettivo
Una frase: quale istituto giuridico Lorenzo deve saper spiegare al termine, nelle parole usate dalla professoressa.

## Quadro Normativo
Norme di riferimento citate nel PDF, con estremi completi (es. L. 633/1941, D.Lgs. 70/2003, Reg. UE 2016/679). Solo quelle presenti nel materiale.

## Concetti Chiave
Per ogni concetto: definizione ripresa fedelmente dal PDF, ratio legis se spiegata dalla professoressa, esempi usati nelle slide. Non aggiungere contenuto non presente nel PDF.

## Riferimenti Normativi
| Articolo / Norma | Contenuto (come descritto nel PDF) | Rilevanza per il corso |
|------------------|------------------------------------|------------------------|

## Casi e Scenari
Situazioni concrete usate dalla professoressa nelle slide per illustrare i concetti. Se non presenti nel PDF, omettere questa sezione.

## Domande di Autoverifica
Cinque domande aperte formulate sugli argomenti del PDF, del tipo che potrebbe fare la professoressa all'esame.
1. ...
2. ...
3. ...
4. ...
5. ...

## Riepilogo
Tre concetti normativi centrali del modulo, formulati come li ha presentati il PDF.
- ...
- ...
- ...
```

---

**5. Aggiorna la master map**
Segna il modulo `$ARGUMENTS` come 🔄 se era ⬜.

**6. Comunica il risultato**
- Path del file creato
- Per SysAdmin/Security: indica di avviare la VM e seguire gli esercizi guidati
- Per Diritto: indica di leggere la lezione e rispondere alle domande di autoverifica prima di scrivere gli appunti grezzi
