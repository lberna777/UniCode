---
description: "Genera il piano di studio per oggi basandosi sullo stato dei moduli, le scadenze degli esami e la fase corrente del piano settimanale."
---

Leggi entrambi i file:
- `/home/lorenzo/UniCode/master_map_studio.md`
- `/home/lorenzo/UniCode/ESAMI SCELTI.md`

La data di oggi è disponibile nel contesto di sessione. Calcola i giorni mancanti a ciascun esame:
- Diritto: 16/06/2026
- SysAdmin: 22/06/2026
- Security: 17/07/2026

Determina la fase corrente del piano settimanale (da ESAMI SCELTI.md) in base alla data odierna, e identifica il prossimo modulo da fare per ciascuna materia dalla master map.

Produci esclusivamente questo output:

---

**Piano di oggi — [DATA]**

**Scadenze**: Diritto tra X gg (16/06) · SysAdmin tra X gg (22/06) · Security tra X gg (17/07)

**Fase corrente**: [descrizione della fase dal piano settimanale, es. "29/04–11/05 · 2h SysAdmin + 2h Diritto + 2h Security"]

---

**Blocco 1 — [Materia] · ~Xh**
[Modulo ID] — [azione concreta: cosa fare, quale esercizio, quale file leggere o creare]

**Blocco 2 — [Materia] · ~Xh**
[Modulo ID] — [azione concreta]

**Blocco 3 — [Materia] · ~Xh**
[Modulo ID] — [azione concreta]

---

**Segnali di rischio** (solo se presenti):
- [eventuali moduli in ritardo rispetto alla fase, materiale mancante, scadenze vicine]

---

Regole:
- I tre blocchi seguono l'allocazione oraria della fase corrente (es. 2+2+2 nella fase 1).
- La materia più a rischio rispetto alla scadenza va nel Blocco 1.
- Se un modulo ha la lezione già pronta ma la pratica VM non fatta, segnalarlo esplicitamente nell'azione.
- Se per Security non è ancora disponibile il PDF necessario, il blocco Security diventa "Richiedere PDF [nome modulo] da Virtuale prima di procedere".
- Non aggiungere testo libero oltre a questi elementi.
