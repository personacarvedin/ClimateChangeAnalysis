# 🌍 Climate Data Analysis: CO₂ Emissions & Global Temperature Trends

This project explores the relationship between global CO₂ emissions and global temperature anomalies using publicly available datasets.  
It includes data processing, visualization, and statistical analysis to assess whether rising emissions correlate with temperature change.

---

## 📦 Dependencies

The following Python libraries are required:

- `pandas`
- `matplotlib`
- `plotly`
- `scipy`
- `pyspark`

Install them using:

```bash
pip install plotly pandas matplotlib scipy
````

---

## 📁 Data Sources

| Dataset                    | Source            | Description                                           |
| -------------------------- | ----------------- | ----------------------------------------------------- |
| Global Temperature Anomaly | NASA GISTEMP      | Annual global temperature anomalies from 1880–present |
| CO₂ Emissions              | Our World in Data | Global annual fossil fuel CO₂ emissions               |

---

## 📊 Analysis Steps

### 1️⃣ Load datasets

The project fetches temperature and CO₂ data directly from online sources:

```python
url_temp = 'https://data.giss.nasa.gov/gistemp/tabledata_v4/GLB.Ts+dSST.csv'
url_co2 = 'https://raw.githubusercontent.com/owid/co2-data/master/owid-co2-data.csv'
```

### 2️⃣ Data Cleaning & Preparation

* Select relevant year/anomaly/emissions columns
* Remove missing values
* Align both datasets to shared year ranges

### 3️⃣ Visualizations

The notebook generates multiple plots:

#### 📈 Global Temperature Anomaly (NASA)

A line chart showing temperature anomaly trends across years.

#### 🔥 Global CO₂ Emissions

A line chart showing global emissions growth.

#### 🗺️ Interactive CO₂ Choropleth Map

World map highlighting emission levels by country.

#### 🌍 Top Emitters

Plot comparing the top 5 emitting countries in the most recent available year.

---

## 📐 Statistical Correlation

The code attempts to compute Pearson correlation between CO₂ emissions and temperature anomalies:

```python
corr, p_value = pearsonr(anomaly, co2)
print(f"Pearson Correlation: {corr:.2f}")
print(f"P-value: {p_value}")
```

> ⚠️ In the current run, no overlapping years were found between datasets, so the correlation could not be computed:

```
Length of anomaly: 0  
Length of co2: 0  
Not enough data points to compute correlation.
```

---

## 🧪 Future Improvements

* Align both datasets to a matching year range
* Add moving averages or trend lines
* Expand analysis to methane, land use, or renewable capacity
* Export clean visual reports

---

## 🧭 Project Goal

This notebook supports climate awareness by demonstrating:

* Historical trends in fossil fuel emissions
* Observable temperature anomaly increase
* Global distribution of emissions
* Emerging geographic leaders in climate impact

---

