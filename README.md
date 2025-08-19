
# ELT Pipeline with Azure Data Factory & Databricks  

![Azure Data Factory](https://img.shields.io/badge/Azure%20Data%20Factory-0078D4?logo=microsoftazure&logoColor=white)
![Azure Synapse](https://img.shields.io/badge/Azure%20Synapse-blue?logo=azure)
![Azure Data Lake](https://img.shields.io/badge/Azure%20Data%20Lake-blue?logo=azure)
![Azure Key Vault](https://img.shields.io/badge/Azure%20Key%20Vault-yellow?logo=azure)
![Databricks](https://img.shields.io/badge/Databricks-FF3621?logo=databricks&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?logo=apachespark&logoColor=white)


## Introduction
This Data Engineering project consists of developing a data pipeline that consumes raw data from an API, applies transformations and standardization, and makes the data available for analytical consumption.  
It leverages Microsoft Azure services and follows best development practices.

---

## Objectives
This project has two main objectives:

- **Personal Goal**: put into practice data engineering knowledge, document each step of the implementation, and share the results with the community.  
- **Final Delivery Goal**: deliver clean and standardized data into a Data Warehouse ready for consumption and analysis.

---

## Project Architecture Overview
![Project Architecture][![Arquitetura do Projeto](docs/project_architecture.png)]

---

---

## 📂 Repository Structure

```
├─ 📂 data-factory
│  ├─ 📂 dataset
│  │  ├─ ds_datalake_historical.json
│  │  ├─ ds_datalake_profile.json
│  │  ├─ ds_fmp_historical.json
│  │  ├─ ds_fmp_profile.json
│  │  ├─ ds_gold_dim_company.json
│  │  ├─ ds_gold_fact_quote.json
│  │  ├─ ds_synapse_dim_company.json
│  │  └─ ds_synapse_fact_quote.json
│  ├─ 📂 factory
│  │  └─ df-elt-project.json
│  ├─ 📂 linkedServices
│  │  ├─ ls_databricks_ws.json
│  │  ├─ ls_datalake_gold.json
│  │  ├─ ls_datalake_raw.json
│  │  ├─ ls_fmp_api.json
│  │  ├─ ls_keyvault.json
│  │  └─ ls_synapse_dpool.json
│  ├─ 📂 pipeline
│  │  └─ publish_config.json 
├─ 📂 databricks
│  │  ├─ exploration.ipynb
│  │  └─ analysis.ipynb
├─ 📂 docs
└─ readme.md
```
---

## Tools Used
- **Azure Data Factory** → Data orchestration  
- **Azure Databricks (PySpark)** → Data transformations  
- **Azure Synapse** → Analytic Warehouse  
- **Azure Data Lake Storage Gen2** → Storage (Bronze, Silver, Gold layers)  
- **Azure Key Vault** → Secrets management  

---

## Step-by-Step Implementation

### 1. Define an API for the project
The first step was to choose an API to consume data from.  
I selected the **Financial Modeling Prep (FMP)** API because its **Basic Plan** is free and meets the project needs (historical stock price data). 

![API plans](docs/api-plans.png)

After reading the documentation and understanding its endpoints, I registered on the website to obtain an **API Key**.  

---

### 2. Create Azure Resources
Inside Microsoft Azure, the necessary resources were created within a Resource Group.  
This Resource Group served as a container for all services used in the project.By grouping the resources together, it was easier to manage access, monitor costs, and keep the environment organized.

![Resource Group](docs/resource.png)


#### Storage Account (Data Lake)
Data is stored in an **Azure Data Lake Storage Gen2** using the **medallion architecture**:
![Storage layers](docs/layers-overview.png)
- **Bronze**: raw data directly from the source. 
 ![Bronze folders](docs/bronze-folders.png)
- **Silver**: cleaned and standardized data (duplicates removed, column names standardized, column types validated).  
![Silver folders](docs/silver-folders.png)
- **Gold**: enriched data with business metrics. 
![Gold folders](docs/gold-folders.png)

Additionally, a **Staging Layer** was created to move data from the Gold layer into **Azure Synapse** using **Databricks**.
![Project Architecture](docs/create_table-dim_company.png)

#### Synapse
- A **Dedicated SQL Pool** was created.  
- Tables and a **stored procedure** were implemented to populate the `dim_date` table.  
![Synapse tables](docs/synapse-tables.png)

#### Key Vault
For data security, **Azure Key Vault** was used to securely store sensitive information, such as the API Key and Synapse credentials.  
![Key Vault secrets](docs/keys.png)

#### Databricks
- A low-cost cluster was created (since this is a personal project).  
![Databricks cluster](docs/details-cluster.png)
- Databricks was used to process and move data across the Bronze → Silver → Gold → Synapse layers.  
- To connect Databricks with the Data Lake, credentials were stored securely using **Databricks Secret Scope**, following [this documentation](https://learn.microsoft.com/en-us/azure/databricks/security/secrets/example-secret-workflow).  

Developed notebooks:
- `Bronze_To_Silver.py`: Data exploration, standardization, and cleaning.  
- `Silver_To_Gold.py`: Data enrichment.  
- `Validation_Gold.py`: Validation to ensure Gold data consistency.  
- `Copy_To_Synapse_dim_company.py`: Moves `dim_company` from Gold to Synapse.  
- `Copy_To_Synapse_fact_quote.py`: Moves `fact_quote` from Gold to Synapse.  

---

### 3. Data Factory (Orchestration) <img src="docs/logos/adf-logo.jpeg" alt="Data Factory logo" width="20"/>
Azure Data Factory was used to orchestrate the pipeline, ensuring automation and monitoring.

Steps:
1. **Linked Services** → Connections to external data sources and compute services. They define *how* Data Factory connects to the resources.  
![Data Factory - Linked Services](docs/df-linked_services.png)
2. **Datasets** → Representations of data structures within the linked data stores. They point to specific files, tables, or folders and are used as inputs and outputs in Data Factory activities.  
![Data Factory - Datasets](docs/df-datasets.png)
3. **Pipeline Flow** → Orchestrated sequence of activities:  
   - Retrieve API Key from Key Vault  
   - Ingest API data into the Bronze layer  
   - Process Bronze → Silver with Databricks  
   - Process Silver → Gold with Databricks  
   - Validate Gold data with Databricks  
   - Execute stored procedure for `dim_date`  
   - Ingest Gold data into Synapse with Databricks  

![Data Factory - Pipeline](docs/pipeline.png)

---
## Results
The project successfully delivers **clean and analysis-ready data** stored in **Azure Synapse**.  
This allows quick, reliable, and business-oriented analytics.  

The images below demonstrate the results obtained, with the tables properly populated in Synapse:

![Table dim_date](docs/table-dim_date.png) 
![Table dim_company](docs/table-dim_company.png)
![Table fact_quote](docs/table-fact_quote.png)

---

## Conclusion
Both goals were achieved:  
- Raw data was collected, processed, and stored in a Data Warehouse.  
- Knowledge gained from studies, documentation, tutorials, and videos was applied and documented.  

This project also serves as a **knowledge-sharing resource** for the Data Engineering community.  

---

## Next Steps
To improve this project, possible extensions include:
- Connecting Synapse to **Power BI** for interactive reports.  
- Exploring logging and monitoring tools for pipeline observability.  
- Adding new data sources.  

---

## References

- [Azure Databricks Documentation](https://learn.microsoft.com/pt-br/azure/databricks/) 
- [Azure Data Factory Documentation](https://learn.microsoft.com/en-us/azure/data-factory/)  
- [Azure Data Factory - Use Key Vault](https://learn.microsoft.com/en-us/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities)  
- [Azure Data Lake Storage Documentation](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction)  
- [Azure Synapse Documentation](https://learn.microsoft.com/en-us/azure/synapse-analytics/)  

- [Azure Key Vault Documentation](https://learn.microsoft.com/en-us/azure/key-vault/)  
- [Databricks Secrets Documentation](https://docs.databricks.com/aws/en/security/secrets/example-secret-workflow)  
- [DP-700T00 Microsoft Training](https://learn.microsoft.com/en-us/training/courses/dp-700t00)  
- [Financial Modeling Prep API](https://site.financialmodelingprep.com/developer/docs/stable)  


## Author

- [GabrielSCamba](https://github.com/GabrielSCamba)
