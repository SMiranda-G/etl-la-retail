# LA Retail Sales ETL Project
This project demonstrates a complete **ETL (Extract, Transform, Load)** pipeline using Python and Pandas.
It extracts retail sales data from various sources (CSV, JSON, Parquet, SQLite), transforms it (cleaning, type conversion, feature engineering), enriches it with weather data from an external API, and finally loads the result into an SQLite database for further analysis.

---

## Overview

The project processes daily retail sales data for stores in Los Angeles. It handles:

- Reading data from multiple file formats (`.csv`, `.json`, `.parquet`) and a SQLite table.
- Concatenating daily CSV files.
- Cleaning column names, handling missing values, and fixing data types.
- Computing a new feature `rev_per_unit` (revenue per unit).
- Enriching the dataset with daily weather data (temperature, precipitation, wind speed) from the Open-Meteo API.
- Loading the final dataset into an SQLite database for querying and visualization.

---

## Technologies Used

- **Python 3.x**
- **Pandas** – data manipulation and analysis
- **SQLite3** – lightweight relational database
- **Requests** – API calls
- **Glob** – file pattern matching
- **Matplotlib & Seaborn** – basic visualizations
- **Open-Meteo API** – free weather data

---

## Pipeline Steps

### 1. Extract

Data is extracted from multiple sources:

- **Flat files**: CSV, JSON, Parquet files stored locally.  
  For daily CSV files, `glob` is used to read all files matching a pattern and concatenate them into a single DataFrame.

- **SQLite database**: Query existing tables using `pd.read_sql()`.

- **Weather API**: For each unique date in the sales data, a request is sent to the Open-Meteo Archive API to retrieve temperature, precipitation, and wind speed.

#### Code Snippet – Reading Multiple CSVs
```python
import glob
import pandas as pd

file_paths = glob.glob("LA_Retail_Sales_By_Day/*.csv")
sales_df = pd.concat([pd.read_csv(fp) for fp in file_paths], ignore_index=True)
```