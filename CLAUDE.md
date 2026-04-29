# CLAUDE.md — Studio Universitario Lorenzo

Questo file definisce il comportamento di Claude in ogni sessione. È vincolante e ha precedenza su qualsiasi comportamento di default.

---

## AZIONE OBBLIGATORIA A INIZIO SESSIONE

**Prima di qualsiasi altra cosa**, leggere:

```
/home/lorenzo/UniCode/master_map_studio.md
```

Contiene lo stato aggiornato di ogni modulo, il log delle sessioni, e il punto esatto da cui riprendere. Senza averlo letto non è possibile condurre correttamente una sessione.

---

## Scadenze Esami (Sessione Giugno/Luglio 2026)

| Esame | Data | Giorni da 29/04 |
|---|---|---|
| Diritto dell'Informatica T | **16/06/2026** ore 09:30 | 48 |
| Lab Amministrazione di Sistemi T | **22/06/2026** ore 09:00 | 54 |
| Lab Sicurezza Informatica T | **17/07/2026** ore 14:00 | 79 |

**Piano orario giornaliero** (5-6h/gg, miscelato):

| Fase | Periodo | SysAdmin | Diritto | Security |
|---|---|---|---|---|
| 1 | 29/04–11/05 | 2h | 2h | 2h |
| 2 | 12/05–25/05 | 2h | 2h | 2h |
| 3 | 26/05–15/06 | 1.5h | 1.5h | 3h |
| 4 | 16/06–22/06 | 3h | — | 3h |
| 5 | 23/06–17/07 | — | — | 5-6h |

Dettaglio completo e stime ore per modulo: `/home/lorenzo/UniCode/ESAMI SCELTI.md`

Per generare il piano del giorno: `/piano`

---

## Contesto del Progetto

Lorenzo è uno studente universitario (piattaforma: **Virtuale**, UniBo) che sta recuperando un divario rispetto al programma con un approccio di studio attivo.

**Livello reale accertato** — aggiornato al 2026-04-22:
- **Lab Amministrazione di Sistemi T**: in progressione attiva. Blocco 0 ✅, Blocco 1 ✅, Blocco 2 🔄 (2C creata, lab non eseguito). Prossimo: **3A — systemd**.
- **Lab Sicurezza Informatica T**: concetti vaghissimi. Non ancora affrontato in lab pratico.
- **Diritto dell'Informatica T**: quasi completo. Manca Privacy/GDPR.

**Appunti GitHub** (utente `lberna777`): esistono ma non devono mai essere usati come baseline di conoscenza assunta. Trattare ogni argomento come nuovo fino a evidenza contraria dall'esercizio pratico nella sessione corrente.

---

## Regole di Conduzione delle Sessioni

### 1. Fonte primaria: Virtuale
Tutto il materiale deve essere ancorato ai PDF presenti in `SLIDE TEORIA/` e `SLIDE LAB/`. Non sostituire con risorse esterne salvo richiesta esplicita.

### 2. Studio attivo — mai solo lettura
Per i corsi laboratoriali: studiare un argomento significa eseguire i comandi sulla VM. Non segnare un modulo come completato se Lorenzo ha solo letto senza eseguire in prima persona.

Schema per ogni modulo:
1. Verifica se il PDF necessario è già presente in `SLIDE TEORIA/` o `SLIDE LAB/`
2. Se non è presente, **comunicare esplicitamente a Lorenzo il titolo esatto del PDF necessario** e attendere che lo carichi prima di procedere
3. Leggi il PDF
4. Crea la lezione in `claudeLezioni/lezione_moduloXX_nome.md`
5. Lorenzo esegue i comandi sulla VM e scrive gli appunti grezzi
6. Claude ripulisce gli appunti in `claudeAppunti/appunti_moduloXX_nome.md`
7. Aggiorna `master_map_studio.md`

### Gestione PDF per nuove materie
Quando si inizia una materia non ancora presente nel progetto (es. Sicurezza Informatica, Diritto), **chiedere subito a Lorenzo i titoli di tutti i PDF disponibili** per quella materia (teoria e lab), così da sapere cosa richiedere sessione per sessione. Non procedere a creare contenuto senza avere il PDF corrispondente.

### 3. Aggiornare la master map a fine sessione
Al termine di ogni sessione aggiornare il log in `master_map_studio.md` con:
- cosa è stato coperto
- stato dei moduli toccati (⬜ → 🔄 → ✅)
- eventuali lacune da recuperare
- punto esatto da cui iniziare la sessione successiva

---

## Workflow Appunti Grezzi → Appunti Puliti

Il segnale di innesco è "ho aggiunto gli appunti di questo modulo" o equivalente.

**Procedura obbligatoria:**

1. **Leggere il file grezzo integralmente** — identificare:
   - domande aperte (esplicite o tra parentesi)
   - lacune rispetto alla lezione del modulo
   - bug o errori negli script scritti in autonomia
   - note di stile

2. **Produrre il file di appunti puliti** in `claudeAppunti/appunti_moduloXX_argomento.md` che:
   - mantiene la struttura e il filo della lezione corrispondente
   - risponde a ogni domanda aperta inline, come blocco citazione `>` immediatamente dopo il concetto a cui si riferisce
   - include le sezioni mancanti con nota esplicita
   - corregge i bug con analisi degli errori

3. **Aggiornare `master_map_studio.md`** — log di sessione, stato del modulo, punto di ripresa.

**Nota sul comportamento di Lorenzo**: omette intenzionalmente dagli appunti grezzi le sezioni già consolidate. L'assenza di un argomento non implica che sia stato saltato. Includere comunque la sezione negli appunti puliti se è prerequisito, con una nota che segnala l'omissione. Non aggiornare la master map come lacuna senza prima verificare con Lorenzo.

---

## VM di Lavoro

### VM SysAdmin — Vagrant + Debian 12
```bash
cd ~/sysAdmin-lab
vagrant up --provider=virtualbox
vagrant ssh
```
Per spegnerla: `vagrant halt`

### VM Security — Kali Linux / Parrot OS
- Gestita tramite VirtualBox
- Scheda host-only: `vboxnet0`
- Usare Snapshot prima di ogni esercizio di compromissione

---

## Struttura del Progetto

```
/home/lorenzo/UniCode/
├── CLAUDE.md                    ← questo file
├── master_map_studio.md         ← filo conduttore, aggiornato ogni sessione
├── glossario.md                 ← termini tecnici, cresce a ogni sessione
├── troubleshooting_vm.md        ← soluzioni ai problemi ricorrenti sulla VM
├── template_appunti_grezzi.md   ← template che Lorenzo usa dopo la pratica
│
├── claudeLezioni/               ← lezioni create da Claude (una per modulo)
│   ├── LEZIONI SYSADM/
│   └── LEZIONI DIRITTO/
├── claudeAppunti/               ← appunti definitivi (grezzi → ripuliti)
├── APPUNTI GREZZI/              ← appunti scritti da Lorenzo dopo la pratica
│   ├── Lab - sysAdm/
│   ├── Lab - Security/
│   └── Diritto/
├── SLIDE TEORIA/                ← PDF teoria da Virtuale
│   ├── SYSADM/
│   ├── DIRITTO INFORMATICO/     ← include sottocartella NORMATIVE/
│   └── SICINF/                  ← solo pagina corso; PDF da scaricare sessione per sessione
├── SLIDE LAB/                   ← PDF lab da Virtuale
│   └── SYSADM/
└── SIMULAZIONI ESAMI/           ← prove d'esame passate
    └── SYSADM/
```

Convenzione di naming:
- `lezione_moduloXX_argomento.md` — materiale didattico, generato da Claude prima della sessione pratica
- `appunti_moduloXX_argomento.md` — appunti definitivi post-sessione

---

## Lingua e Stile

Usare linguaggio accademico universitario. Non dare nulla per scontato. Risposte concise e dirette — non ripetere quello che Lorenzo ha già detto. Se un contenuto può stare in un file, metterlo in un file anziché in chat.
