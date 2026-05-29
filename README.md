# 🌍 Climate Data Analysis: CO₂ Emissions and Global Temperature Trends

This project explores the statistical relationship between global fossil fuel $CO_2$ emissions and global temperature anomalies using publicly available datasets.

Rather than just a static analysis, this repository demonstrates a **three-stage data engineering evolution**—progressing from a localized Pandas prototype to a highly scalable, production-ready **distributed PySpark pipeline** built to handle real-world data data-type mismatches, messy web sources, and cluster memory constraints.

---

## 🏗️ Pipeline Evolution and Repository Directory

The repository is structured to show how code scales from local data analysis to distributed big data architectures:

### 📁 1. [01_local_pandas_pipeline.ipynb](https://github.com/personacarvedin/ClimateChangeAnalysis/blob/main/01_local_pandas_pipeline.ipynb) (Local Prototype)

* **Framework:** Python, Pandas
* **Objective:** Quick ingestion, EDA, and baseline correlation attempt.
* **The Reality Check:** Highlighting standard real-world engineering failures. The analysis breaks down (`Length of anomaly: 0`) due to uncleaned string data and textual metadata headers buried inside raw CSV web formats.

### 📁 2. [02_hybrid_spark_migration.ipynb](https://github.com/personacarvedin/ClimateChangeAnalysis/blob/main/02_hybrid_spark_migration.ipynb) (Hybrid Spark Architecture)

* **Framework:** PySpark, Pandas
* **Objective:** Address data-type mismatch using strict schemas.
* **The Solution:** Explicitly enforces type casting on Pandas dataframes before loading them into a local `SparkSession`. Employs a relational inner join to cleanly match **144** overlapping data points, revealing a massive Pearson correlation coefficient of **0.93** ($P\text{-value}: 5.22 \times 10^{-65}$).

### 📁 3. [03_distributed_pyspark_production.ipynb](https://github.com/personacarvedin/ClimateChangeAnalysis/blob/main/03_distributed_pyspark_production.ipynb) (Production Enterprise Pipeline)

* **Framework:** Distributed PySpark, Cluster-Optimized Core Engine
* **Objective:** Build a scalable ETL engine capable of running seamlessly on multi-node Hadoop/Cloud architectures without driver memory bottlenecks.
* **Production Features Built:**
* **Automated Header Detection Layer:** Programmatically scrapes source HTML headers via `urllib` to dynamically skip metadata variations.
* **Fault-Tolerant Type Casting:** Utilizes native Spark SQL `try_cast()` to gracefully handle non-standard text anomalies (like NASA's `***` missing value string flags) across split partitions without pipeline disruption.
* **Cluster Optimization & Smart Downsampling:** Performs all heavy filtering, sorting, and analytical groupings natively across cluster nodes *prior* to driver data aggregation (`.toPandas()`), preventing Out-Of-Memory (OOM) errors.



---

## 📊 Analytics and Visualizations Built

The analytical pipelines generate four distinct visualization structures used to drive strategic climate awareness:

1. **Global Temperature Anomaly Chart (NASA GISTEMP):** A temporal line chart tracking temperature variations from 1880–Present.
2. **Global $CO_2$ Trends (Our World in Data):** Interactive line graphs showcasing accelerating industrial emissions.
3. **Interactive Country Choropleth Map:** An interactive global dashboard tracking $CO_2$ distribution patterns across political boundaries using ISO codes.
4. **Top Emitters Bar Chart Analysis:** A dynamic ranking engine tracking the top 5 global emitters for any specified calendar year.

---

## ⚙️ Dependencies and Setup

The project is built around the Big Data technology stack. Ensure you have an active Apache Spark environment (or a managed runtime like Google Colab) configured.

### Required Python Packages

* `pyspark`
* `pandas`
* `matplotlib`
* `plotly`
* `scipy`

Install dependencies locally or on your cluster nodes via:

```bash
pip install pyspark pandas matplotlib plotly scipy

```

---

## 📁 Data Assets Managed

* **Global Temperature Anomaly Data (NASA GISTEMP v4):** Captures annual global temperature anomalies spanning from 1880 to the present day.
* **$CO_2$ Emissions Matrix (Our World in Data):** Consolidates global and country-specific fossil fuel $CO_2$ metrics, tracking historical emissions trends across international boundaries.

---

## 📐 Enterprise Data Metrics (Final Output)

Upon execution of the **Version 3 Production Pipeline**, the distributed join and type-casting modules successfully pull an extra year of data masked by previous string errors, yielding:

* **Aligned Analytical Records:** 145 Years
* **Pearson Correlation Coefficient ($r$):** 0.93
* **Statistical Significance ($p$-value):** $6.44 \times 10^{-66}$
