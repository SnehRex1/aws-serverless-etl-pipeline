# AWS Serverless ETL Pipeline using S3, Lambda and Glue

## Overview

This project demonstrates a serverless, event-driven ETL (Extract, Transform, Load) pipeline built on AWS.

The pipeline automatically processes employee and department datasets whenever new files are uploaded to Amazon S3. AWS Lambda is used as an event-driven orchestrator to trigger AWS Glue ETL jobs written in PySpark. The transformed output is then stored back in Amazon S3.

The project showcases how AWS serverless services can be combined to build scalable and cost-effective data engineering pipelines without managing infrastructure.

---

## Architecture

<img width="1108" height="607" alt="image" src="https://github.com/user-attachments/assets/9b90feee-8dd3-4b1d-a845-c6aa716e74dc" />


```
Employee.csv + Department.csv

            ↓

    Amazon S3 (Raw Bucket)

            ↓

    S3 Event Notification

            ↓

        AWS Lambda

            ↓

   AWS Glue ETL Job

            ↓

    Data Cleaning

    - Handle missing values
    - Standardize columns
    - Join datasets

            ↓

Amazon S3 (Processed Bucket)
```

---

## Tech Stack

| Service    | Purpose                            |
| ---------- | ---------------------------------- |
| AWS S3     | Store raw and processed datasets   |
| AWS Lambda | Trigger Glue ETL job automatically |
| AWS Glue   | Serverless ETL using PySpark       |
| AWS IAM    | Manage permissions                 |
| PySpark    | Data transformation and processing |
| CSV        | Input data format                  |

---

## Dataset

### Employee Dataset

File: `emp_1.csv`

Columns:

- emp_id
- name
- salary
- address
- loc
- email

### Department Dataset

File: `department.csv`

Columns:

- user
- department
- designation

---

## Workflow

### Step 1: Upload Raw Data

Employee and department CSV files are uploaded into an S3 Raw Bucket.

Example:

```
s3://raw-data-bucket/

├── emp_1.csv

└── department.csv
```

---

### Step 2: S3 Event Notification

Whenever a new file is uploaded:

- Amazon S3 generates an event.
- The event automatically triggers an AWS Lambda function.

This creates an event-driven architecture and eliminates manual intervention.

---

### Step 3: Lambda Triggers Glue Job

The Lambda function invokes the Glue ETL Job using boto3.

```python
import boto3

glueClient = boto3.client("glue")

def lambda_handler(event, context):

    glueClient.start_job_run(
        JobName="employee_department_etl"
    )

    return {
        "statusCode":200,
        "body":"Glue Job Started"
    }
```

---

### Step 4: AWS Glue ETL Processing

AWS Glue reads CSV files from S3 and performs the following transformations:

#### Data Cleaning

- Replace missing names with "Unknown"
- Replace null location values with "NA"
- Replace missing email addresses

#### Data Transformation

- Infer schema automatically
- Convert datatypes
- Rename columns if required

#### Data Joining

Employee and department datasets are joined using:

```
emp_id = user
```

---

### Step 5: Store Processed Data

The final transformed dataset is stored in:

```
s3://processed-data-bucket/
```

This processed data can be consumed by downstream analytics or reporting systems.

---

## Project Features

- Event-driven architecture
- Fully serverless implementation
- Automatic Glue job triggering
- PySpark based ETL
- Data cleaning and transformation
- Dataset joining
- Secure IAM role integration
- Scalable and cost-effective

---

## AWS Services Used

### Amazon S3

- Stores raw and processed datasets
- Provides event notifications
- Highly durable and scalable object storage

---

### AWS Lambda

- Triggered automatically by S3 events
- Starts AWS Glue ETL jobs
- Serverless and pay-per-use

---

### AWS Glue

- Serverless ETL service
- Uses Apache Spark under the hood
- Performs data cleaning and transformations
- Supports PySpark scripts

---

### AWS IAM

IAM Roles are used to:

- Allow Lambda to start Glue jobs
- Allow Glue to read/write S3 data
- Provide secure access between AWS services

---

## Folder Structure

```
aws-serverless-etl-pipeline/

│── README.md

│── data/
│   ├── emp_1.csv
│   └── department.csv

│── lambda/
│   └── lambda_trigger_glue.py

│── glue/
│   └── glue_etl_job.py

│── architecture/
│   └── architecture.png

│── screenshots/
│   ├── s3_bucket.png
│   ├── lambda_function.png
│   ├── glue_job.png
│   └── output.png
```

---

## Future Improvements

This project can be extended by:

- Adding AWS Glue Crawlers
- Integrating Amazon Athena for SQL querying
- Converting CSV to Parquet format
- Partitioning data in S3
- Adding AWS Step Functions for orchestration
- Implementing logging and monitoring using CloudWatch

---

## Resume Description

**AWS Serverless ETL Pipeline | AWS S3, Lambda, Glue, PySpark**

- Designed and implemented an event-driven ETL pipeline using Amazon S3, AWS Lambda and AWS Glue.
- Developed Glue ETL jobs using PySpark to clean, transform and join employee and department datasets.
- Automated workflow execution using S3 event notifications and Lambda triggers.
- Stored processed data in S3 and implemented IAM roles for secure communication between AWS services.

---

## Key Learnings

- Serverless Data Engineering
- Event-Driven Architecture
- AWS Glue ETL with PySpark
- S3 Event Notifications
- Lambda Orchestration
- IAM Roles and Permissions
- Data Cleaning and Transformation
- Building scalable ETL pipelines on AWS
