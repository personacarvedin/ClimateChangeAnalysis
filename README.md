# 🌍 Climate Data Analysis: CO₂ Emissions and Global Temperature Trends

This project explores the statistical relationship between global fossil fuel $CO_2$ emissions and global temperature anomalies using publicly available datasets.

Rather than just a static analysis, this repository demonstrates a **three-stage data engineering evolution**—progressing from a localized Pandas prototype to a highly scalable, production-ready PySpark distributed pipeline. Each stage reveals real-world engineering challenges and solutions.

---

## 🏗️ Pipeline Evolution and Repository Directory

The repository is structured to show how code scales from local data analysis to distributed big data architectures:

### 📁 1. [01_local_pandas_pipeline.ipynb](https://github.com/personacarvedin/ClimateChangeAnalysis/blob/main/01_local_pandas_pipeline.ipynb) (Local Prototype)

* **Framework:** Python, Pandas
* **Objective:** Quick ingestion, EDA, and baseline correlation attempt.
* **The Reality Check:** Highlighting standard real-world engineering failures. The analysis breaks down (`Length of anomaly: 0`) due to uncleaned string data and textual metadata headers buried inside the CSV files. The temperature dataset stores anomaly values as strings (e.g., `"-.17"`), which fail to parse when attempting numeric operations. Additionally, the datasets have non-overlapping year ranges (temperature: 1880–present, CO₂: 1750–present), requiring careful alignment.
* **Output:** Correlation computation **fails** — no valid data points for statistical analysis.

### 📁 2. [02_hybrid_spark_migration.ipynb](https://github.com/personacarvedin/ClimateChangeAnalysis/blob/main/02_hybrid_spark_migration.ipynb) (Hybrid Spark Architecture)

* **Framework:** PySpark, Pandas
* **Objective:** Address data-type mismatch using strict schemas and explicit type casting.
* **The Solution:** Explicitly enforces type casting on Pandas dataframes before loading them into a local `SparkSession`. Converts string anomaly values to `double` type and applies strict integer casting to years. Employs a relational inner join to cleanly match overlapping data points. Successfully recovers data that Pandas silently dropped.
* **Output:** **144 overlapping years**, Pearson correlation **0.93**, p-value **5.22e-65**
* **Limitation:** One year of valid data is still lost due to strict type casting rules; datasets with malformed markers (like NASA's `***` for missing values) are discarded entirely.

### 📁 3. [03_distributed_pyspark_production.ipynb](https://github.com/personacarvedin/ClimateChangeAnalysis/blob/main/03_distributed_pyspark_production.ipynb) (Production Enterprise Pipeline)

* **Framework:** Distributed PySpark, Cluster-Optimized Core Engine
* **Objective:** Build a scalable ETL engine capable of running seamlessly on multi-node Hadoop/Cloud architectures without driver memory bottlenecks.
* **Production Features Built:**
  * **Automated Header Detection Layer:** Programmatically scrapes source HTML headers via `urllib` to dynamically skip metadata variations across different data sources.
  * **Fault-Tolerant Type Casting:** Utilizes native Spark SQL `try_cast()` to gracefully handle non-standard text anomalies across split partitions without crashing or data loss.
  * **Cluster Optimization & Smart Downsampling:** Performs all heavy filtering, sorting, and analytical groupings natively across cluster nodes *prior* to driver data aggregation (`.toPandas()`), preventing out-of-memory failures and maximizing throughput.
  * **Native Spark DataFrame Operations:** Uses `expr()` and Spark SQL functions to push computation to distributed nodes, reducing data movement and improving performance on enterprise clusters.
* **Output:** **145 overlapping years** (recovers 1 additional year masked by strict type casting), Pearson correlation **0.93**, p-value **6.44e-66**
* **Key Achievement:** The fault-tolerant `try_cast()` function successfully recovers data that both Pandas and hybrid Spark approaches rejected, demonstrating the advantage of enterprise-grade error handling.

---

## 📊 Pipeline Evolution Comparison

| Metric | Notebook 1 (Pandas) | Notebook 2 (Hybrid Spark) | Notebook 3 (Production) |
|--------|---------------------|--------------------------|------------------------|
| **Framework** | Pandas | PySpark (Local) | PySpark (Distributed) |
| **Data Quality Handling** | None | Strict type casting | Fault-tolerant `try_cast()` |
| **Years Aligned** | 0 ❌ | 144 ✓ | 145 ✓✓ |
| **Pearson Correlation** | N/A (failed) | 0.93 | 0.93 |
| **P-value** | N/A (failed) | 5.22e-65 | 6.44e-66 |
| **Status** | Analysis crashes | Works locally | Production-ready |
| **Scalability** | Single machine | Single machine | Multi-node clusters |

**Key Insight:** The production pipeline recovers 1 additional year of data by gracefully handling malformed values (NASA's `***` markers) that would crash or be silently dropped by simpler approaches. This demonstrates why enterprise data engineering practices matter for real-world datasets.

---

## 📊 Analytics and Visualizations Built

The analytical pipelines generate four distinct visualization structures used to drive strategic climate awareness:

1. **Global Temperature Anomaly Chart (NASA GISTEMP):** A temporal line chart tracking temperature variations from 1880–Present, showing accelerating warming trends in recent decades.
2. **Global $CO_2$ Trends (Our World in Data):** Interactive line graphs showcasing accelerating industrial emissions across the 20th and 21st centuries.
3. **Interactive Country Choropleth Map:** An interactive global dashboard tracking $CO_2$ distribution patterns across political boundaries using ISO country codes, enabling geographic comparison.
4. **Top Emitters Bar Chart Analysis:** A dynamic ranking engine tracking the top 5 global CO₂ emitters for any specified calendar year, revealing shifts in industrial dominance over time.

---

## ⚙️ Dependencies and Setup

The project is built around the Big Data technology stack. Ensure you have an active Apache Spark environment (or a managed runtime like Google Colab) configured.

### Required Python Packages

* `pyspark` — Distributed data processing framework
* `pandas` — Local data manipulation and conversion
* `matplotlib` — Static visualization library
* `plotly` — Interactive visualization library
* `scipy` — Statistical analysis (Pearson correlation)

Install dependencies locally or on your cluster nodes via:

```bash
pip install pyspark pandas matplotlib plotly scipy
```

### Running the Notebooks

1. **Local Prototype (Notebook 1):** Runs on any machine with Python and Pandas
2. **Hybrid Spark (Notebook 2):** Requires local PySpark installation or Google Colab
3. **Production Pipeline (Notebook 3):** Optimized for multi-node Spark clusters; runs on Google Colab, AWS EMR, Databricks, or on-premises Hadoop clusters

---

## 📁 Data Assets Managed

* **Global Temperature Anomaly Data (NASA GISTEMP v4):** Captures annual global temperature anomalies spanning from 1880 to the present day. Source: https://data.giss.nasa.gov/gistemp/
* **$CO_2$ Emissions Matrix (Our World in Data):** Consolidates global and country-specific fossil fuel $CO_2$ metrics, tracking historical emissions trends across international boundaries. Source: https://github.com/owid/co2-data

---

## 📐 Enterprise Data Metrics (Final Output)

Upon execution of the **Version 3 Production Pipeline**, the distributed join and type-casting modules successfully pull an extra year of data masked by previous string errors, yielding:

* **Aligned Analytical Records:** 145 Years
* **Pearson Correlation Coefficient ($r$):** 0.93
* **Statistical Significance ($p$-value):** $6.44 \times 10^{-66}$

This exceptionally strong correlation (0.93) with ultra-high statistical significance ($p < 10^{-60}$) provides compelling evidence of a robust causal relationship between global CO₂ emissions and observed temperature anomalies over the past 145 years.

---

## 🎯 What This Project Demonstrates

This repository is a **teaching project** that illustrates critical data engineering concepts:

- **Data Quality Challenges:** Real-world datasets are messy; naive approaches fail
- **Schema Evolution:** Type casting and validation are essential at scale
- **Error Handling:** Fault-tolerant operations recover data that strict approaches discard
- **Distributed Computing:** Pushing computation to cluster nodes prevents memory bottlenecks
- **Statistical Rigor:** Proper alignment and quality control enable reliable analysis
