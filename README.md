

# Real-Time Company Profiles Pipeline

## Overview

This project demonstrates a **complete data pipeline** for collecting, storing, and analyzing real-time company profile data. It uses **Finnhub API** to fetch company profiles, saves the data to a **CSV file**, uploads it to **AWS S3**, and performs **data analysis and cleaning in Databricks using PySpark**.

This pipeline is ideal for **financial data analytics**, **market research**, and **real-time data engineering practice**.

---

## Features

* Fetch company profiles from **Finnhub** API.
* Save the data to a **CSV file**.
* Upload the CSV to an **AWS S3 bucket**.
* Access and read the CSV from S3 in **Databricks** using PySpark.
* Clean and analyze data for further processing or visualization.

---

## Technologies Used

* **Python 3.x**
* **Finnhub API** – for stock market company profiles
* **boto3** – AWS SDK for Python to interact with S3
* **PySpark** – Data processing and analysis
* **AWS S3** – Cloud storage for CSV files
* **Databricks** – Notebook environment for Spark analysis

---

## Project Structure

```
.
├── fetch_profiles.py         # Script to fetch company profiles and save as CSV
├── upload_to_s3.py           # Script to upload CSV to S3
├── databricks_notebook.ipynb # PySpark notebook to read CSV from S3
├── requirements.txt          # Python dependencies
└── README.md                 # Project documentation
```

---

## Setup & Installation

### 1. Install Python Dependencies

```bash
pip install finnhub-python boto3 pyspark
```

### 2. Finnhub API

* Sign up at [Finnhub](https://finnhub.io/) to get your free API key.
* Replace the API key in `fetch_profiles.py`:

```python
finnhub_client = finnhub.Client(api_key="YOUR_API_KEY")
```

### 3. AWS S3

* Create an S3 bucket.
* Ensure your AWS IAM user has permissions to **read and write** to the bucket.
* Configure AWS credentials in your scripts.

---

## How to Run

### Step 1: Fetch Company Profiles and Save CSV

```python
python fetch_profiles.py
```

This script:

* Fetches profiles for 20 random companies from Finnhub.
* Saves the data to `company_profiles.csv`.

### Step 2: Upload CSV to S3

```python
python upload_to_s3.py
```

This script uploads `company_profiles.csv` to your specified S3 bucket.

### Step 3: Read CSV in Databricks with PySpark

Use the following in a Databricks notebook:

```python
import boto3
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("Read S3 CSV").getOrCreate()

# AWS credentials
ACCESS_KEY = "<YOUR_AWS_ACCESS_KEY>"
SECRET_KEY = "<YOUR_AWS_SECRET_KEY>"
BUCKET_NAME = "<YOUR_BUCKET_NAME>"
FILE_KEY = "company_profiles.csv"

# Download CSV locally
s3_client = boto3.client(
    "s3",
    aws_access_key_id=ACCESS_KEY,
    aws_secret_access_key=SECRET_KEY
)
local_file = "/tmp/company_profiles.csv"
s3_client.download_file(BUCKET_NAME, FILE_KEY, local_file)

# Read CSV into PySpark DataFrame
df = spark.read.csv(local_file, header=True, inferSchema=True)
df.show(5)
```

---

## Data Cleaning & Analysis

* Handle missing values with `df.fillna()` or `df.dropna()`.
* Cast columns to proper types: `df = df.withColumn("marketCap", df["marketCapitalization"].cast("double"))`
* Trim whitespace from string columns.
* Perform filtering, aggregations, and visualizations in PySpark.

---

## Notes

* **Security**: Do not hardcode AWS credentials in production. Use **Databricks Secrets** or environment variables.
* **Scaling**: For larger datasets, reading directly from `s3a://` is preferred (requires Hadoop AWS JARs).
* **Customization**: You can expand the company list or fetch additional fields from Finnhub.

---

## References

* [Finnhub API Documentation](https://finnhub.io/docs/api)
* [Boto3 Documentation](https://boto3.amazonaws.com/)
* [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)

