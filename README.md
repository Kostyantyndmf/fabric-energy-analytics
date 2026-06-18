# End-to-End Energy Transition & Economic Decoupling Pipeline

An end-to-end Analytics Engineering project built within **Microsoft Fabric**, utilizing a **Medallion Architecture** to ingest, transform, and model global energy and economic data from *Our World in Data*. The final objective is to analyze the decoupling correlation between GDP growth and Carbon Intensity from 1985 to 2025.

## 🛠️ Tech Stack & Architecture

* **Orchestration & Ingestion:** Microsoft Fabric Data Factory (Dataflow Gen2)
* **Storage / Data Lake:** OneLake (Delta Parquet format)
* **Data Warehousing:** Synapse Data Warehouse (T-SQL)
* **Semantic Layer:** Power BI Semantic Model via **DirectLake** mode (No import, zero latency)
* **Data Visualization:** Power BI Corporate Executive Dashboard

## 📊 Data Pipeline Architecture (Medallion)

1.  **Bronze Layer:** Raw CSV ingestion from *Our World in Data* into OneLake.
2.  **Silver Layer:** Automated ingestion schema definition and data type promotion.
3.  **Gold Layer (Synapse Warehouse):** Full **ELT** processing using optimized T-SQL queries. 
    * Implemented `DENSE_RANK()` for surrogate keys generation.
    * Filtered out data gaps and volatility prior to 1985.
    * Hard-cleansed macro-regional aggregates (e.g., OWID codes) to prevent multi-counting and secure a clean star schema.

## 📐 Data Model (Star Schema)

The model was structurally decoupled into a clean Star Schema via T-SQL to optimize DirectLake performance:
* `fact_energy`: Core metrics (Population, GDP, Renewables/Fossil Share, Carbon Intensity).
* `dim_country`: Cleaned geographical dimensions (ISO Codes, Countries, excluding aggregates).
* `dim_calendar`: Time dimension mapped post-1985.

## 📈 Key Insights & Dashboard Features

* **Executive Layout:** High-density corporate grid separating macro-KPIs, temporal evolution, and correlation matrixes.
* **Dynamic Decoupling Scatter:** Implemented a **Play Axis** on the Scatter Chart crossing `GDP per Capita` and `Carbon Intensity`, allowing stakeholders to dynamically watch countries transition towards economic growth while lowering their carbon footprint.
* **Strict Metric Logic:** Fixed visual overlaps by defining exact calculations: $\text{Low Carbon} = \text{Renewables} + \text{Nuclear}$.

## 📁 Repository Structure

* `SQL_Gold_Energy`: Production-ready T-SQL script handling the `DROP/CREATE` architecture for Dimensions and Facts.
* `Energy_Rapport.pbix`: Power BI Report containing both the presentation layer and the DirectLake semantic model configuration.
