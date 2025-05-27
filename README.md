# Apache_Iceberg 
Apache Iceberg is an open-source, high-performance table format designed to manage large-scale analytic datasets in data lakes. Originally developed by Netflix and Apple, it addresses limitations found in older systems like Apache Hive, particularly concerning data consistency, scalability, and transactional integrity. 

## Key Features of Apache Iceberg
### ACID Transactions: 
Ensures atomicity, consistency, isolation, and durability, allowing multiple engines to read and write to the same table concurrently without data corruption.
### Schema Evolution: 
Supports adding, renaming, or reordering columns without the need to rewrite the entire dataset, facilitating adaptability to changing data requirements. 
### Time Travel and Rollback: 
Maintains a history of table snapshots, enabling users to query previous versions of data or revert to earlier states as needed. 
### Hidden Partitioning: 
Automatically manages partitioning, optimizing query performance without requiring manual partition definitions. 
### Broad Engine Compatibility: 
Integrates seamlessly with various data processing engines, including Apache Spark, Flink, Hive, Trino, and Presto, allowing for flexible and efficient data operations. 

## Use Cases
Apache Iceberg is particularly beneficial for organizations that:
Require robust data governance and compliance features, such as GDPR adherence.
Need to perform incremental data processing or manage slowly changing dimensions.
Operate in environments with multiple data processing engines accessing shared datasets.
Seek to implement data versioning and rollback capabilities for auditing and error correction. 


# Apache Iceberg Spark Integration Setup

This guide details how to set up the Apache Iceberg runtime JAR for use with Apache Spark. 

# Apache Iceberg with PySpark and Hive Metastore

This guide demonstrates how to set up and use Apache Iceberg with PySpark and Hive Metastore, including:

- Catalog configuration
- Table creation
- Partitioning
- Time travel
- Schema evolution
- Metadata inspection

## Prerequisites

- Apache Spark installed in `/home/hadoop/spark`
- Internet access to download dependencies
- User with appropriate permissions (e.g., `hadoop`)

## Installation Steps

Open a terminal and run the following commands:

### Step 1: Download the Iceberg Spark Runtime JAR
```bash
wget --no-check-certificate https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-spark-runtime-3.3_2.12/1.4.2/iceberg-spark-runtime-3.3_2.12-1.4.2.jar
``` 

### Step 2: Move the JAR file to Spark's JARs directory
```bash
mv iceberg-spark-runtime-3.3_2.12-1.4.2.jar /home/hadoop/spark/jars/
```

### Step 3: Set the correct file permissions
```bash
chmod 644 /home/hadoop/spark/jars/iceberg-spark-runtime-3.3_2.12-1.4.2.jar
```

### Step 4: Navigate to Spark binary directory (optional)
```bash
cd ~/spark/bin
```







