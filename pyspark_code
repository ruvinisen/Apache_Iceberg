Here is what you can do in iceberg

### Navigate to Spark binary directory (optional)
```bash
cd ~/spark/bin
```

### start pyspark
```bash
   pyspark 
```
### start SparkSession 
```bash
 from pyspark.sql import SparkSession
``` 
```bash
spark = SparkSession.builder \
.appName("IcebergHiveCatalog") \
.config("spark.sql.catalog.my_hive", "org.apache.iceberg.spark.SparkCatalog") \
.config("spark.sql.catalog.my_hive.type", "hive") \
.config("spark.sql.catalog.my_hive.uri", "thrift://localhost:9083") \
.config("spark.sql.catalog.my_hive.warehouse", "hdfs:///user/hive/warehouse") \
.enableHiveSupport() \
.getOrCreate()
```
>>> spark.sql("SHOW DATABASES IN my_hive").show()
+---------+
|namespace|
+---------+
|  default|
+---------+

>>> spark.sql("CREATE DATABASE my_hive.new_iceberg_db")
DataFrame[]
>>> spark.sql("SHOW DATABASES IN my_hive").show()
+--------------+
|     namespace|
+--------------+
|       default|
|new_iceberg_db|
+--------------+

>>> spark.sql("""
CREATE TABLE my_hive.new_iceberg_db.my_table_1 (
   id INT,
   name STRING,
   created_at TIMESTAMP
 )
 USING iceberg
 """)
DataFrame[]
>>> spark.sql("""
INSERT INTO my_hive.new_iceberg_db.my_table_1 VALUES
(1, 'Alice', TIMESTAMP '2025-05-20 11:00:00'),
 (2, 'Bob', TIMESTAMP '2025-05-20 11:10:00')
 """)
DataFrame[]                                                                     
>>> spark.sql("SELECT * FROM my_hive.new_iceberg_db.my_table_1").show()
+---+-----+-------------------+                                                 
| id| name|         created_at|
+---+-----+-------------------+
|  1|Alice|2025-05-20 11:00:00|
|  2|  Bob|2025-05-20 11:10:00|
+---+-----+-------------------+

>>> spark.sql("""
... CREATE TABLE my_hive.new_iceberg_db.partitioned_table (
...     id INT,
...     name STRING,
...     created_at TIMESTAMP
... )
... USING iceberg
... PARTITIONED BY (days(created_at))
... """)

>>> spark.sql("""
... INSERT INTO my_hive.new_iceberg_db.partitioned_table VALUES
... (1, 'Alice', TIMESTAMP '2025-05-20 09:00:00'),
... (2, 'Bob', TIMESTAMP '2025-05-21 10:30:00')
... """)
25/05/20 11:39:31 WARN SparkWriteBuilder: Skipping distribution/ordering: extensions are disabled and spec contains unsupported transforms
DataFrame[]                                                   
                  
>>> spark.sql("SELECT * FROM my_hive.new_iceberg_db.partitioned_table.snapshots").show()
+--------------------+-------------------+---------+---------+--------------------+--------------------+
|        committed_at|        snapshot_id|parent_id|operation|       manifest_list|             summary|
+--------------------+-------------------+---------+---------+--------------------+--------------------+
|2025-05-20 11:39:...|4198847565748285814|     null|   append|hdfs://localhost:...|{spark.app.id -> ...|
+--------------------+-------------------+---------+---------+--------------------+--------------------+

>>> spark.sql("""
... SELECT * FROM my_hive.new_iceberg_db.partitioned_table 
... VERSION AS OF 4198847565748285814
... """).show()
+---+-----+-------------------+
| id| name|         created_at|
+---+-----+-------------------+
|  1|Alice|2025-05-20 09:00:00|
|  2|  Bob|2025-05-21 10:30:00|
+---+-----+-------------------+

>>> spark.sql("""
... ALTER TABLE my_hive.new_iceberg_db.partitioned_table
... ADD COLUMN email STRING
... """)
25/05/20 11:42:24 WARN BaseTransaction: Failed to load metadata for a committed snapshot, skipping clean-up
DataFrame[]



>>> spark.sql("""
... ALTER TABLE my_hive.new_iceberg_db.partitioned_table
... DROP COLUMN email
... """)



✅ Option 1: Use SQL to Describe the Table
This shows the columns, data types, partition info, and mor
>>> spark.sql("DESCRIBE EXTENDED my_hive.new_iceberg_db.partitioned_table").show(truncate=False)
-------------------+-------+
|id                          |int                                                                                                                   |       |
|name                        |string                                                                                                                |       |
|created_at                  |timestamp                                                                                                             |       |
|                            |                                                                                                                      |       |
|# Partitioning              |                                                                                                                      |       |
|Part 0                      |days(created_at)                                                                                                      |       |
|                            |                                                                                                                      |       |
|# Metadata Columns          |                                                                                                                      |       |
|_spec_id                    |int                                                                                                                   |       |
|_partition                  |struct<created_at_day:date>                                                                                           |       |
|_file                       |string                                                                                                                |       |
|_pos                        |bigint                                                                                                                |       |
|_deleted                    |boolean                                                                                                               |       |
|                            |                                                                                                                      |       |
|# Detailed Table Information|                                                                                                                      |       |
|Name                        |my_hive.new_iceberg_db.partitioned_table                                                                              |       |
|Location                    |hdfs://localhost:9000/user/hive/warehouse/new_iceberg_db.db/partitioned_table                                         |       |
|Provider                    |iceberg                                                                                                               |       |
|Owner                       |hadoop                                                                                                                |       |
|Table Properties            |[current-snapshot-id=4198847565748285814,format=iceberg/parquet,format-version=2,write.parquet.compression-codec=zstd]|       |
+----------------------------+----------------------------------------------------------------------------------------------------------------------+-------+
This will give output including:
	Schema
	Partitioning
	Location
	Provider (iceberg)
	Table Properties (if any)


✅ Option 2: Use table_properties Metadata Table
Iceberg exposes metadata tables to inspect internals.
>>> spark.sql("SELECT * FROM my_hive.new_iceberg_db.partitioned_table.table_properties").show()
Other useful metadata tables:
	snapshots
	history
	manifests
	files
	partitions

✅ Option 3: View All Tables and Properties in the Database
>>> spark.sql("SHOW TABLES IN my_hive.new_iceberg_db").show()
+--------------+-----------------+-----------+
|     namespace|        tableName|isTemporary|
+--------------+-----------------+-----------+
|new_iceberg_db|         my_table|      false|
|new_iceberg_db|partitioned_table|      false|
+--------------+-----------------+-----------+


✅ Step 1: List all catalogs (optional)

spark.sql("SHOW CATALOGS").show()
You may see catalogs like:
	spark_catalog
	my_hive (Iceberg with Hive)
	my_catalog (Iceberg with Hadoop)

>>> spark.sql("SHOW CATALOGS").show()
+-------+
|catalog|
+-------+
|my_hive| 


✅ Step 2: For each catalog, list all databases
You can loop over and inspect them manually:
spark.sql("SHOW DATABASES IN my_hive").show()

>>> spark.sql("SHOW DATABASES IN my_hive").show()
+--------------+
|     namespace|
+--------------+
|       default|
|new_iceberg_db|
+--------------+
