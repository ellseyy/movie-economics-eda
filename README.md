# Movie Economics EDA

A quantitative exploratory data analysis investigating capital allocation efficiency, audience rating decoupling, genre dominance shifts, and theatrical seasonality using the TMDb dataset (3,229 titles, 1980–2015).

## Tech Stack & Architecture

- **Runtime & Environment:** Python 3.10+ (`venv`) / Jupyter Notebooks
- **Data Processing & Stats:** Pandas / NumPy / SciPy (`pearsonr`, `spearmanr`)
- **Visualization Engine:** Matplotlib / Seaborn
- **Dataset Pipeline:** TMDb API historical records (processed to `tmdb_movies_cleaned.csv`)

## Core Hypotheses & Findings

### 1. Capital Allocation & Diminishing Returns (H1)
- **Status:** Validated
- **Insight:** Production budget strongly correlates with absolute gross revenue (Pearson **0.705**, Spearman **0.679**), but relative returns decay exponentially. Low-budget films (**>=10M**) yield the highest median multiple (**2.63x**).
- **Industry Squeeze:** Mid-budget films (**10M-50M**) carry the highest failure risk, recording the lowest baseline win rate (**72.3%**).
<img width="792" height="469" alt="Снимок экрана 2026-10-14 в 6 42 06 PM" src="https://github.com/user-attachments/assets/184c5b52-a2d4-4c25-a656-0beb1de73dc6" />


### 2. Rating Decoupling & Distribution Reach (H2)
- **Status:** Validated
- **Insight:** Audience rating (`vote_average`) demonstrates near-zero linear correlation with box office gross (**0.17**), decoupling perceived quality from commercial scale.
- **Reach Proxy:** Review volume (`vote_count`) strongly tracks theatrical gross (**0.76**), confirming that box office scales with top-of-funnel marketing reach rather than post-watch satisfaction.
<img width="751" height="466" alt="Снимок экрана 2026-10-15 в 11 44 17 AM" src="https://github.com/user-attachments/assets/04f91ec1-d37b-4115-baf5-d52c59efeadf" />


### 3. Genre Market Dominance & Prime Eras (H3)
- **Status:** Validated
- **Insight:** Tracked market share shifts via 3-Year Simple Moving Average (SMA):
  - **Action Ascent:** Peaked at **29.1%** in **1994** (*True Lies*), establishing long-term dominance via global spectacle.
  - **Drama Contraction:** Peaked at **26.2%** in **2006** (*The Pursuit of Happyness*) before storytelling migrated to prestige streaming/TV.
  - **Survivorship Bias:** Pre-1990 ratings are mathematically inflated due to curation bias in digital archives.
<img width="753" height="378" alt="Снимок экрана 2026-10-15 в 11 44 45 AM" src="https://github.com/user-attachments/assets/37b56940-8ebe-4e39-ba65-1b3d880c377b" />


### 4. Theatrical Seasonality & "Dump Months" (H4)
- **Status:** Validated
- **Insight:** A **4.08x revenue spread** exists between prime theatrical windows and dead zones:
  - **Peak Window:** June leads all months with a median revenue of **112.3M**, followed by December (**93.7M**).
  - **Dump Month:** September forms the deepest trough with a median gross of only **27.5M**.
<img width="751" height="403" alt="Снимок экрана 2026-10-15 в 11 45 26 AM" src="https://github.com/user-attachments/assets/32208863-cd2e-4ba0-8194-4aa4f0bc4861" />


## Feature Engineering Pipeline

| Feature | Type | Logic / Definition | Purpose |
| :--- | :--- | :--- | :--- |
| `roi_multiplier` | float | `(revenue - budget) / budget` | Capital efficiency multiple |
| `net_profit` | float | `revenue - budget` | Absolute dollar margin |
| `is_profitable` | bool | `net_profit > 0` | Binary classification for portfolio win rate |
| `budget_tier` | categorical | `<10M`, `10M-50M`, `50M-100M`, `100M+` | Risk segmentation by production scale |
| `primary_genre` | string | Extracted first genre item | Prevents multi-label variance dilution |
| `decade` / `month` | categorical | Parsed from `release_date` | Time-series and seasonality cohorts |

## Repository Structure

```text
movie-economics-eda/
├── data/
│   ├── raw/
│   │   └── tmdb_movies.csv
│   └── processed/
│       └── tmdb_movies_cleaned.csv
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   └── 02_eda_hypotheses.ipynb
├── assets/                         <img width="753" height="445" alt="Снимок экрана 2026-10-15 в 11 36 46 AM" src="https://github.com/user-attachments/assets/3bed49df-b3f2-4b4b-99c9-f44940f321ba" />

├── .gitignore
├── README.md
└── requirements.txt
