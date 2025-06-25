# StatisticsOfExtremeEvents

# 📊 Considerazioni Quantitative dai Risultati del Progetto Finale 2

Il progetto finale 2 applica i concetti dell'Extreme Value Theory (EVT) ai dati di precipitazione giornaliera per la regione Lazio, dal 1951 al 2022 (per un totale di 26.296 giorni).

---

## 1. Analisi Block Maxima (BM)

- Sono stati estratti i **massimi annuali** utilizzando un blocco di dimensione 365 giorni. Il dataset ha prodotto **72 massimi annuali**.
- Questa è una quantità ragionevole di punti per adattare un modello **GEV/Gumbel**.

### 📈 Stime dei valori di ritorno (IC 95%):
- 2 anni: **98.245 mm** (IC: 92.884 - 104.560 mm)
- 5 anni: **119.497 mm** (IC: 110.575 - 129.219 mm)
- 10 anni: **133.568 mm** (IC: 121.912 - 145.934 mm)

- Il valore di ritorno stimato per 1 anno è **-inf** con intervallo di confidenza **NaN**: ciò può indicare un’inadeguatezza del modello Gumbel per periodi così brevi o un problema numerico.
- L’evento massimo osservato nel dataset (206.9 mm il 22-09-2019) ha un periodo di ritorno empirico di circa **73 anni**, ben superiore ai 10 anni stimati dal modello → evento estremamente raro.

---

## 2. Analisi Peaks Over Threshold (POT)

- È stato scelto un parametro di **declustering** `r = 30D`.
- La scelta della soglia è stata studiata. Inizialmente è stata fissata a **91 mm**, individuando **57 eventi estremi**.

### 📈 Parametri MLE del modello GPD (soglia 91 mm):
- `c = -0.253`, `loc = 91.160`, `scale = 31.918`
- Il parametro di forma negativo (`c < 0`) indica una distribuzione di tipo **Weibull** per gli eccessi.

### 📈 Valori di ritorno (IC 95%):
- 2 anni: **105.008 mm** (IC: 70.913 - 131.026 mm)
- 5 anni: **128.248 mm** (IC: 116.453 - 175.031 mm)
- 10 anni: **142.577 mm** (IC: 129.937 - 191.569 mm)

- Anche qui, il valore stimato per 1 anno è **-inf**.

---

## 🔍 Ricerca della soglia ottimale

- È stata condotta una ricerca per valori di soglia da **80 a 110 mm**, valutando la bontà di adattamento tramite gli **R²** dei plot **Q-Q** e **P-P**.
- I valori numerici di R² sono stati **estratti automaticamente** dai plot diagnostici, poiché non forniti direttamente dalla libreria `pyextremes`.

### 📊 Alcuni esempi di risultati R²:
- Soglia **81** → QQ: **0.9950**, PP: **0.9970**
- Soglia **84** → QQ: **0.9930**, PP: **0.9960**
- Soglia **88** → QQ: **0.9920**, PP: **0.9960**

➡️ La soglia **81 mm** ha fornito i valori **più alti di R²** → selezionata per l’analisi finale.

---

## 3. Analisi POT finale (soglia ottimale = 81 mm)

### 📈 Parametri GPD:
- `c = 0.034`, `loc = 81.314`, `scale = 21.540`
- Il parametro di forma positivo ma molto vicino a 0 → distribuzione simile a quella **esponenziale**.

### 📈 Valori di ritorno stimati (IC 95%):
- 1 anno: **84.900 mm** (IC: 26.588 - 97.558 mm)
- 2 anni: **100.096 mm** (IC: 96.051 - 145.631 mm)
- 5 anni: **120.745 mm** (IC: 114.490 - 179.238 mm)
- 10 anni: **136.803 mm** (IC: 128.438 - 193.353 mm)

---

## 🔁 Confronto e Conclusioni

### 💧 Valori di ritorno coerenti tra BM e POT:
- 5 anni → BM: **119.5 mm** | POT: **120.7 mm**
- 10 anni → BM: **133.6 mm** | POT: **136.8 mm**

Le differenze sono **modeste**, considerando l’ampiezza degli intervalli di confidenza.

---

### 📈 Bontà di Stima (Fit dei Modelli):

- I **plot Q-Q e P-P** sono stati generati per tutti i modelli (BM e POT con soglie 91 e 81).
- L’allineamento dei punti lungo la diagonale nei plot indica un buon fit.
- Il progetto ha **quantificato la bontà del fit** con R², superando 0.99 per le soglie migliori.
- Questo supporta numericamente ciò che già si osservava visivamente.

---

### ✅ Conclusione

Entrambi i modelli EVT — **BM con GEV** e **POT con GPD (soglia 81 mm)** — forniscono stime **coerenti e affidabili** per eventi estremi nella regione Lazio.

L’aggiunta della valutazione quantitativa tramite **R²** ha permesso di rafforzare l’analisi, rendendo il processo di selezione della soglia **rigoroso e giustificabile**.
