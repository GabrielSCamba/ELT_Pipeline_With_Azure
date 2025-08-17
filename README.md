
# ELT Pipeline with Azure Data Factory & Databricks  

![Azure Data Factory](https://img.shields.io/badge/Azure%20Data%20Factory-0078D4?logo=microsoftazure&logoColor=white)
![Azure Synapse](https://img.shields.io/badge/Azure%20Synapse-blue?logo=azure)
![Azure Data Lake](https://img.shields.io/badge/Azure%20Data%20Lake-blue?logo=azure)
![Azure Key Vault](https://img.shields.io/badge/Azure%20Key%20Vault-yellow?logo=azure)
![Databricks](https://img.shields.io/badge/Databricks-FF3621?logo=databricks&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?logo=apachespark&logoColor=white)



This repository contains the artifacts of my **Data Engineering project**, where I implemented an **ELT (Extract, Load, Transform)** pipeline to ingest data from an API using:
 

- **Azure Data Factory (ADF)** → Orchestrates data pipelines  
- **Azure Databricks (PySpark)** → Performs data transformations  
- **Azure Synapse** → Serves as the analytics warehouse  
- **Azure Data Lake Storage Gen2** → Stores data in **Bronze**, **Silver**, and **Gold** layers

---

## 📂 Repository Structure


├─ data-factory
│  ├─ dataset
│  │  ├─ ds_datalake_historical.json
│  │  ├─ ds_datalake_profile.json
│  │  ├─ ds_fmp_historical.json
│  │  ├─ ds_fmp_profile.json
│  │  ├─ ds_gold_dim_company.json
│  │  ├─ ds_gold_fact_quote.json
│  │  ├─ ds_synapse_dim_company.json
│  │  └─ ds_synapse_fact_quote.json
│  ├─ factory
│  │  └─ df-elt-project.json
│  ├─ linkedServices
│  │  ├─ ls_databricks_ws.json
│  │  ├─ ls_datalake_gold.json
│  │  ├─ ls_datalake_raw.json
│  │  ├─ ls_fmp_api.json
│  │  ├─ ls_keyvault.json
│  │  └─ ls_synapse_dpool.json
│  ├─ pipeline
│  │  └─ publish_config.json 
├─ databricks
│  │  ├─ exploration.ipynb
│  │  └─ analysis.ipynb
├─ docs
└─ readme.md

---

## 📝 Project Summary

- **Extract**: Ingest raw data into the **Bronze** layer of the Data Lake.  
- **Load**: Orchestrate data movement across layers using **Azure Data Factory**.  
- **Transform**: Process data in **Azure Databricks**, generating **Silver** (cleaned) and **Gold** (analytics-ready) datasets.  
- **Data Quality**: Ensure data validation and consistency with **Azure Databricks**.  
- **Data Consumption**: Store processed data in **Azure Synapse** for downstream consumption.

---

## ⚙️ Tech Stack
- Azure Data Factory  
- Azure Databricks (PySpark)  
- Azure Synapse
- Data Lake (Azure Data Lake Storage Gen2)
- Key Vault

---

## 📖 Full Article
A detailed explanation of each step of the project is available in the LinkedIn article:  
👉 [Read the full article on LinkedIn](https://www.linkedin.com/in/GabrielSCamba) *(replace with the actual link once published)*  

---

## Author

- [GabrielSCamba](https://github.com/GabrielSCamba)
