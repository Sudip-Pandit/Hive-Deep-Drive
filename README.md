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
CREATE TABLE hiveFirstTable
(  
   employee_id INT,
   first_name STRING,
   last_name STRING,
   hire_date STRING,
   salary INT,
   email STRING,
   phone_number STRING
)
ROW FORMAT delimited 
FIELDS TERMINATED BY ',' 
LINES TERMINATED BY '\n'
STORED AS textfile;

# How to create external table in Hive?
CREATE EXTERNAL TABLE table_name
(  
   order_id INT,
   order_date STRING,
   order_status STRING
)
ROW FORMAT delimited 
FIELDS TERMINATED BY ',' 
LINES TERMINATED BY '\n'
STORED AS textfile
LOCATION '/user/<>/<directory>';
