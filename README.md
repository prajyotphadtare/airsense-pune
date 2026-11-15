# AirSense Pune: Next-Day AQI Forecasting

Predicting tomorrow's Air Quality Index (AQI) in Pune from today's pollutant readings and recent history, and flagging likely pollution-spike days (AQI > 100). Built as my M.Sc. Data Science mini project (MP III) at MIT World Peace University, Pune.

## Highlights
- Cleaned faulty sensor readings and built lag, rolling-window and seasonal features from daily PM2.5, NO₂ and AQI data.
- Compared a persistence baseline with Ridge Regression, Random Forest and Gradient Boosting using a **chronological** train/test split (no future leakage).
- Evaluated both as a regressor (MAE, RMSE, R²) and as a spike-alert system (precision, recall, F1).
- Explained the model with permutation importance.

## Data
[Daily Air Quality Index and Pollutant Concentrations for Pune, India](https://data.mendeley.com/datasets/y6p778djzj) (Mendeley Data), compiled from Maharashtra Pollution Control Board stations via OpenAQ. I used the city-averaged daily file.

- 2,022 daily records (9 Mar 2018 to 28 Jan 2026). No records exist for 2023 and 2024.
- 1,344 days with a valid AQI after cleaning; 962 usable days after feature construction.
- Train: 770 days (16 Mar 2018 to 5 Jul 2025). Test: 192 days (6 Jul 2025 to 27 Jan 2026).

The dataset is not included in this repo. Download it and set `DATA_FILE` in the script.

## Results (test set)

| Model | MAE | RMSE | R² | Spike precision | Spike recall | Spike F1 |
|---|---|---|---|---|---|---|
| Persistence baseline | 20.1 | 42.9 | 0.268 | 0.890 | 0.881 | 0.886 |
| Ridge Regression | 21.1 | 35.6 | 0.496 | 0.912 | 0.723 | 0.807 |
| Random Forest | 22.6 | 36.4 | 0.473 | 0.925 | 0.733 | 0.818 |
| Gradient Boosting | 27.5 | 41.2 | 0.324 | 0.864 | 0.693 | 0.769 |

- Ridge Regression had the lowest RMSE (about 17% below the baseline) and the highest R².
- The gains are modest: the persistence baseline had the lowest MAE and caught more spike days.
- The largest misses were sudden one-day peaks (for example 394.8 on 5 Dec 2025, forecast 150.7).
- Today's PM2.5 and today's AQI were the most influential features.

## How to run
```bash
pip install pandas numpy scikit-learn matplotlib
# download city_averaged.csv from the Mendeley dataset, then:
python pune_aqi_forecast.py
```
The script prints the cleaning summary and the model comparison, and saves figures and tables to `results/`.

## Limitations
- Large data gaps (2023 to 2024) and only PM2.5 and NO₂ were dense enough to use as pollutant features.
- Sensor-fault limits were chosen by inspecting the data.
- A single chronological split on about six months of test data; walk-forward evaluation would be more reliable.
- No weather variables (wind, rainfall, temperature), which likely explain the missed spikes.

## Possible next steps
- Add meteorological features and test sequence models such as LSTM.
- Try a lower alert threshold (for example 90) to improve spike recall.
- Use walk-forward validation and station-level models.

## Author
Prajyot Phadtare, M.Sc. Data Science, MIT WPU, Pune
[LinkedIn](https://www.linkedin.com/in/prajyot-phadtare-667a6932b) | prajyotphadtare@gmail.com