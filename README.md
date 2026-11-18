# Morocco Electricity Consumption and Environmental Factors Analysis

**Track 1: Data Analytics** | AnalystLab Africa Experience Lab Internship Programme | Week 5
**Author:** Stephanie Georgewill Alaye

Exploratory analysis and visualisation of electricity consumption across three zones in Morocco, using readings taken every 10 minutes in 2017 together with temperature, humidity and wind speed.

## Business questions

1. What patterns can be identified in electricity consumption across the three zones?
2. During which hours and days is consumption highest and lowest?
3. How does consumption vary across the months of the year?
4. Are temperature, humidity and wind speed associated with consumption?
5. Which zone records the highest and lowest consumption?
6. How can historical consumption patterns support better energy planning and management?

## Files in this project

| File | Description |
|---|---|
| `Morocco_Electricity_Analysis.ipynb` | The completed Jupyter notebook with all code, outputs and charts |
| `powerconsumption.csv` | Main dataset (52,416 rows, 9 columns) |
| `powerconsumption_datadictionary.csv` | Data dictionary |
| `cleaned_power_data.csv` | Cleaned dataset with the added time columns (created by the notebook, see Step 4) |
| `Analytical_Report.pdf` | Short analytical report |
| `Business_Insights_and_Recommendations.pdf` | Key findings and recommendations |
| `README.md` | This file |

## Requirements

- Python 3 (installed with Anaconda)
- Jupyter Notebook
- Libraries: Pandas, NumPy, Matplotlib, Seaborn, Plotly

All of these except Plotly come with Anaconda. Install Plotly once with:

```
pip install plotly
```

## How to reproduce the analysis

### 1. Set up the folder

Create a folder (for example `Week5`) and place these three files in it:

- `powerconsumption.csv`
- `powerconsumption_datadictionary.csv`
- `Morocco_Electricity_Analysis.ipynb`

### 2. Open the notebook

1. Open **Anaconda Navigator** and click **Launch** under **Jupyter Notebook**.
2. Browse to the `Week5` folder and open `Morocco_Electricity_Analysis.ipynb`.

### 3. Run the notebook

Click **Kernel > Restart Kernel and Run All Cells**. The notebook runs from top to bottom and rebuilds every result and chart. The notebook reads the CSV files from the same folder, so keep them together.

### 4. Data preparation steps (reproducible)

The preparation done in the notebook is:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

df = pd.read_csv("powerconsumption.csv")

# Convert Datetime (month/day/year hour:minute) to a date-time type
df["Datetime"] = pd.to_datetime(df["Datetime"], format="%m/%d/%Y %H:%M")

# Create time-based columns
df["Year"] = df["Datetime"].dt.year
df["Month"] = df["Datetime"].dt.month
df["Day"] = df["Datetime"].dt.day
df["Hour"] = df["Datetime"].dt.hour
df["DayOfWeek"] = df["Datetime"].dt.day_name()
df["Date"] = df["Datetime"].dt.date

# Save the cleaned dataset
df.to_csv("cleaned_power_data.csv", index=False)
```

## Notebook structure

| Task | What it does |
|---|---|
| Task 1: Dataset understanding and inspection | Loads the data, checks structure, column names, missing values, duplicates, date range and the 10-minute interval, and reads the data dictionary |
| Task 2: Data cleaning and preparation | Converts Datetime and creates Year, Month, Day, Hour, DayOfWeek and Date columns |
| Task 3: Consumption analysis | Summary statistics by zone, highest and lowest periods, hourly, daily, weekly and monthly patterns |
| Task 4: Environmental relationships | Correlation table, heatmap and scatter plots for temperature, humidity and wind speed |
| Task 5: Data visualisation | Trend over time, average by zone, hourly, monthly, day-of-week and zone comparisons (Matplotlib, Seaborn, Plotly) |
| Task 6: Business insights and recommendations | Findings and recommendations (also in the PDF) |

## Key results

- Demand peaks every day at about **20:00** and is lowest at about **06:00**.
- Consumption is highest in **July and August** and lowest in **December** (all zones combined).
- **Zone 1** has the highest average consumption, then Zone 2, then Zone 3. Zone 3 is the most seasonal.
- Thursday is the highest day and Sunday the lowest, but weekly differences are small.
- Temperature has a moderate positive association with consumption, humidity a weak negative one, and wind speed a weak one.

## Assumptions and data notes

- **Unit of the power readings.** The data dictionary describes them as kilowatt-hours, but each row is a 10-minute reading. It is not confirmed whether a value is energy used in the interval or a rate at that moment. For that reason, readings were **averaged and compared, not summed into total energy**.
- **Diffuse-flow fields.** `GeneralDiffuseFlows` and `DiffuseFlows` are described as emissions gases diffusing into the atmosphere, but their values are extremely skewed and their meaning is unclear. They were not used for conclusions.
- **Wind speed.** Values cluster near 0 and near 5 with almost nothing in between, which suggests a recording or sensor-resolution issue. Wind results are treated as weak and indirect.
- **Single year.** The data covers 1 January to 30 December 2017, so patterns cannot be confirmed across multiple years.
- **Correlation is not causation.** Correlations show how variables move together, not that one causes the other.
- **Extreme values.** The highest readings occur on summer evenings and match the peak-demand pattern, so they were kept.
- Scatter plots use a random sample of 5,000 rows for readability. All other analysis uses the full dataset.
