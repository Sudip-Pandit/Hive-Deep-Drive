# Hive-Deep-Drive
This repository contains all concepts and commands related to hive.
### Hive Tables
1)External table

2)Internal Table

### External table stores data not in the /user/hive/warehouse/ directory but internal table stores data in /user/hive/warehouse directory.
### It is under control of hive .i.e. once the table is dropped both the schema and data in the table get dropped.
### But, when droping the external table, only the schema get dropped but data remains exist.
### Internal tables are called managed tables.
### We need to mention the external keyword while creating the external table.
### Data is stored in the system defined /user/hive/warehouse/ directory. 

# How to create table in Hive?
CREATE TABLE hiveFirstTable(employee_id INT, first_name STRING, last_name STRING, hire_date STRING, salary INT, email STRING, phone_number STRING)

ROW FORMAT delimited 

FIELDS TERMINATED BY ','

LINES TERMINATED BY '\n'

STORED AS textfile;

# How to create external table in Hive?
CREATE EXTERNAL TABLE table_name(order_id INT, order_date STRING, order_status STRING)

ROW FORMAT delimited 

FIELDS TERMINATED BY ',' 

LINES TERMINATED BY '\n'

STORED AS textfile
LOCATION '/user/<>/<directory>';
  
# How to describe the hive table attributes?
# ROW FORMAT:
###### It describe the format of the data file that will be loaded to Hive. There are two possible types: DELIMITED, SERDE

# TBLPROPERTIES:
###### The TBLPROPERTIES clause allows you to tag the table definition with your own metadata key/value pairs.
###### Example: Ignoring 'n' number of lines of header or footer from file to be loaded

# STORED AS:
###### It specifys the format of the data file. 
###### The default text file is plain, other different types are ORC, PARQUET, RCFILE, AVRO, JSONFILE, SEQUENCEFILE

# Partition By:
###### This clasue is used to partition the data into multiple files 
###### It is good in terms of performance tunning
###### Table data is divided into multiple parts based on the low cardinality column ###### to increase the performance of the hive
###### Partitions are slices of data which helps to manage the data into more 
###### managerable chunks.
###### Partition column is generally taken from the common columns that are frequently used for queries having low cardinality columns. 
###### Basically columns such as date, state, country and departments are taken as a ###### partition column.
###### It is a good way of dividing a table into parts based on the value of the partitioned attribute of the column.
###### Each partition can have its own columns, serDe and storage info
###### Partition determine how the data is stored in a manageable way to increase the ###### performance of the hive
###### Hive creates directorty for each value of partitioned column
###### Each partitioned directory stores data containing only partitioned value as specified by directory/folder name
###### Hive creates seperate directory for each value of partitioned column

# Clustered by:
###### Bucketing is a concept to clasify the data into more manageable parts to increase the performance of the hive.
###### Table data is classified into n number of buckets using hash function.
###### Bucketing is based on the value of the hash function of the particular column ###### of a table.
###### Bucketing is very seful for sampling and join optimization
###### Hash partition within ranges
###### For sub-sampling within a partition
###### It can speed up queries that involve sampling the data (Without bucketing hive ###### scans entire datasets)
###### Hive creates files for each value of bucket
  
# Command regarding partitioned column

Create table hiveFirstPartitionedTable
(  
             employee)id INT,
  
             hire_date STRING,
  
             department_id INT
)
PARTITIONED BY (department_id INT)
  
Row format delimited 
  
fields terminated by  ','
  
Stored as textfile;


  
  
