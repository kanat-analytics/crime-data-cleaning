# Crime Incidents Data Cleaning

Cleaning a messy crime dataset: from raw, inconsistent records to a clean, analysis-ready file.

**Dataset:** [Messy Crime Dataset for Data Cleaning Practice](https://www.kaggle.com/) (Kaggle, by sananshaikh)
**Tools:** Python, Pandas, NumPy, Seaborn, Matplotlib, Google Colab

## Pipeline

Raw Data (5,250 rows, 33 columns) → Cleaning → Clean Dataset (5,050 rows, 34 columns)

## Cleaning Steps

### 1. Duplicates
- Found and removed **200 duplicate rows** (5,250 → 5,050).

### 2. Unrealistic values → NaN
- `suspect_age`, `victim_age`: values outside 0–100 (e.g. -75, 298) replaced with NaN. Rows were kept to preserve other data.
- `property_loss_usd`: **195 negative values** replaced with NaN (a loss cannot be negative).

### 3. Data types
- `incident_datetime`: text with mixed formats (`2024-04-16 08:45:03`, `05-03-2020`) converted to datetime.
- `property_loss_usd`: numbers stored as text converted to numeric. **136 non-numeric entries** became NaN.

### 4. Text categories
- `crime_type`: dozens of spellings reduced to **15 categories**. Fixed typos (`asslt` → `assault`) and merged synonyms (`drunk driving` → `dui`, `murder` → `homicide`, `larceny` → `theft`).
- `district`: abbreviations expanded (`sou` → `south`), **10 districts**.
- `suspect_gender`, `victim_gender`: standardized.
- `severity`: mixed words and digits unified into `low`, `medium`, `high`, `critical`.
- `reported_online`: `True/yes/YES/1` → `yes`, `False/no/NO/0` → `no`.

### 5. Missing values — decided column by column
- **Categories** → filled with `'unknown'`, so these cases stay visible in counts and charts.
- **Numbers** (`num_arrests`, `property_loss_usd`) → kept as NaN. Filling with 0 or the mean would invent data.
- **People and locations** (names, IDs, phone, coordinates) → kept as NaN. This data cannot be invented.
- 775 records have no suspect ID or name. This is kept as meaningful information: **suspect not identified**.

### 6. New feature
- `day_of_week` extracted from `incident_datetime`.

## Assumptions
- `severity` digits mapped as `1 = low`, `2 = medium`, `3 = high`, `4 = critical` (logical scale, not documented in the source).
- `online fraud` → `cyber crime` (could also fit `fraud`).
- `manslaughter` and `murder` → `homicide`; `armed robbery` → `robbery`.

## Visual Checks
Charts were used to verify the cleaning, not only to present results:
- **Crimes by type** revealed leftover synonyms after the first cleaning pass. After merging, all 15 categories have similar counts (308–376).
- **Crimes by severity**: the `unknown` count matches the original number of missing values (342).
- **Crimes by day of week**: confirms dates were converted correctly.
- **Average loss by severity**: nearly equal across levels, which suggests the loss values in this practice dataset were generated independently of severity.

## Files
- `crime_data_cleaning_ipynb_.ipynb` — full cleaning process
- `crime_incidents_clean.csv` — clean dataset
