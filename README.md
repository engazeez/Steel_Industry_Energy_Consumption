# Steel Industry Energy Consumption — Predicting & Understanding Plant Electricity Use

A CRISP-DM data science project analysing electricity consumption at a steel plant,
using the UCI *Steel Industry Energy Consumption* dataset. The project explores what
drives energy use, when the plant consumes the most, where power-factor inefficiency
hides avoidable cost, and how accurately consumption can be modelled — with an honest
account of why a near-perfect model is not as impressive as it first looks.

## Motivation

A steel plant pays for every kilowatt-hour it draws, and utilities add penalties when
the *power factor* (how efficiently that power is used) is poor. I wanted to turn a
year of 15-minute meter readings into answers a plant manager could act on: when to
schedule heavy production, which electrical factors matter, where efficiency is being
lost, and whether consumption can be predicted from sensor data. The dataset also sits
squarely in my domain (energy), which made the physical relationships interpretable
rather than abstract.

## Business questions

1. **When does the plant consume the most energy?** (daily and weekly demand patterns)
2. **Which measurable electrical factors most strongly drive consumption?**
3. **Does the plant show power-factor inefficiency that signals avoidable cost?**
4. **Can we predict energy consumption accurately, and what is the honest performance
   ceiling of such a model?**

## Files in this repository

| File | Description |
|------|-------------|
| `Steel_Energy_Prediction.ipynb` | Main notebook. Full CRISP-DM workflow: business understanding, EDA, data preparation, Linear Regression and Random Forest modelling, evaluation, and deployment notes. All four business questions are asked and answered with visuals/tables/statistics. |
| `Steel_industry_data.csv` | The dataset (35,040 rows × 11 columns). Also loadable via `ucimlrepo` (id 851). |
| `README.md` | This file. |

## Libraries used

- **pandas**, **numpy** — data handling and numerical work
- **matplotlib**, **seaborn** — visualisation
- **scikit-learn** — train/test split, Linear Regression, Random Forest, metrics, cross-validation
- **ucimlrepo** — optional direct download of the dataset from the UCI repository

Install with:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn ucimlrepo
```

## Summary of results

- **Q1 — Timing:** Consumption tracks the shift schedule. It is lowest overnight and at
  weekends, and peaks mid-day on weekdays (weekday mean ≈ 33.6 kWh vs weekend ≈ 11.7 kWh;
  Sunday lowest at ≈ 7.6 kWh). The weekday daytime peak is the target for load-shifting.
- **Q2 — Drivers:** Lagging reactive power is the dominant legitimate driver
  (r ≈ 0.90, ~87% of Random Forest feature importance). Key relationships are non-linear.
- **Q3 — Inefficiency:** Leading reactive power appears almost only during idle periods —
  a sign of power-factor-correction capacitors over-compensating when little is running,
  a classic source of avoidable reactive-power penalties.
- **Q4 — Prediction:** A Random Forest predicts consumption to RMSE ≈ 0.6 kWh
  (R² ≈ 0.9997, 5-fold CV-confirmed), decisively beating a Linear Regression baseline
  (R² ≈ 0.91). Crucially, that near-perfect score reflects the **power-triangle identity**
  — reactive power and power factor algebraically reconstruct active energy — not modelling
  wizardry. `CO2` was excluded as data leakage (it is computed from energy consumed).

The honest takeaway: the linear model's ~0.91 R² is the more meaningful figure for how
much a simple additive model explains, and a true forward *forecast* would use only
information known ahead of time (calendar and lagged-usage features).

## CRISP-DM process

The notebook is organised into the six CRISP-DM phases: Business Understanding →
Data Understanding → Data Preparation → Modeling → Evaluation → Deployment. Missing
values (none present) and categorical variables (dropped, with justification) are
handled and documented in the Data Preparation phase.

## Acknowledgements

- Dataset: V. E. Sathishkumar, Shin Changsun, and Cho Yongyun,
  *Steel Industry Energy Consumption*, UCI Machine Learning Repository (id 851).
  Source facility: DAEWOO Steel Co. Ltd, Gwangyang, South Korea.
- Built with the open-source Python data science stack listed above.
