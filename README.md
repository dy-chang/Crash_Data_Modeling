# 🚦 Chicago Traffic Crash Analytics

> End-to-end traffic safety analysis using **150,000+ real crash records**
> from the City of Chicago Open Data Portal.
> Covers data engineering, exploratory analysis, machine learning modeling,
> and evidence-based policy recommendations.

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-2.0-green?logo=pandas)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3-orange)
![Plotly](https://img.shields.io/badge/Plotly-5.18-purple?logo=plotly)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## 📊 Interactive Charts

| # | Chart | Key Finding |
|---|-------|-------------|
| 1 | [Crash Volume by Hour & Severity](charts/01_crash_by_hour_severity.html) | PM rush (4–6 PM) drives peak crash volume |
| 2 | [Injury Rate: Weather × Lighting](charts/02_injury_rate_weather_lighting.html) | Dark + rain/snow = highest injury risk |
| 3 | [Top Causes of Injury Crashes](charts/03_top_causes_injury.html) | Failing to yield is the #1 actionable cause |
| 4 | [Monthly Crash Trend by Year](charts/04_monthly_trend_by_year.html) | Summer peak (Jun–Sep) consistent across years |
| 5 | [ROC Curves — 3 ML Models](charts/05_roc_curves.html) | Gradient Boosting achieves best AUC |
| 6 | [Feature Importance](charts/06_feature_importance.html) | Hour, intersection, and hit-and-run are top predictors |

> 💡 Open any `.html` file locally in a browser for full hover/zoom interactivity.

---

## 🗂️ Repository Structure
chicago-crash-analytics/
├── chicago_crash_analysis.py ← full pipeline (run this)
├── requirements.txt
├── .gitignore
├── notebooks/
│ └── analysis.ipynb ← Jupyter version with inline charts
└── charts/
├── 01_crash_by_hour_severity.html
├── 02_injury_rate_weather_lighting.html
├── 03_top_causes_injury.html
├── 04_monthly_trend_by_year.html
├── 05_roc_curves.html
└── 06_feature_importance.html

## 🚀 Quickstart

```bash
git clone https://github.com/<your-username>/chicago-crash-analytics.git
cd chicago-crash-analytics
pip install -r requirements.txt
python chicago_crash_analysis.py
```

Charts are saved to `charts/`. Open any `.html` file in a browser.

---

## 📦 Data Source

| Item | Detail |
|---|---|
| **Dataset** | City of Chicago — Traffic Crashes - Crashes |
| **API Endpoint** | `data.cityofchicago.org/resource/85ca-t3if` |
| **Volume** | 150,000 most recent records (2022–2026) |
| **Update Frequency** | Daily (auto-fetched at runtime) |
| **Original Columns** | 48 (time, location, weather, lighting, cause, injury counts) |

---

## 📐 Methodology

### Feature Engineering

11 binary/numeric features derived from raw columns:

| Feature | Description |
|---|---|
| `is_dark` | Lighting = DARKNESS or DARKNESS, LIGHTED ROAD |
| `is_wet` | Road surface = WET, SNOW/SLUSH, or ICE |
| `is_rain_snow` | Weather = RAIN, SNOW, FREEZING RAIN, or SLEET |
| `is_inter` | Intersection-related crash flag |
| `is_hitrun` | Hit-and-run flag |
| `rush_hour` | Hour in [7–9 AM] or [4–6 PM] |
| `late_night` | Hour in [10 PM–4 AM] |
| `high_speed` | Posted speed limit ≥ 45 mph |
| `multi_vehicle` | 3 or more vehicles involved |
| `num_units` | Number of vehicles (clipped 1–5) |
| `cause_*` | One-hot dummies for top 6 contributory causes |

### Machine Learning

**Task:** Binary classification — predict whether a crash results in **injury**
(vs. Property Damage Only)

**Models trained:**

| Model | Key Parameters |
|---|---|
| Logistic Regression | L2 regularization, `class_weight=balanced` |
| Random Forest | 100 trees, `max_depth=6`, `class_weight=balanced` |
| Gradient Boosting | 150 estimators, `lr=0.05`, `max_depth=4`, `subsample=0.8` |

**Evaluation:** ROC-AUC on 20% held-out test set
(stratified split, N = 50,000 random sample)

---

## 🔍 Key Findings

1. **PM rush hour (4–6 PM)** accounts for the highest absolute crash volume,
   but **late-night crashes (10 PM–4 AM)** carry a significantly elevated injury rate.

2. **Dark lighting + rain/snow** is the highest-risk environmental combination —
   injury rate is roughly 2× that of clear/daylight conditions.

3. **Failing to Yield Right-of-Way** and **Following Too Closely** are the
   top two actionable causes of injury crashes (excluding undetermined cases).

4. **Hit-and-run** and **intersection** flags are among the top ML predictors
   of injury severity — strong candidates for targeted enforcement.

5. **Summer months (Jun–Sep)** consistently show peak crash volumes across
   all years, suggesting seasonal traffic volume as a primary driver.

---

## 📋 Policy Recommendations

| Priority | Finding | Recommended Action |
|---|---|---|
| 🔴 High | Intersection = top ML predictor | Protected-left signals + pedestrian refuges at flagged intersections |
| 🔴 High | Late-night injury rate elevated | DUI checkpoints + automated speed cameras after 10 PM |
| 🟠 Medium | Dark + adverse weather = highest risk | Dynamic speed limits on arterials via RWIS sensor triggers |
| 🟠 Medium | High-speed roads (≥ 45 mph) | Speed cameras, median barriers, rumble strips on risk corridors |
| 🟡 Standard | Hit-and-run = strong injury predictor | Expand traffic camera network; increase penalties |
| 🟡 Standard | Yield + tailgating = #1 cause | Public education campaigns + automated enforcement at hotspots |

---

## 🛠️ Tech Stack

- **Data collection:** Socrata Open Data API (no API key required)
- **Data processing:** `pandas`, `numpy`
- **Modeling:** `scikit-learn` — LogisticRegression, RandomForestClassifier, GradientBoostingClassifier
- **Visualization:** `plotly` — interactive HTML charts (no kaleido required)

---

## 📄 License

MIT License — crash data © City of Chicago (Open Data Portal)
