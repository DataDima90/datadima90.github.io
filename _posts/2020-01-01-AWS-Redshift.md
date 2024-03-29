---
layout: post
title: AWS Redshift
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Redshift, AWS]
mathjax: true
summary: AWS Redshift Overview
---

# Redshift

- Fully-managed, petabyte-scale data warehouse service for analyzing data using BI tools
- Entreprise-class data warehouse and relational database query and management system
- Connect using many types of client applications
  - BI
  - Reporting
  - Analytics
- Build multi-stage query operations that retrieve, compare, and evaluate large amounts of data
- Efficient storage and optiimum query performance
  - Massively parallel processing
  - Columnar data storage
  - Very efficient, targeted data compression encoding schemes

**Usage patterns**
- Designed for online analytical processing (OLAP) using business intelligence tools
- Analyze sales data for multiple products
- Analyze ad impressions and clicks
- Aggregate gaming data
- Analyze social trends

**Cost**
- Charges based on the size and number of cluster nodes
- Backup sotrage > provisioned storage size and backups stored after cluster termiantion billed at standard S3 rate

**Performance**
- Columnar storage, data compression, and zone maps to reduce query I/O
- Paralllizes and distributes SQL operations to take advantage of all available resources

- Automatically detects and replaces a failed node in your data warehouse cluster

**Durability and availability**
- Failed node cluster is read-only until replacement node is provisioned and added to the DB
- Cluster remains available on drive failure; Redshift mirrors your data across the cluster

**Scalability and elasticity**
- With API change the number, or type, of nodes while cluster remains live

**Interfaces**
- JDBC and ODBC drivers for use with SQL clients
- S3, DynamoDB, Kinesis, BI tools such as QuickSight

**Anti-patterns**
- Small data sets, better big data sets
- OLTP, better OLAP
- Unstructured data
- Blob data, S3 better choice

**Analytics Solutions Patterns with Redshift**

OLAP using BI tools
- Near real-time analysis of millions of rows of manufacturing data generated by continous manufacturing equipment
- Analyze events from mobile app to gain insight into how users use the application
- Gain value and insights from large, complex, and dispersed datasets
- Make live data generated by range of next-gen security solutions available to large numbers of organizations for analysis

![]({{ site.url }}/assets/img/posts/redshiftpattern.png)

**Redshift Architecture**
- Based on PostgreSQL
- Clients connect via JDBC and ODBC
- Built upon clusters
  - One or more compute nodes
  - If greater than 1 compute nodes, a leader node coordinates the compute nodes and communicates with external client apps
- Leader node
  - Build execution plans to execute database operations, such as complex queries
  - Compiles code and distributes it to the compute nodes, also assigns a portion of the data to each compute node
- Compute nodes
  - Executes the compiled code and sends intermediate results back to the leader node for final aggregation
  - Has dedicated CPU, memory, and attached disk storage, which are determined by the node type - node types: RA3, DC2, DS2
  - User data is stored on compute nodes
- Node slices
  - Compute node are partitioned into slices
  - Slices are allocated a portiion of the node's memory and disk space
  - Processes a part of the workload assigned to the node
  - Leader node distributes data to the slices, divides query workload to the slices
  - Slices work in parallel to complete our queries
  - Assign a column as a distribution key when we create our table on Redshift
  - When we load data into our table, rows are distributed to the node slices by the table distribution key - facilitates parallel processing

**Moving data to and from Redshift**
- When using the COPY command to load data files into Redshift, Apache Parquet and ORC are better choices than JSON or CSV because both are columnar data formats that allow you to copy your data more efficiently and cost-effectively into Redshift.
- You should use the Redshift COPY command to load the data into Redshift.
- You should use the Redshift UNLOAD command to retrieve data from Redshift.
- Redshift integrates well with AWS servies to move, transform, and load our data quickly and reliably
  - S3
  - DynamoDB
  - EMR via SSH and COPY Command from Redshift
  - EC2 via SSH and COPY Command from Redshift
  - Data Pipeline
  - DMS
