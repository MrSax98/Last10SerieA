# Interactive Football Data Analysis and Q-Learning Prediction Toolkit ⚽📊🤖

Questo progetto combina due componenti principali per lavorare con dati storici di partite di calcio (originariamente Serie A, ma adattabile):

1.  **Parte 1: Interactive Data Analysis Notebook (`seriea_analysis.ipynb`)**
    * Un ambiente Jupyter Notebook per l'esplorazione visiva dei dati storici.
    * Utilizza `pandas` per la manipolazione, `plotly` per grafici interattivi e `ipywidgets` per un'interfaccia utente nel notebook.
    * Permette di identificare trend, pattern e distribuzioni nei dati passati.

2.  **Parte 2: Q-Learning Prediction Script (`q_football_predictor.py`)**
    * Uno script Python che implementa un agente di Reinforcement Learning (Q-Learning).
    * L'agente viene addestrato sui dati storici per imparare a prevedere i risultati delle partite future (Vittoria Casa '1', Pareggio 'X', Vittoria Trasferta '2').
    * Utilizza feature ingegnerizzate (forma recente, H2H) e discretizzazione adattiva basata sui dati.

Lo scopo complessivo è fornire strumenti sia per capire i dati storici sia per tentare di prevedere gli esiti futuri basandosi su un approccio di apprendimento automatico.

## Funzionalità Chiave ✨

* **Caricamento Dati Robusto:** Legge e combina dati da più file stagionali (CSV/XLSX), gestendo encoding, estraendo stagioni dai nomi file e pulendo i dati.
* **Preprocessing Dati:** Calcola metriche derivate comuni (Total Goals, Goal Difference, Points H/A, Score String).
* **Analisi Esplorativa Interattiva (Parte 1):**
    * Ampia gamma di grafici Plotly interattivi (andamenti temporali, distribuzioni, analisi per squadra, confronti H/A).
    * Interfaccia `ipywidgets` nel notebook per selezionare grafici e squadre.
    * Layout grafici ottimizzato (tema scuro).
* **Predizione con Q-Learning (Parte 2):**
    * Feature Engineering specifico per la predizione (rolling stats, H2H calcolati *prima* della data della partita).
    * **Discretizzazione Adattiva:** Bin delle feature calcolati tramite quantili sui dati storici per una migliore rappresentazione dello stato.
    * Addestramento Q-Learning con epsilon-greedy e decadimento dell'esplorazione.
    * Predizione per nuove partite con output dei Q-values e stima delle probabilità (via Softmax).
* **Output Multipli (Parte 2):** Log dettagliati, tabella testuale dei pronostici, tabella HTML (in ambienti supportati) con barre di confidenza e file CSV esportabile.
* **Configurabilità:** Parametri chiave modificabili (percorso dati, iperparametri RL, parametri feature, numero episodi, output file, livello log) sia nel codice che via linea di comando (per la Parte 2).
* **Codice Commentato:** Type hints e commenti per migliorare leggibilità e manutenibilità.

## Requisiti 📋

* Python 3.7+
* Librerie Python (installabili tramite `pip` - vedi `requirements.txt`):
    * `pandas`
    * `numpy`
    * `plotly` (per Parte 1)
    * `ipywidgets` (per Parte 1)
    * `tabulate` (per Parte 2)
    * `tqdm` (per Parte 2)
    * `ipython` (necessaria per la visualizzazione HTML in Parte 1 e Parte 2 se eseguita in ambienti Jupyter)
    * `notebook` o `jupyterlab` (necessari per eseguire la Parte 1, l'analisi nel notebook)

## Installazione ⚙️

1.  **Clona il Repository:**
    ```bash
    git clone [https://github.com/tuo-username/tuo-repository-name.git](https://github.com/tuo-username/tuo-repository-name.git)
    cd tuo-repository-name
    ```
    *(Sostituisci `tuo-username/tuo-repository-name` con il tuo URL)*

2.  **(Consigliato) Crea un Ambiente Virtuale:**
    ```bash
    python -m venv venv
    # Su Linux/macOS:
    source venv/bin/activate
    # Su Windows:
    .\venv\Scripts\activate
    ```

3.  **Installa le Dipendenze:**
    Crea un file `requirements.txt` con il seguente contenuto:
    ```txt
    pandas
    numpy
    plotly
    ipywidgets
    tabulate
    tqdm
    ipython
    notebook # o jupyterlab
    ```
    Poi esegui:
    ```bash
    pip install -r requirements.txt
    ```

## Preparazione dei Dati 📊

Entrambe le parti del progetto utilizzano la **stessa fonte dati**.

1.  **Formato File:** File `.csv` o `.xlsx`.
2.  **Directory:** Raccogli i file delle stagioni in una directory dedicata.
3.  **Colonne Necessarie:** Ogni file deve contenere almeno le seguenti informazioni (i nomi colonna sono flessibili):
    * `Date`: Data della partita.
    * `HomeTeam`, `AwayTeam`: Nomi delle squadre.
    * `FTHG`, `FTAG`: Gol a fine partita Casa/Trasferta.
    * `FTR`: Risultato ('H', 'D', 'A').
    * `Season` (Opzionale): Identificativo stagione.
4.  **Consistenza Nomi Squadre:** I nomi devono essere consistenti tra i file e corrispondere a quelli usati per le predizioni (Parte 2).
5.  **Aggiorna Percorsi Codice:**
    * **Per Parte 1 (Notebook):** Modifica la variabile `directory` nella cella "Esecuzione Principale" del file `.ipynb`.
    * **Per Parte 2 (Script):** Modifica il valore di default per `data_directory` nel dizionario `CONFIG` all'inizio del file `.py`, oppure usa l'argomento `--data-dir` da linea di comando.

## Utilizzo 🚀

### Parte 1: Data Analysis Notebook (`seriea_analysis.ipynb`)

1.  **Avvia Jupyter:** Esegui `jupyter notebook` o `jupyter lab` nel terminale dalla directory del progetto.
2.  **Apri il Notebook:** Naviga e apri il file `.ipynb`.
3.  **Modifica Percorso Dati:** Verifica/correggi la variabile `directory` nel codice.
4.  **Esegui Celle:** Esegui tutte le celle ("Run All").
5.  **Interagisci:** Usa i menu a tendina alla fine del notebook per selezionare e visualizzare i diversi grafici Plotly interattivi.

### Parte 2: Q-Learning Predictor (`q_football_predictor.py`)

Esegui lo script dalla linea di comando:

* **Esecuzione base (usa default per dati e episodi):**
    ```bash
    python q_football_predictor.py
    ```
* **Specifica directory dati e numero episodi:**
    ```bash
    python q_football_predictor.py --data-dir /percorso/ai/tuoi/dati/ --episodes 250
    ```
* **Cambia file output CSV e livello log:**
    ```bash
    python q_football_predictor.py --output-csv miei_pronostici.csv --log-level DEBUG
    ```
* Vedi l'output `--help` per tutte le opzioni:
    ```bash
    python q_football_predictor.py --help
    ```

## Output 📄

* **Parte 1 (Notebook):**
    * Visualizzazione diretta dei grafici Plotly interattivi all'interno dell'ambiente Jupyter.
    * Log di caricamento e preprocessing stampati nell'output delle celle.
* **Parte 2 (Script):**
    * **Console:** Log dettagliati su caricamento, training, predizioni; tabella testuale finale dei pronostici.
    * **Tabella HTML:** Se eseguito in ambiente Jupyter/IPython, mostra una tabella HTML formattata dei pronostici con barre di confidenza.
    * **File CSV:** Salva un file (default: `schedina_predictions.csv`) con i dettagli completi delle previsioni (pronostico, confidenza, probabilità stimate, Q-values).

## Configurazione Avanzata ⚙️

* **Parte 1 (Notebook):**
    * Il percorso dati va modificato nella variabile `directory`.
    * I parametri specifici dei grafici (es. `top_n`, `window`) possono essere cambiati nel dizionario `PLOT_FUNCTIONS`.
* **Parte 2 (Script):**
    * Gli iperparametri Q-Learning, i parametri delle feature (rolling window, min H2H), e il numero di bin sono nel dizionario `CONFIG` all'inizio del file `.py`.
    * Il percorso dati, numero episodi, nome file output e livello log possono essere sovrascritti da linea di comando (CLI).

## Metodologia (Panoramica) 🧠

* **Analisi Esplorativa (Parte 1):** Utilizza visualizzazioni dirette (linee, barre, torte, scatter, istogrammi) sui dati storici aggregati per stagione o per squadra per identificare tendenze macroscopiche, distribuzioni di risultati/gol e performance relative delle squadre.
* **Q-Learning (Parte 2):**
    1.  **Stato:** Rappresenta una partita futura tramite un vettore di feature discretizzate (forma recente H/A, H2H H/A). La discretizzazione usa quantili per adattarsi ai dati.
    2.  **Azioni:** Le possibili predizioni ('H', 'D', 'A').
    3.  **Ricompensa:** Semplice (+1 per predizione corretta, -1 per errata).
    4.  **Apprendimento:** L'agente esplora i dati storici, associa stati ad azioni, riceve ricompense e aggiorna la Q-Table (`Q(stato, azione)`) per stimare il valore atteso di una predizione in un dato stato. Usa epsilon-greedy per bilanciare esplorazione e sfruttamento.
    5.  **Predizione:** Per una nuova partita, calcola lo stato, trova i Q-values associati, e sceglie l'azione con il Q-value massimo. Softmax viene usato per derivare probabilità/confidenza.

## Possibili Miglioramenti Futuri 🔮

* **Dati & Feature:**
    * Integrare dati più ricchi (quote scommesse, statistiche avanzate xG/xGA, dati giocatori, meteo, etc.).
    * Ingegnerizzare feature più complesse (es. Elo ratings, indicatori di momentum).
* **Modellazione (Parte 2):**
    * Ottimizzare iperparametri (Q-Learning, numero bin, finestre rolling) con tecniche automatiche.
    * Sperimentare con reward shaping più sofisticato.
    * Esplorare altri algoritmi RL (DQN, Policy Gradients) se lo spazio stati diventa ingestibile.
    * Implementare valutazione con cross-validation time-series.
* **Analisi & Usabilità (Parte 1 & 2):**
    * Aggiungere più tipi di grafici e controlli interattivi nel notebook.
    * Aggiungere analisi statistiche formali nel notebook.
    * Creare un'interfaccia web unificata (es. con Dash o Streamlit) per entrambe le funzionalità.
    * Aggiungere opzioni di esportazione per grafici e risultati formattati.
    * Migliorare la gestione degli errori e dei casi limite.

## Licenza 📄
PROGETTO SCRITTO CON L'AIUTO DI GEMINI (GOOGLE)
---
