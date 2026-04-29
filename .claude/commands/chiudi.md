---
description: "Chiude la sessione di studio. Aggiorna log, stati dei moduli e punto di ripresa nella master map."
---

Esegui i seguenti passi in ordine.

---

**1. Leggi la master map** *(solo se non già letta in questa conversazione)*
Leggi `/home/lorenzo/UniCode/master_map_studio.md` per recuperare: numero di sessione corrente, data, stato dei moduli, punto di ripresa pianificato.

---

**2. Raccogli le informazioni dalla sessione**

Chiedi a Lorenzo in un'unica domanda compatta — adattando le domande alla materia trattata:

*Per sessioni SysAdmin o Security (lab con VM):*
- Quali moduli/argomenti sono stati affrontati
- Quali esercizi sulla VM sono stati completati, quali interrotti, quali saltati
- Domande rimaste aperte o blocchi non risolti
- Problemi tecnici nuovi sulla VM

*Per sessioni Diritto (teoria):*
- Quali moduli/argomenti sono stati affrontati
- Se le domande di autoverifica sono state completate
- Concetti rimasti poco chiari o da approfondire

*Se la sessione ha toccato più materie*: chiedi per ciascuna.

Attendi la risposta prima di procedere.

---

**3. Aggiorna la master map**

Modifica `/home/lorenzo/UniCode/master_map_studio.md`:

- **Intestazione**: incrementa il numero di sessione di 1, aggiorna la data "Aggiornato".
- **Stato moduli**: aggiorna ⬜/🔄/✅ per ogni modulo toccato.
  - SysAdmin/Security → ✅ solo se Lorenzo ha eseguito gli esercizi sulla VM in prima persona
  - Diritto → ✅ solo se ha letto la lezione, risposto alle domande di autoverifica e scritto appunti grezzi
- **Log di sessione**: aggiungi una nuova voce in cima alla sezione (ordine cronologico inverso):

```
### Sessione N — YYYY-MM-DD (completata)
**Focus**: <materia/e — modulo/i>
**Coperto in sessione**:
- ...
**Non coperto / da riprendere**:
- ...
**Prossima sessione — da dove partire**:
→ ...
```

- **Avanzamento**: aggiorna le barre di avanzamento per corso (SysAdmin, Security, Diritto).

---

**4. Aggiorna il glossario** *(se necessario)*

Se sono emersi termini tecnici o giuridici nuovi non ancora in `/home/lorenzo/UniCode/glossario.md`, aggiungili con definizione concisa in ordine alfabetico.

---

**5. Aggiorna il troubleshooting** *(solo per sessioni lab con VM)*

Se sono stati risolti problemi tecnici nuovi, aggiungili a `/home/lorenzo/UniCode/troubleshooting_vm.md` con: sintomo, causa, soluzione.
Per sessioni Diritto, salta questo passo.

---

**6. Conferma finale**

Mostra a Lorenzo:
- Numero della sessione chiusa e materie/moduli aggiornati
- Punto esatto da cui partirà la prossima sessione per ogni materia attiva
