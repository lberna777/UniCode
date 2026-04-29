---
description: "Mostra il riepilogo rapido dello stato di tutti i moduli per corso e il prossimo step prioritario per ciascuno."
---

Leggi `/home/lorenzo/UniCode/master_map_studio.md` e produci esclusivamente:

---

**1. Tabelle di stato per corso**

Una tabella separata per ciascun corso, nell'ordine: SysAdmin → Security → Diritto.

Formato:
```
### SysAdmin — Lab Amministrazione di Sistemi T
| Modulo | Nome | Stato |
|--------|------|-------|

### Security — Lab Sicurezza Informatica T
| Modulo | Nome | Stato |

### Diritto — Diritto dell'Informatica T
| Modulo | Nome | Stato |
```

Usa ✅ completato, 🔄 in corso, ⬜ da fare. Per i moduli 🔄 aggiungi tra parentesi lo stato interno se presente (es. "Es. 1-2 ✅, Es. 3-7 ⬜").

---

**2. Avanzamento per corso**

Tre barre separate (non una per blocco), nel formato:

```
SysAdmin  ████████░░ 80%   (N/M moduli completati)
Security  ░░░░░░░░░░  0%   (0/12 moduli completati)
Diritto   ░░░░░░░░░░  0%   (0/8 moduli completati)
```

Calcola la percentuale sul totale dei moduli di ciascun corso.

---

**3. Prossimo step per corso**

Una riga per ogni corso con moduli non completati:

```
SysAdmin  → [ID modulo] — [azione concreta immediata]
Security  → [ID modulo] — [azione concreta immediata]
Diritto   → [ID modulo] — [azione concreta immediata]
```

---

Non aggiungere spiegazioni, commenti o testo libero oltre a questi tre elementi.
