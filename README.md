# 💼 Azure Data Pipeline – Retail Customer Transactions

This project showcases an **Azure Data Factory pipeline** that performs **event-triggered ingestion**, **row-level data validation**, **SQL database loading**, and **error handling** using cloud-native services.

---

## 📌 Project Summary

A CSV file containing customer transactions is uploaded to **Azure Blob Storage**, triggering an ADF pipeline. The pipeline uses a **Mapping Data Flow** to validate rows, send valid records to **Azure SQL Database**, send invalid records to **Blob error folder**, and archive the source file.

---

## 🚀 Key Features

- ✅ Event-based trigger when `.csv` is uploaded
- 🔍 Row-level validation using ADF **Conditional Split**:
  - Email must not be null
  - Amount must be > 0
  - Created date must be present
- 📥 Valid data → stored in `Azure SQL`
- 📤 Invalid data → stored in `error-data/` folder with timestamped file name
- 📦 Source file → archived in `processed-data/`

---

## 🧱 Tech Stack

| Component              | Usage                            |
|------------------------|----------------------------------|
| Azure Data Factory     | Orchestration & Data Flow        |
| Azure Blob Storage     | Source, Error, Archive storage   |
| Azure SQL Database     | Final staging table              |
| Azure Event Grid       | Trigger pipeline on blob upload  |
| Data Flow Parameters   | Used for dynamic file naming     |

---

## 📁 Folder Structure
azure-data-pipeline-retail-validation/
├── data/ # Sample CSV files
├── sql/ # SQL script to create table
├── pipeline-screenshots/ # ADF screenshots
├── adf_export/ # (Optional) JSON/ARM export
├── README.md


---

## 📄 SQL Table (Target Schema)

```sql
CREATE TABLE stg_customer_transactions (
  customer_id VARCHAR(20),
  email VARCHAR(100),
  amount FLOAT,
  created_date DATE
);

📂 Sample File Paths
Type	Path
Input CSV	raw-data/customer_transactions.csv
Valid Output	Azure SQL Table: stg_customer_transactions
Invalid Output	error-data/invalid_rows_YYYYMMDD_HHMMSS.csv
Archived File	processed-data/customer_transactions_YYYYMMDD.csv

🧪 Sample Data

customer_id,email,amount,created_date
C101,neha.sharma@mail.com,1250,2024-06-01
C102,arjun.rao@mail.com,900,2024-06-02
C103,,0,2024-06-03
C104,,300,2024-06-04
C105,rahul.patel@mail.com,750,


🛠 How It Works
Upload CSV to raw-data container

Event trigger starts ADF pipeline

Data Flow:

Valid rows go to SinkSQL

Invalid rows go to SinkErrorBlob

File archived to processed-data/

Monitor pipeline under Monitor > Pipeline Runs


🔄 Possible Enhancements

Add schema drift handling

Parameterize container names and paths

Add logging or email notifications

Build Power BI dashboard on top of SQL data

👤 Author
Priya Banerjee
Data Engineer | Azure | Power BI | SQL

