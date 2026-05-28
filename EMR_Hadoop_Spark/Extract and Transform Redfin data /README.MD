
## Project Name:
Real Estate Big Data Processing Pipeline using Amazon EMR, PySpark, and S3

------------------------------------------------------------

PROJECT OVERVIEW

In this project, we built a Big Data ETL pipeline using AWS services.

The main goal was to extract large real estate datasets from the Redfin data source, process the data using PySpark on Amazon EMR, and store both raw and transformed data in Amazon S3 for analytics and reporting.

We used distributed computing because the dataset contained millions of records and processing such huge data on a single server would be slow and inefficient.

------------------------------------------------------------

TECHNOLOGIES USED

- AWS EMR
- Apache Spark
- PySpark
- Amazon S3
- IAM
- VPC
- Jupyter Notebook
- Redfin Dataset

------------------------------------------------------------

ARCHITECTURE FLOW

Redfin Data Source
        ↓
Amazon EMR Cluster
        ↓
Raw Data Stored in S3
        ↓
PySpark Transformations
        ↓
Processed Data Stored in S3
        ↓
Analytics / Reporting

------------------------------------------------------------

WHAT IS AMAZON EMR?

Amazon EMR stands for Elastic MapReduce.

It is a managed Big Data platform provided by AWS that comes with pre-installed frameworks like:
- Apache Spark
- Hadoop
- Hive
- Presto

Instead of manually setting up Hadoop clusters and maintaining servers, AWS manages:
- Cluster provisioning
- Scaling
- Monitoring
- Failure recovery

This reduces operational overhead and improves scalability.

------------------------------------------------------------

EMR CLUSTER ARCHITECTURE

1. Master Node
The master node manages the entire cluster.

Responsibilities:
- Resource management
- Task scheduling
- Monitoring node health

Only one master node exists in a cluster.

------------------------------------------------------------

2. Core Nodes
Core nodes perform:
- Data processing
- Data storage using HDFS

Multiple core nodes can exist.

------------------------------------------------------------

3. Task Nodes
Task nodes only process tasks.

They do not store data.

These are optional nodes used for additional processing power.

------------------------------------------------------------

STEP-BY-STEP PROJECT IMPLEMENTATION

STEP 1 – CREATE IAM USER

First, we created an IAM user instead of using the root account.

Reason:
- Better security
- Controlled access
- Industry best practice

We provided:
- Console access
- Programmatic access
- Administrator permissions for the demo

We also generated:
- Access Key
- Secret Access Key

------------------------------------------------------------

STEP 2 – CREATE S3 BUCKETS

We created multiple S3 buckets.

1. Raw Bucket
Used to store raw Redfin data.

2. Processed Bucket
Used to store transformed data.

3. Workspace Bucket
Used to store Jupyter notebooks.

4. EMR Logs Bucket
Used to store cluster logs.

This follows a proper data lake architecture.

------------------------------------------------------------

STEP 3 – CREATE VPC

We created a custom VPC for secure networking.

Configured:
- Public subnets
- Route tables
- Security groups

Purpose:
- Secure communication
- Network isolation
- Controlled access

------------------------------------------------------------

STEP 4 – PROVISION EMR CLUSTER

Next, we created the EMR cluster.

Installed Applications:
- Apache Spark
- Jupyter Enterprise Gateway

Spark was used for distributed data processing.

Jupyter Enterprise Gateway allowed notebook integration with EMR.

------------------------------------------------------------

CLUSTER CONFIGURATION

Master Node:
- 1 node

Core Nodes:
- 2 nodes initially

Auto Scaling:
Enabled EMR managed scaling.

Benefits:
- Automatically scale up during high workloads
- Scale down during low workloads
- Cost optimization

------------------------------------------------------------

STEP 5 – CONFIGURE SECURITY

Configured:
- EC2 Key Pair
- IAM Service Roles
- Security Groups
- Instance Profiles

Purpose:
- Secure cluster access
- Allow EMR to access S3
- Enable notebook connectivity

------------------------------------------------------------

STEP 6 – CREATE EMR STUDIO

We created EMR Studio.

Purpose:
- Web-based development environment
- Notebook management
- Cluster integration

Inside EMR Studio, we created a workspace which acts as a Jupyter Notebook environment.

------------------------------------------------------------

STEP 7 – ATTACH NOTEBOOK TO EMR CLUSTER

We attached the Jupyter Notebook workspace to the EMR cluster.

Selected:
- EMR on EC2
- Existing runtime cluster

Now the notebook could execute Spark jobs directly on the EMR cluster.

------------------------------------------------------------

STEP 8 – INITIALIZE SPARK SESSION

Inside the notebook, we initialized SparkSession.

from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("Redfin Data Analysis") \
    .getOrCreate()

Purpose:
- Start Spark engine
- Create distributed processing environment

------------------------------------------------------------

STEP 9 – EXTRACT DATA FROM REDFIN

We downloaded the dataset from Redfin using Bash commands.

wget -O - <redfin-url> | aws s3 cp - s3://raw-bucket/file.gz

Purpose:
- Download large dataset
- Push directly into S3 raw layer

Dataset size was around 1.1 GB.

------------------------------------------------------------

STEP 10 – READ DATA USING PYSPARK

Loaded the data from S3 into Spark DataFrame.

df = spark.read.csv(
    "s3://bucket/file",
    header=True,
    inferSchema=True,
    sep="\t"
)

Purpose:
- Parallel data reading
- Schema inference
- Efficient processing

------------------------------------------------------------

STEP 11 – DATA EXPLORATION

Performed:
- Schema validation
- Row counts
- Column analysis
- Null value checks

Example:

df.printSchema()
df.count()

Dataset contained approximately 4.9 million rows.

------------------------------------------------------------

STEP 12 – DATA CLEANING

Removed null values.

df = df.na.drop()

After cleaning:
- Rows reduced from 4.9 million
- To 4.4 million

Purpose:
- Improve data quality
- Prepare for analytics

------------------------------------------------------------

STEP 13 – DATA TRANSFORMATION

Selected required columns only.

Created:
- Year column
- Month column

Example:

from pyspark.sql.functions import year, month

df = df.withColumn(
    "year",
    year("period_end")
)

We also converted month numbers:
- 1 → January
- 2 → February

Purpose:
- Better readability
- Easier reporting

------------------------------------------------------------

STEP 14 – STORE PROCESSED DATA

Finally, transformed data was saved back into the processed S3 bucket.

Purpose:
- Analytics-ready storage
- Data lake processed layer
- Future reporting and ML usage

------------------------------------------------------------

CHALLENGES FACED

1. EMR Permission Issues
Issue:
EMR Studio could not access S3 bucket.

Solution:
Attached:
- S3 Full Access
- IAM roles
- Service permissions

------------------------------------------------------------

2. Cluster Startup Delay
EMR provisioning took around 10–12 minutes.

Reason:
- Node provisioning
- Spark installation
- Runtime initialization

------------------------------------------------------------

3. Notebook Connectivity
Issue:
Workspace popup blocked.

Solution:
Enabled browser popups.

------------------------------------------------------------

KEY LEARNINGS

This project helped me understand:
- Distributed computing
- Spark architecture
- Cloud ETL pipelines
- EMR cluster management
- PySpark transformations
- Big Data processing
- Auto scaling in EMR

------------------------------------------------------------

SHORT INTERVIEW SUMMARY

“I worked on a Big Data ETL pipeline using Amazon EMR and PySpark. We extracted large-scale Redfin real estate data, stored raw data in S3, processed it using distributed Spark clusters on EMR, performed transformations and null handling, then saved cleaned data back into S3 for analytics and reporting. We used EMR Studio with Jupyter notebooks for development and enabled auto-scaling for performance and cost optimization.”

------------------------------------------------------------

WHY EMR INSTEAD OF EC2?

Because EMR provides:
- Managed Hadoop/Spark
- Auto scaling
- Distributed processing
- Fault tolerance
- Easier maintenance

------------------------------------------------------------

WHY PYSPARK?

Because PySpark supports:
- Parallel processing
- Large-scale data handling
- Fast transformations
- EMR integration

------------------------------------------------------------

WHY S3?

Because S3 provides:
- Durable storage
- Cheap storage
- Scalability
- Easy integration with EMR

------------------------------------------------------------
