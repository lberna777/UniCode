---
description: "Avvia o cambia il focus della sessione di studio. Uso: /sessione [materia]  (es. /sessione, /sessione diritto, /sessione sysadmin, /sessione security)"
argument-hint: "materia opzionale — sysadmin | diritto | security | (vuoto = panoramica generale)"
---

Il parametro passato è: "$ARGUMENTS"

**Rileva il focus richiesto:**
- `$ARGUMENTS` vuoto → modalità **generale** (tutte le materie)
- contiene "dir" → focus **Diritto**
- contiene "sys" → focus **SysAdmin**
- contiene "sec" → focus **Security**
- altro → avvisa che il parametro non è riconosciuto, mostra le opzioni valide (`sysadmin`, `diritto`, `security`) e fermati

**Rileva se la sessione è già in corso:**
Controlla se la master map è già stata letta in questa conversazione. Se sì, non rilегgerla — usa i dati già in contesto e segnala a Lorenzo che stai cambiando focus senza ricaricare tutto.

---

**1. Leggi la master map** *(solo se non già letta in questa conversazione)*
Leggi `/home/lorenzo/UniCode/master_map_studio.md` integralmente.

---

**2. Mostra lo stato dei moduli**

*Focus generale* — tabella raggruppata per corso:

```
## SysAdmin — Lab Amministrazione di Sistemi T
| Modulo | Nome | Stato |
|--------|------|-------|
| ...    | ...  | ✅/🔄/⬜ |

## Security — Lab Sicurezza Informatica T
| Modulo | Nome | Stato |

## Diritto — Diritto dell'Informatica T
| Modulo | Nome | Stato |
```

*Focus specifico* — mostra solo i moduli di quella materia, con più dettaglio: se un modulo è 🔄, elenca lo stato interno degli esercizi (Es. 1 ✅, Es. 2 🔄, ecc.) se presente nella master map.

---

**3. Identifica il punto di ripresa**

*Focus generale*: mostra il punto di ripresa di ogni materia con moduli attivi (🔄), una riga per materia.

*Focus specifico*: mostra solo il punto di ripresa di quella materia, in evidenza.

---

**4. Proponi il piano per questa sessione**

Sulla base del focus e del punto di ripresa indica:
- Il modulo da affrontare (ID, nome)
- L'obiettivo concreto della sessione

*Per SysAdmin e Security*:
- Sequenza di esercizi/comandi da eseguire sulla VM
- Se il modulo è nuovo: ricorda di eseguire `/lezione <ID>` prima di avviare la VM

*Per Diritto*:
- Concetti da consolidare
- Se il modulo è nuovo: ricorda di eseguire `/lezione <ID>` e rispondere alle domande di autoverifica
- Se il modulo è in corso: indica da quale concetto riprendere

---

**5. Verifica PDF** *(solo per il modulo da affrontare oggi)*

- SysAdmin: `/home/lorenzo/UniCode/SLIDE TEORIA/SysAdmin/` e `SLIDE LAB/`
- Security: stesse cartelle SysAdmin + eventuale materiale Security
- Diritto: `/home/lorenzo/UniCode/SLIDE TEORIA/DIRITTO INFORMATICO/`

Se il PDF del modulo manca, fermati e comunicane il titolo esatto prima di procedere.

---

**6. Piano del giorno** *(sempre — sia focus generale che specifico)*

Leggi `/home/lorenzo/UniCode/ESAMI SCELTI.md`. Calcola i giorni mancanti a ciascun esame dalla data odierna (disponibile nel contesto):
- Diritto: 16/06/2026
- SysAdmin: 22/06/2026
- Security: 17/07/2026

Determina la fase corrente del piano settimanale in base alla data odierna e mostra:

```
**Piano del giorno — [DATA]**
Scadenze: Diritto tra X gg · SysAdmin tra X gg · Security tra X gg
Fase: [es. "29/04–11/05 · 2h SysAdmin + 2h Diritto + 2h Security"]

Blocco 1 — [Materia] · ~Xh
[Modulo ID] — [azione concreta]

Blocco 2 — [Materia] · ~Xh
[Modulo ID] — [azione concreta]

Blocco 3 — [Materia] · ~Xh
[Modulo ID] — [azione concreta]
```

In modalità *focus specifico*: il blocco della materia richiesta è Blocco 1; gli altri due blocchi mostrano comunque cosa fare nelle altre materie, perché lo studio è miscelato.

Se per Security non è ancora disponibile il PDF necessario, il blocco Security diventa: "Richiedere PDF [nome modulo] da Virtuale prima di procedere".
