
# Data Migration Pipeline on Azure Cloude Platform

This project demonstrates an end-to-end Azure data engineering solution, starting from a local SQL database and culminating in Power BI reporting, all automated.



## Business Objective

This project serves as a learning opportunity for common data engineering practices, focusing on ETL pipeline techniques. The skills sharpened here are valuable for small to medium-sized businesses aiming to migrate their local data to the cloud.

![Screenshot 2023-10-11 225825](https://github.com/somnath-2001/Azure_Data_Migration_Engineering_project/assets/118129457/2ccff973-e085-48c0-913d-fa1f650c715a)


## Current Environment

- Utilized the AdventureWorks dataset from Microsoft.
- Set up an on-premises Microsoft SQL server on a personal computer.
- Imported the dataset using Microsoft SQL Server Management Studio.
- Created a new user profile, "nik."
- Saved "nik" profile's password credentials as a Secret in Azure Key Vault.

![Screenshot 2023-10-12 011305](https://github.com/somnath-2001/Azure_Data_Migration_Engineering_project/assets/118129457/1d58dcc4-5352-47e0-914f-03e052b20bc3)



## 1: Data Ingestion

Data ingestion from the on-premises SQL server to Azure SQL is accomplished via Azure Data Factory. The process involves:

1. Installation of Self-Hosted Integration Runtime.
2. Establishing a connection between Azure Data Factory and the local SQL Server.
3. Setting up a copy pipeline to transfer all tables from the local SQL server to the Azure Data Lake's "bronze" folder.


![Azure DataFactory](https://github.com/somnath-2001/Azure_Data_Migration_Engineering_project/assets/118129457/2bbbbbf1-b257-4544-9477-22f6d2c87d73)






## 2: Data Transformation

After ingesting data into the "bronze" folder, it is transformed following the medallion data lake architecture (bronze, silver, gold). Data transitions through bronze, silver, and ultimately gold, suitable for business reporting tools like Power BI.

Azure Databricks, using PySpark, is used for these transformations. Data initially stored in parquet format in the "bronze" folder is converted to the delta format as it progresses to "silver" and "gold." This transformation is carried out through Databricks notebooks:

1. Mount the storage.
2. Transform data from "bronze" to "silver" layer.
3. Further transform data from "silver" to "gold" layer.

![sdfasd](https://github.com/somnath-2001/Azure_Data_Migration_Engineering_project/assets/118129457/6d034dd9-fd3d-4555-bb92-cf46a0a326d7)


Azure Data Factory is updated to execute the "bronze" to "silver" and "silver" to "gold" notebooks automatically with each pipeline run.

![Screenshot 2023-10-12 002249](https://github.com/somnath-2001/Azure_Data_Migration_Engineering_project/assets/118129457/e300ea40-160e-40f2-8e51-ebea9b51e7c7)



## 3: Data Loading

Data from the "gold" folder is loaded into the Business Intelligence reporting application, Power BI. Azure Synapse is used for this purpose. The steps involve:

1. Creating a link from Azure Storage (Gold Folder) to Azure Synapse.
2. Writing stored procedures to extract table information as a SQL view.
3. Storing views within a server-less SQL Database in Synapse.


![Screenshot 2023-10-12 012305](https://github.com/somnath-2001/Azure_Data_Migration_Engineering_project/assets/118129457/a3d08109-de50-4eec-a87a-7ba3933dd0be)


## 4: Data Reporting

Power BI connects directly to the cloud pipeline using DirectQuery to dynamically update the database. A Power BI report is developed to visualize AdventureWorks dataset data, including sales, product information, and customer gender.

![gif](https://github.com/somnath-2001/Azure_Data_Migration_Engineering_project/assets/118129457/da9e0206-19c8-4623-983d-2b0b1a6935bf)



## 5: Final Pipeline Test

To verify the end-to-end pipeline, two new customers are added to the local SQL database server. If successful, the pipeline will update, and the Power BI report will dynamically show the new data. The total number of customers should increase from 847 to 849.

![Screenshot 2023-10-12 013527](https://github.com/somnath-2001/Azure_Data_Migration_Engineering_project/assets/118129457/9ab6a58c-9bb6-42d2-98cd-1164f1a4bcf2)

Great success!


## Conclusion and Limitations

This project demonstrates the ability to create an end-to-end ETL cloud solution using Azure. Some considerations:

- The dataset used was small (7mb total, 800 rows). This was done to keep compute + storage costs low for myself.
- Multiple applications were employed for a relatively simple task.
- Given the dataset's simplicity, the project could have been managed entirely through Azure Data Factory, with data cleaning done downstream in Power BI.
- The inclusion of Azure Synapse and Databricks was for the sake of self-learning and emulating real-world business pipelines.
