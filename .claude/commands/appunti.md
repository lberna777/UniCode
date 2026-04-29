---
description: "Elabora gli appunti grezzi di un modulo in appunti definitivi e aggiorna la master map. Uso: /appunti <ID>  (es. /appunti 3A, /appunti D1, /appunti S4)"
argument-hint: "ID modulo — SysAdmin: 0A-3F | Security: S1-S12 | Diritto: D1-D8"
---

Il modulo da elaborare è: $ARGUMENTS

**Rileva il tipo di modulo dal prefisso dell'ID:**
- ID inizia con `D` → modulo **Diritto** — cartella grezzi: `APPUNTI GREZZI/Diritto/`
- ID inizia con `S` → modulo **Security** — cartella grezzi: `APPUNTI GREZZI/Lab - Security/`
- ID inizia con cifra → modulo **SysAdmin** — cartella grezzi: `APPUNTI GREZZI/Lab - sysAdm/`
- ID vuoto o non riconosciuto → mostra i formati validi e fermati

---

**1. Individua il file grezzo**

Cerca in base al tipo:
- SysAdmin: `/home/lorenzo/UniCode/APPUNTI GREZZI/Lab - sysAdm/Appunti_modulo$ARGUMENTS.md`
- Security: `/home/lorenzo/UniCode/APPUNTI GREZZI/Lab - Security/Appunti_modulo$ARGUMENTS.md`
- Diritto: `/home/lorenzo/UniCode/APPUNTI GREZZI/Diritto/Appunti_modulo$ARGUMENTS.md`

Considera varianti di nome (minuscole, underscore, spazi). Se il file non esiste, comunicalo e fermati.

---

**2. Leggi il file grezzo integralmente**

Identifica e annota:
- Domande aperte (esplicite o tra parentesi tonde/quadre)
- Lacune rispetto ai concetti chiave del modulo nella master map
- **Per SysAdmin/Security**: bug o errori negli script scritti in autonomia (sintassi sbagliata, logica invertita, keyword mancanti)
- **Per Diritto**: imprecisioni nelle definizioni giuridiche, articoli citati in modo errato, concetti confusi
- Sezioni omesse — l'assenza non implica che siano state saltate, non marcarle come lacune senza prima verificare con Lorenzo

---

**3. Leggi la lezione di riferimento**

Cerca `/home/lorenzo/UniCode/claudeLezioni/lezione_modulo$ARGUMENTS_*.md` e leggila come struttura di riferimento.

---

**4. Leggi la master map**

Leggi `/home/lorenzo/UniCode/master_map_studio.md` per recuperare i concetti chiave e lo stato del modulo `$ARGUMENTS`.

---

**5. Crea il file di appunti definitivi**

Path: `/home/lorenzo/UniCode/claudeAppunti/appunti_modulo$ARGUMENTS_<nome_breve>.md`
`<nome_breve>` deve corrispondere a quello usato nella lezione corrispondente.

**Struttura comune a tutti i tipi:**
- Segui l'ordine della lezione come ossatura
- Per ogni domanda aperta: rispondi inline come blocco citazione `>` immediatamente dopo il concetto a cui si riferisce
- Per ogni sezione omessa: includila con nota `> ⚠️ Questa sezione non era presente negli appunti grezzi.`

**Solo per SysAdmin e Security — aggiungi:**
- Per ogni bug identificato: mostra il codice errato, l'analisi dell'errore e la versione corretta

**Solo per Diritto — aggiungi:**
- Per ogni imprecisione giuridica: mostra la formulazione di Lorenzo, la correzione e il riferimento normativo esatto
- Alla fine: sezione "Domande di autoverifica — Risposte" con le risposte alle domande della lezione, se Lorenzo le ha incluse negli appunti grezzi

---

**6. Aggiorna la master map**

Aggiorna `/home/lorenzo/UniCode/master_map_studio.md`:
- Stato del modulo `$ARGUMENTS`:
  - → ✅ solo se Lorenzo ha eseguito gli esercizi sulla VM (SysAdmin/Security) o ha risposto alle domande di autoverifica (Diritto) — verificarlo dagli appunti grezzi
  - → 🔄 altrimenti, con nota sullo stato interno
- Nell'ultima voce del log di sessione aggiungi: "Appunti modulo `$ARGUMENTS` elaborati → `appunti_modulo$ARGUMENTS_<nome_breve>.md`"
- Aggiorna il campo "Prossima sessione — da dove partire" se necessario

---

**7. Comunica il risultato**

Indica:
- Path del file creato
- Numero di domande risolte, bug/imprecisioni corrette, sezioni integrate
- Se il modulo è stato portato a ✅ o è rimasto 🔄 (con motivazione)
