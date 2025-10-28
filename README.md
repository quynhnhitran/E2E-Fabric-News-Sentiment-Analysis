# E2E-Fabric-News-Sentiment-Analysis

## 🎯 Project Objective

The **Fabric News Sentiment Analysis** project is an end-to-end solution for real-time news sentiment analysis, built entirely on the **Microsoft Fabric** platform. This solution automatically ingests news articles from the **Bing API**, processes them, performs sentiment analysis (positive, negative), visualizes the results via **Power BI**, and triggers real-time alerts.

This solution demonstrates the capability to build modern data pipelines, utilizing a **Lakehouse** as the central hub, and deploying large-scale **Data Science** models, all within a unified, integrated environment.

<img width="1370" height="610" alt="image" src="https://github.com/user-attachments/assets/fd092196-1285-4e35-a94d-122ddd7f30a8" />
<p align="center"><i>(Workflow)</i></p>

## 🛠️ Core Technology Stack

| Component | Purpose |
| :--- | :--- |
| **Data Factory (Pipeline)** | Orchestration and ETL/ELT. Loads raw data from the Bing API. |
| **Lakehous** | Unified data storag. |
| **Synapse Data Engineering (Notebook)** | Data Transformation and **NLP (Natural Language Processing)** preprocessing. |
| **Synapse Data Science (Notebook)** | Building, training, and deploying the Sentiment Analysis model. |
| **Data Activator** | Triggers real-time alerts based on defined conditions (**Reflex Items**). |
| **Power BI (Report & Semantic Model)** | Visualization of analysis results and Self-service Analytics. |

## 🏗️ Solution Architecture & Data Flow (End-to-End Lineage)

<img width="1394" height="693" alt="image" src="https://github.com/user-attachments/assets/8b78b32f-40eb-497a-8d44-599ca289f372" />

The solution architecture follows a modern Lakehouse pattern, ensuring flexibility, scalability, and high performance. The detailed workflow is outlined below.

### Workflow Details

* **Ingestion & Staging**:
    * `news-ingestion-pipeline` calls the **Web Activity** to connect with the **Bing API**.
    * Raw data (JSON) is loaded into the `bing_lake_db` (Lakehouse), initiating the **Delta Lake (Bronze layer)** architecture.
* **Transformation (Synapse DE)**:
    * `process_bing_news` (Notebook) reads raw data from the Lakehouse, cleans it, and writes it to the **Processed/Silver** area.
* **Modeling (Synapse DS)**:
    * `new-sentiment-analysis` (Notebook) reads the clean data, applies a **Text Classification** model, and writes the final results (**Sentiment Score**) to the **Curated/Gold** area.
* **Reporting & BI**:
    * `news-dashboard-dataset` (Semantic Model) is created based on Gold data, using **Direct Lake** mode to provide high-performance querying for the `news-dashboard` (Power BI Report).
    * `bing_lake_db SQL analytics endpoint` provides the capability to query Delta tables directly using **T-SQL**.
* **Real-time Actions**:
    * Two **Activator Items** (`positive sentiment item` and `negative sentiment item`) continuously monitor new data in the Lakehouse (Gold layer) to trigger immediate alerts (**Reflex**) when critical events occur.
    * | <img width="310" height="87" alt="image" src="https://github.com/user-attachments/assets/56651088-8dd9-4735-947b-6113b5709fde" /> | <img width="460" height="416" alt="image" src="https://github.com/user-attachments/assets/b9183e0b-7898-472d-809c-49ecef2e166d" />  |
    * | <img width="301" height="89" alt="image" src="https://github.com/user-attachments/assets/19f64ba4-5447-49f9-97c1-c12b0c062478" /> | <img width="494" height="376" alt="image" src="https://github.com/user-attachments/assets/5404ddd6-238e-4435-a67c-b35e90acea36" />  |


## 💻 Workspace Resource Manifest

<img width="1833" height="848" alt="image" src="https://github.com/user-attachments/assets/8e9f35a9-6580-4067-ba58-d777548c7a29" />
<p align="center"><i>(Workspace)</i></p>

| Item Name | Type | Technical Description |
| :--- | :--- | :--- |
| `bing_lake_db` | Lakehouse | Centralized data storage, implementing **Bronze/Silver/Gold** architecture on **Delta Lake**. |
| `bing_lake_db` | SQL analytics endpoint | Allows direct **T-SQL** querying of the Lakehouse. |
| `news-ingestion-pipeline` | Pipeline | Coordinates the workflow, including the **Web Activity** API call. |
| `process_bing_news` | Notebook | Contains code for the **Transformation** step. |
| `new-sentiment-analysis` | Notebook | Contains code for the **Sentiment Analysis** step (NLP model deployment). |
| `news-dashboard-dataset` | Semantic model | Optimized data model for Power BI, using **Direct Lake**. |
| `news-dashboard` | Report | Visualization of news sentiment data. |
| `positive sentiment item` | Activator (Reflex) | Configuration for **Positive** monitoring and alerts. |
| `negative sentiment item` | Activator (Reflex) | Configuration for **Negative/Risk** monitoring and alerts. |





