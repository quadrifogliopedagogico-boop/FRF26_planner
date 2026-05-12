# Florence Roma Florence 2026 — Planner

Strumento di pianificazione non ufficiale per la **Randonneè Mondiale Florence Roma Florence 2026** organizzata da [Casellina Ciclismo](https://www.casellinaciclismo.org/corse/la-florence-roma-florence).

---

## Descrizione

Applicazione HTML standalone (file unico, zero dipendenze esterne a runtime) per pianificare le proprie giornate di percorrenza sulla FRF 2026.

- **1.210 km · 13.000 m D+** · Partenza 18 agosto 2026, Scandicci (FI)
- **11 controlli ufficiali** da tabella ARI/Casellina Ciclismo
- **Limite regolamentare: 90 ore** (standard ARI/ACP 1200 km brevet)
- Calcolo tempi su **2.253 segmenti GPX reali** (~500 m ciascuno), estratti dal tracciato ufficiale FRF 2026

---

## Funzionalità

### Modello fisico
Tre modalità di calcolo selezionabili:

| Modalità | Input | Note |
|---|---|---|
| **Velocità + VAM** | Vel. pianura, VAM salita | Stima diretta, adatto a chi conosce i propri valori storici |
| **Potenza media** | Watt medi, peso ciclista + bici | Modello Newton, differenziato salita/piano/discesa |
| **FTP + Peso** | FTP, intensità % salita/piano, peso | Modello più accurato per atleti con dati di allenamento |

Parametri comuni a tutti i modelli:
- **Fatica cumulativa** (%/giorno): decadimento delle prestazioni giorno per giorno
- Forchetta automatica **+5% / -10%** (ottimistico / nominale / conservativo)
- Velocità discesa fissa: 30 km/h
- Parametri aerodinamici: CdA 0.40 · Cr 0.005 · ρ 1.1 kg/m³ · η 0.95

### Pianificazione giorni
- **Preset 4 giorni** (consigliato): sosta ai dormitori di Foligno (T4) · Ciampino (T6) · Castiglione d/Lago (T8)
- **Preset 3 giorni** (veloce): sosta a Carsoli (T5) · Castiglione d/Lago (T8)
- Configurazione libera: ogni tappa può essere unita o separata con un clic
- Soste intermedie personalizzabili per ogni tappa (in minuti)

### Orari e ripartenze
- Ora di partenza configurabile (default 05:00)
- **Ora di ripresa** configurabile (default 05:00): la ripartenza automatica calcola la prossima occorrenza dell'orario impostato dopo l'arrivo, con minimo 2h di riposo garantito
- Override manuale campo per campo (formato HH:MM)

### Registro tempi reali
- Registrazione arrivi/partenze con timestamp corrente (tasto unico)
- Annulla ultimo inserimento
- Calcolo automatico del **fattore di performance reale** rispetto al piano
- Proiezione degli orari futuri corretta in base al ritmo effettivo

### Indicatori
- Budget 90h con barra di avanzamento e margine residuo
- Avviso sforamento se elapsed > 90h
- Bilanciamento carico giornaliero (coefficiente di variazione)
- Badge 🎒 Bag Drop (Foligno · Ciampino · Castiglione d/Lago, €15 cad.)
- Badge 🛌 Dormitorio disponibile
- Grafico a barre distribuzione carico per giorno

---

## Struttura tappe (tabella controlli ufficiale)

| N | Da | A | Km parz. | Km prog. | D+ parz. | Servizi |
|---|---|---|---:|---:|---:|---|
| 0 | — | **Scandicci (FI)** | — | 0 | — | Ristoro · Docce · Dormit. |
| 1 | Scandicci | **Poppi (AR)** | 80 | 80 | 1.500 m | Ristoro |
| 2 | Poppi | **Anghiari (AR)** | 50 | 130 | 650 m | Ristoro |
| 3 | Anghiari | **Gubbio (PG)** | 80 | 210 | 650 m | Ristoro · Docce |
| 4 | Gubbio | **Foligno (PG)** 🎒 | 85 | 295 | 1.000 m | Ristoro · Docce · Dormit. |
| 5 | Foligno | **Carsoli (AQ)** | 185 | 480 | 1.800 m | Ristoro · Docce · Dormit. |
| 6 | Carsoli | **Ciampino (Roma)** 🎒 | 120 | 600 | 1.400 m | Ristoro · Docce · Dormit. |
| 7 | Ciampino | **Lago di Vico (VT)** | 130 | 730 | 1.300 m | Ristoro |
| 8 | Lago di Vico | **Castiglione d/Lago (PG)** 🎒 | 150 | 880 | 2.100 m | Ristoro · Docce · Dormit. |
| 9 | Castiglione d/L. | **Figline-Incisa (FI)** | 110 | 990 | 500 m | Ristoro · Docce · Dormit. |
| 10 | Figline-Incisa | **Capannori (LU)** | 110 | 1.100 | 1.200 m | Ristoro · Docce |
| 11 | Capannori | **Scandicci (FI)** | 110 | 1.210 | 700 m | Ristoro · Docce · Dormit. |

---

## Utilizzo

1. Scaricare `frf_planner.html`
2. Aprire in qualsiasi browser moderno (Chrome, Firefox, Safari, Edge)
3. Nessuna installazione, nessuna connessione internet necessaria a runtime

> Il file è completamente autonomo: dati GPX, logica di calcolo e interfaccia sono embedded in un singolo file HTML di ~90 KB.

---

## Note tecniche

### GPX e segmenti
- Sorgente: tracciato ufficiale FRF 2026 (33.390 trackpoint)
- Smoothing elevazione: finestra mobile 15 punti per ridurre il rumore GPS
- D+ smoothed: ~10.020 m (vs 13.000 m dichiarati — delta tipico algoritmo vs profilo altimetrico ufficiale)
- Distanza GPX: 1.218,5 km (vs 1.210 km ufficiali — scala proporzionale applicata ai breakpoint)
- Segmenti estratti: 2.253 chunk da ~500 m con pendenza media

### Modello fisico
- Salita (pendenza > 1,5%): potenza / (massa × g × (pendenza + Cr)) → velocità
- Piano: equazione cubica Newton con termine aerodinamico (CdA) e resistenza al rotolamento
- Discesa (pendenza < -2%): velocità fissa 30 km/h (conservativo per randonnée)
- Fatica: moltiplicatore `max(0.5, 1 - decay × (giorno - 1))` applicato a potenza/velocità

### Disclaimer
Strumento non ufficiale a uso personale. I dati ufficiali dell'evento (km, D+, orari controlli, servizi) sono pubblicati da [Casellina Ciclismo](https://www.casellinaciclismo.org/corse/la-florence-roma-florence). In caso di discrepanze, i dati ufficiali prevalgono.

---

## Licenza

MIT — uso libero con attribuzione.

---

*Buona fortuna a tutti i partecipanti alla FRF 2026.*
