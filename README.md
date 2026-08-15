# Time Series Analysis & Forecasting: CALABARZON Elementary Enrollment

Exploratory time series decomposition and forecasting of historical elementary school enrollment in **Region IV-A (CALABARZON), Philippines**, comparing **ARIMA** and **Facebook Prophet** to identify which model best captures a declining, non-linear enrollment trend (2010–2020).

## Overview

This project analyzes 11 years (SY 2010–2011 to SY 2019–2020) of elementary enrollment data for Region IV-A to answer a practical planning question: *is enrollment trend-driven or seasonal, and which forecasting model handles its recent decline better?*

The analysis decomposes the series into trend, seasonal, and residual components, statistically tests for stationarity, and then fits and evaluates two forecasting approaches head-to-head.

## Repository Contents

| File | Description |
|---|---|
| `Time_Series_Region_IV_A_CALABARZON_Decomposition-Analysis-Forecasting.ipynb` | Main Jupyter/Colab notebook containing all data prep, decomposition, statistical testing, modeling, and evaluation code |
| `Historical-Number-of-Enrollment-in-REGION-IV-A-Elementary.csv` | Raw source dataset (CSV) — enrollment counts by grade level (Kindergarten–Grade 6), broken down by sex, per school year |
| `Historical-Number-of-Enrollment-in-REGION-IV-A-Elementary.xlsx` | Same raw dataset in Excel format |
| `data_total.csv` | Cleaned, reshaped output exported from the notebook — a simplified `School Year` vs. `Total Elementary` enrollment series used as the model input |

## Methodology

1. **Data preparation** — Loaded the raw multi-level CSV (grade × sex breakdown), cleaned column headers, converted comma-formatted numbers, and aggregated to a single `Total Elementary` enrollment series indexed by school year.
2. **Time series decomposition** — Used `statsmodels.tsa.seasonal.seasonal_decompose` (additive model) to split the series into trend, seasonal, and residual components.
3. **Stationarity testing** — Ran the **Augmented Dickey-Fuller (ADF) test** on the original series and on first- and second-order differences to assess stationarity and estimate the ARIMA differencing parameter `d`.
4. **ACF/PACF analysis** — Used autocorrelation and partial autocorrelation plots on the differenced series to inform the ARIMA `p` and `q` orders.
5. **Modeling** — Fit both an **ARIMA** model and a **Prophet** model on a training split, then generated forecasts for the held-out years.
6. **Evaluation** — Compared both models using MSE, RMSE, MAE, and MAPE, and visualized actual vs. predicted enrollment.

## Key Findings

- **Trend-dominant, non-seasonal series**: the decomposition showed the trend component nearly identical to the original data, with flat seasonal and residual components — expected for annual (non-sub-yearly) data.
- **Non-stationary series**: the ADF test on the raw series failed to reject the null hypothesis of non-stationarity (p = 0.118), and differenced series also showed weak stationarity signals — attributed to the small sample size (n = 11) and a structural break (a sharp enrollment decline from 2018–2020).
- **Prophet outperformed ARIMA**: Prophet's forecast captured the 2018–2020 downturn more accurately than ARIMA, which predicted an unrealistic steady linear increase.

| Metric | ARIMA | Prophet |
|---|---|---|
| MAE | ~81,000 | ~61,359 |
| RMSE | higher (large errors during the downturn) | 84,064 |
| MAPE | — | 2.99% |

Prophet's MAPE of **2.99%** means its forecasts were, on average, within 3% of actual enrollment — a strong result given the trend-driven, structurally-breaking nature of the data.

## Tools & Libraries

- Python (`pandas`, `numpy`, `matplotlib`)
- `statsmodels` (seasonal decomposition, ADF test, ACF/PACF, ARIMA)
- `prophet` (Facebook/Meta's forecasting library)
- `scikit-learn` (evaluation metrics: MSE, MAE, MAPE)
- Developed in Google Colab

## How to Run

1. Clone this repository.
2. Open `Time_Series_Region_IV_A_CALABARZON_Decomposition-Analysis-Forecasting.ipynb` in Jupyter Notebook, JupyterLab, or Google Colab.
3. Ensure `Historical-Number-of-Enrollment-in-REGION-IV-A-Elementary.csv` is in the same directory (or update the `data_path` variable if running locally instead of on Colab, where it currently points to `/content/...`).
4. Install dependencies: `pip install pandas numpy matplotlib statsmodels prophet scikit-learn`.
5. Run all cells in order.

## Data Source

Historical elementary enrollment figures for Region IV-A (CALABARZON), Philippines, by school year (SY 2010–2011 to SY 2019–2020).
