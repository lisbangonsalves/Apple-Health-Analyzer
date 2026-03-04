# 🏋️ Apple Health Analyzer

A Google Colab notebook that parses your Apple Health export and gives you a full interactive dashboard — no server, no deployment, no configuration. Just run and analyze.

---

## What It Does

Turns your Apple Health zip export into a rich web dashboard with recovery scoring, training load analysis, anomaly detection, and personalized recommendations — all running directly inside Colab via a Gradio web UI.

---

## Dashboard

| Tab | What You See |
|-----|-------------|
| **Summary Card** | Today's Recovery, Energy, Peak Score, HRV, RHR, Steps, Calories, and a training recommendation |
| **Timeline** | 90-day Recovery / Energy / Peak Score chart with workout day markers |
| **HRV** | Daily HRV scatter colored by recovery level, with personal baseline band |
| **Training Load** | Daily load bars, 7-day acute vs 28-day chronic load, ACWR injury risk zones |
| **Correlations** | Spearman correlation heatmap across all health metrics |
| **Clustering** | PCA scatter of your days classified into High Performance / Active Recovery / Moderate / Fatigued |
| **Anomalies** | Recovery timeline with illness and overtraining signals flagged |
| **Calendar** | Weekly recovery heatmap |
| **VO2 Max** | VO2 Max trend with age-bracket reference bands |

---

## Scores Explained

**Recovery Score (0–100)** — Calculated from HRV z-score (50%), RHR z-score (30%), and sleep quality (20%) relative to your rolling 28-day personal baseline. Not a fixed formula — it adapts to *you*.

**Energy Score (0–100)** — Normalized composite of active calories, exercise minutes, steps, and workout load.

**Peak Score (0–100)** — Readiness to perform at maximum. Combines ACWR sweet spot proximity, recovery trend, HRV slope, and days since last heavy session.

**ACWR (Acute:Chronic Workload Ratio)** — Injury risk indicator. Sweet spot is 0.8–1.3. Above 1.5 is high risk.

---

## Getting Started

### Step 1 — Export Apple Health Data

On your iPhone: **Health app → Profile photo → Export All Health Data → Share → Save to Files**

This gives you a `export.zip` file.

### Step 2 — Open the Notebook in Colab

Upload `apple_health_gradio.ipynb` to [Google Colab](https://colab.research.google.com):

**File → Upload Notebook → Choose File**

### Step 3 — Run All Cells

Press **Ctrl+F9** (or **Runtime → Run All**).

The final cell will print a public URL like:
```
Running on public URL: https://abc123.gradio.live
```

### Step 4 — Upload and Analyze

Open the URL in any browser, upload your `export.zip`, and your dashboard loads automatically.

> **Note:** The public Gradio URL is valid for 72 hours. Re-run the last cell to get a fresh one.

---

## Metrics Tracked

| Metric | Source |
|--------|--------|
| HRV (Heart Rate Variability) | Apple Watch |
| Resting Heart Rate | Apple Watch |
| Steps | iPhone / Apple Watch |
| Active & Basal Calories | Apple Watch |
| Exercise Minutes | Apple Watch |
| Sleep (Deep / REM / Core) | Apple Watch |
| VO2 Max | Apple Watch |
| Workout Load | Workouts app |
| SpO2, Respiratory Rate | Apple Watch |
| Walking Speed, Distance | iPhone / Apple Watch |
| Wrist Temperature | Apple Watch Series 8+ |

---

## Dependencies

All installed automatically in Cell 1:

```
gradio
plotly
scikit-learn
scipy
statsmodels
numpy
pandas
```

No API keys. No external services. No accounts required beyond Google Colab.

---

## Architecture

```
export.zip
    └── apple_health_export/export.xml
            │
            ▼
        parse_zip()          # Streams XML, extracts records & workouts
            │
            ▼
      build_dataframe()      # Aggregates by day, merges sleep & workout data
            │
            ▼
       run_analysis()        # Recovery, Energy, Peak, ACWR, Anomaly, Clustering, PCA
            │
            ▼
       build_charts()        # Returns Plotly figures
            │
            ▼
       build_summary()       # Today's stats + training recommendation
            │
            ▼
        Gradio UI            # Interactive web dashboard
```

---

## Privacy

Your health data never leaves your session. Everything runs locally inside your Colab runtime — the zip is parsed in memory and nothing is uploaded to any external server. The Gradio public URL tunnels directly to your Colab instance.

---

## Limitations

- Requires Apple Watch for most metrics (iPhone-only exports will have steps and distance but not HRV, RHR, sleep stages, or VO2 Max)
- Recovery scoring needs at least ~5 days of HRV data to establish a baseline; 28+ days gives the most accurate results
- The Gradio public URL expires after 72 hours — re-run the last cell to get a new one
- Large exports (several years of data) may take 1–2 minutes to parse

---

## File Structure

```
apple_health_gradio.ipynb   # The notebook — this is all you need
README.md                   # This file
```
