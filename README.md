# WasteSort-AI-Waste-Classification 
> **One-click Azure solution that ingests COVID‑19 open data for 30 EU/EEA countries + UK, cleans & unifies > 2 million rows / day, models a star‑schema in Synapse Analytics that answers BI queries in < 3 seconds, and powers daily‑refreshed Power BI dashboards.**

---

## 1  Project Goals

1. **Ingest** five public data sets *(confirmed cases, deaths, hospital admissions, testing, population)* via parameterised Azure Data Factory (ADF) pipelines.
2. **Clean & transform** raw CSV feeds (> 2 M rows/day) with ADF Data Flows, PySpark on Databricks, and HiveQL on HDInsight – standardising indicators, pivoting metrics, and harmonising country codes.
3. **Store & model** the curated data as a star‑schema in Azure Synapse Analytics (dim Date, dim Country, fact Cases, fact Hospital, fact Tests, etc.).
4. **Visualise & share** insights through Power BI reports and dashboards that refresh **daily**.

---

## 2  Target Architecture

```text
          ┌──────────┐
  ECDC HTTP│  CSV/API │
          └──────────┘
                │
                ▼  (parameterised copy activities)
      ┌──────────────────────────┐
      │   Azure Data Factory     │
      │  (4 ingest pipelines)    │
      └──────────────────────────┘
                │
                ▼
      ┌──────────────────────────┐   PySpark   ┌──────────────────────┐
      │ Raw zone – ADLS Gen 2    │◄──────────►│ Databricks Cluster   │
      └──────────────────────────┘             └──────────────────────┘
                │ HiveQL
                ▼
      ┌──────────────────────────┐
      │ HDInsight (Hive)         │
      └──────────────────────────┘
                │
                ▼
      ┌──────────────────────────┐
      │ Curated zone – ADLS Gen 2│
      └──────────────────────────┘
                │ PolyBase
                ▼
      ┌──────────────────────────┐
      │ Azure Synapse Analytics  │
      │   (star‑schema DW)       │
      └──────────────────────────┘
                │ DirectQuery
                ▼
      ┌──────────────────────────┐
      │   Power BI Service       │
      └──────────────────────────┘
```

---

## 3  Source Data

| # | Dataset                                 | Origin                        | Update Freq  |
| - | --------------------------------------- | ----------------------------- | ------------ |
| 1 | **Confirmed Cases**                     | ECDC CSV                      | Daily        |
| 2 | **Deaths**                              | ECDC CSV                      | Daily        |
| 3 | **Hospital Admissions & ICU Occupancy** | ECDC CSV                      | Daily/Weekly |
| 4 | **Testing Volumes**                     | ECDC CSV                      | Daily        |
| 5 | **Population**                          | Azure Blob (Eurostat extract) | On demand    |

---

## 4  Data Processing Pipeline

| Stage                        | Tooling                          | Key Steps                                                |
| ---------------------------- | -------------------------------- | -------------------------------------------------------- |
| **Ingest**                   | ADF Copy, For‑Each, Get Metadata | Pull CSV → `adls://covid/raw/...`                        |
| **Transform – Cases/Deaths** | ADF Data Flows                   | Filter Europe, pivot indicator → cases/deaths, ISO codes |
| **Transform – Hospital**     | ADF Data Flows                   | Split daily vs weekly, aggregate per 100 k               |
| **Transform – Population**   | Databricks PySpark               | Clean headers, melt age buckets → parquet                |
| **Sync curated → DW**        | Synapse PolyBase                 | Load dimension & fact tables                             |
| **Quality checks**           | HDInsight Hive + Azure Monitor   | Row counts, NULL scan, schema drift alerts               |

---

## 5  Warehouse Star‑Schema (Synapse)

```text
        dimDate        dimCountry
           ▲               ▲
           │               │
     ┌─────┴───────────────┴─────┐
     │        factCases          │
     ├───────────────────────────┤
     │        factDeaths         │
     ├───────────────────────────┤
     │     factHospital          │
     ├───────────────────────────┤
     │        factTests          │
     └───────────────────────────┘
```

* Columnstore indexes enable **sub‑3‑second** median query latency (tested with 10 concurrent Power BI users).

---

## 6  Power BI Dashboards

* **EU & UK Overview 2020‑2025** – confirmed cases, deaths, tests, hospital/ICU beds.
* **Country drill‑downs** – 30 EU/EEA countries with per‑capita and 7‑day‑avg views.
* **Testing vs Positivity** scatter.
* **Refresh schedule:** incremental, daily at 04:00 UTC.

---

## 7  Tech Stack

* **Azure Data Factory** – Pipelines, Data Flows
* **Azure Data Lake Storage Gen 2**
* **Azure Databricks** – PySpark 3.5 LTS
* **Azure HDInsight** – Hive 4 on Hadoop 3
* **Azure Synapse Analytics** (formerly SQL DW)
* **Power BI Service / Desktop**
* **Python 3.11**, **SQL**, **GitHub Actions**

---

## 8  CI / CD

* **Branching:** `main` → PR → `develop` → merge triggers build.
* **Build (GitHub Actions):** lint notebooks, pytest utilities, validate ARM template.
* **Release:** deploy Synapse schema + ADF ARM to *TEST* → manual approval → *PROD*.
* **Monitoring:** Azure Monitor Alerts + Power BI Dataflow refresh notifications.



## 9  Contributors

| Role                       | Name                 | GitHub       |
| -------------------------- | -------------------- | ------------ |
| Data Engineering Lead / PM | **Nguyen Thanh Dat** | `@nthanhdat` |


