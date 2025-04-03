# Analisi Dati Partite Calcio: Caricamento e Preprocessing

## Descrizione

Questa è la **prima parte** di uno script Python più ampio (`BasilicataIdrica.py` nel tuo esempio, ma probabilmente vorrai rinominarlo in base al contenuto effettivo, es. `analisi_partite.py`) progettato per analizzare e visualizzare dati storici di partite di calcio.

Questa sezione specifica dello script si concentra sulle operazioni iniziali e fondamentali di:

1.  **Caricamento Dati:** Trovare e leggere molteplici file CSV da una directory specificata, ognuno rappresentante una diversa stagione calcistica.
2.  **Pulizia Iniziale:** Gestire errori comuni durante il caricamento (codifiche, file vuoti, errori di parsing), controllare la presenza di colonne essenziali, convertire i dati numerici (gol) e le date (se presenti) nei formati corretti.
3.  **Combinazione e Ordinamento:** Unire i dati di tutte le stagioni in un unico DataFrame Pandas e ordinarli cronologicamente per stagione.
4.  **Preprocessing e Feature Engineering:** Calcolare nuove colonne utili per analisi successive (es. gol totali, differenza reti, punti assegnati, punteggio testuale) e pulire dati come i nomi delle squadre.

L'output di questa prima parte è un **DataFrame Pandas pulito, unificato e arricchito**, pronto per essere utilizzato dalle funzioni di visualizzazione definite nella seconda parte dello script (non descritta in questo README specifico).

## Funzionalità Principali (di questa sezione)

* **Caricamento Multi-File Robusto (`load_data`):**
    * Cerca file CSV che iniziano con `season` (ignorando maiuscole/minuscole) nella directory specificata.
    * Gestisce automaticamente diverse **codifiche** comuni (ISO-8859-1, UTF-8).
    * Estrae il nome della **stagione** dal nome del file (supporta formati come `season-19-20.csv`, `season_0102.csv`, `season99.csv`).
    * **Valida** la presenza delle colonne fondamentali (`HomeTeam`, `AwayTeam`, `FTHG`, `FTAG`, `FTR`).
    * Converte `FTHG`, `FTAG` in **numerico**, gestendo errori e valori mancanti (impostati a 0).
    * Tenta la conversione della colonna `Date` (se esiste) in **datetime**, provando il formato europeo (`dayfirst=True`) e aggiunge una colonna `Year`.
    * Stampa messaggi informativi e di **errore** dettagliati sulla console durante il caricamento.
    * **Combina** i dati validi di tutte le stagioni.
    * **Ordina** le stagioni in modo cronologico (basato sull'anno di inizio).
    * Rimuove righe con **squadre mancanti** e, se le date sono valide, rimuove **partite duplicate** (stessa data, stesse squadre).
* **Preprocessing e Calcolo Colonne Derivate (`preprocess_data`):**
    * Assicura che i gol siano interi.
    * Calcola: `TotalGoals` (FTHG + FTAG), `GoalDifference` (FTHG - FTAG), `Score` (formato "X-Y").
    * Calcola i punti assegnati per partita: `PointsH`, `PointsA` (3 per vittoria, 1 per pareggio).
    * Pulisce eventuali spazi bianchi iniziali/finali dai nomi delle squadre (`HomeTeam`, `AwayTeam`).

## Fonti dei Dati Attese

* Lo script è progettato per leggere file **CSV**.
* **Posizione:** I file CSV devono trovarsi tutti nella stessa **cartella**, il cui percorso deve essere specificato dall'utente all'interno dello script.
* **Nomi File:** I nomi dei file devono iniziare con `season` (es. `season-23-24.csv`, `season_2223.csv`) e finire con `.csv`.
* **Colonne Minime Richieste:** Ogni file CSV *deve* contenere almeno le colonne: `HomeTeam`, `AwayTeam`, `FTHG`, `FTAG`, `FTR`.
* **Colonna Data (Consigliata):** La presenza di una colonna `Date` correttamente formattata è necessaria per alcune analisi temporali (come la media mobile).

## Librerie Utilizzate (per questa sezione)

Per eseguire questa parte dello script, sono necessarie le seguenti librerie Python:

* **Pandas:** Fondamentale per la manipolazione e l'analisi dei dati tabulari (DataFrame).
* **NumPy:** Utilizzata per operazioni numeriche (spesso chiamata internamente da Pandas).
* **os:** Per interagire con il sistema operativo (leggere file e directory).
* **traceback:** Per fornire messaggi di errore più dettagliati in caso di problemi imprevisti.
* **typing:** Per le annotazioni di tipo (migliora leggibilità e manutenibilità).

Puoi installare le librerie principali con pip:
```bash
pip install pandas numpy
