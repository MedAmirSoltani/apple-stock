# 📈 Apple Stock Time Series Forecasting (SARIMA)

A time series analysis and forecasting project on **AAPL (Apple Inc.) stock closing prices**, built entirely in R. The project covers the full pipeline from data preprocessing to model evaluation, comparing auto-fitted and manually tuned SARIMA models.

---

## 🗂️ Dataset

- **Source:** AAPL historical stock data 
- **Features used:** `Date`, `Close` (daily closing price)
- **Analysis window:** 2010–2022 (12 years of monthly aggregated data)

---

## 🔧 Pipeline Overview

### 1. Data Loading & Preprocessing
- Parsed dates using `lubridate` and selected `Date` + `Close` columns
- Completed the date sequence to ensure continuity (daily → no gaps)
- Filled missing values (weekends/holidays) using `zoo::na.fill()` with the `"extend"` strategy

### 2. Exploratory Analysis
- Plotted the last 200 days and the full historical series
- Checked for outliers via boxplot — high values confirmed as real market prices, not anomalies

### 3. Data Aggregation & Conversion to Time Series
- Aggregated daily data to **monthly averages** using `lubridate::floor_date()`
- Converted to `ts` object with `frequency = 12` (monthly seasonality)
- Windowed to 2010–2022 for relevance

### 4. Train/Test Split
- **Test set:** last 24 months
- **Train set:** everything before

### 5. Decomposition
- Applied **multiplicative decomposition** — confirmed upward trend + seasonality
- Justified use of **SARIMA** over plain ARIMA

---

## 📊 Modeling

### Stationarity Testing
- ADF test on raw train data → `p-value = 0.99` → non-stationary
- Applied seasonal differencing (`lag=12`) + first-order differencing → stationary

### Auto SARIMA
- Fitted using `forecast::auto.arima()` with `seasonal = TRUE`
- Used as baseline

### Manual SARIMA — Grid Search
Explored combinations of:
- `p ∈ {0,1,2,3}`, `d = 1`, `q ∈ {0,1,2}`
- `P ∈ {0,1,2}`, `D = 1`, `Q ∈ {0,1}`

**Best manual model:** `SARIMA(2,1,1)(0,1,0)[12]` — lowest AIC among manual candidates

---

## 📉 Results & Evaluation

| Model | Notes |
|---|---|
| Auto SARIMA | Failed to capture the upward trend; forecasts diverged from test data |
| Manual SARIMA (2,1,1)(0,1,0) | Better directional accuracy; tracks test data's increasing trend |

Evaluation metric: `accuracy()` from the `forecast` package (RMSE, MAE, MAPE).

**Conclusion:** The manually tuned SARIMA model outperformed the auto-fitted version in capturing trend direction.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| R | Core language |
| `forecast` | ARIMA/SARIMA modeling & forecasting |
| `lubridate` | Date parsing & aggregation |
| `zoo` | Missing value imputation |
| `ggplot2` | Visualization |
| `tseries` | ADF stationarity test |
| `MLmetrics` | Evaluation metrics |

---

## 📁 File Structure

```
├── amir-soltani.ipynb   # Main analysis notebook (R kernel)
└── AAPL.csv             # Input dataset (from Kaggle)
```

---

## 🚀 How to Run

1. Clone the repository
2. Place `AAPL.csv` in the appropriate input path (or update the path in the notebook)
3. Open `amir-soltani.ipynb` in Jupyter with an R kernel (or RStudio)
4. Run all cells sequentially

```r
# Install required packages if needed
install.packages(c("lubridate", "forecast", "MLmetrics", "ggplot2", "tidyr", "zoo", "tseries"))
```

---

## 👤 Author

**Mohamed Amir Soltani**  
AI Engineer | MS Big Data — UTT  
[GitHub](https://github.com/MedAmirSoltani)
