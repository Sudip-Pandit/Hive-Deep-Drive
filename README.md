# Hive-Deep-Drive
This repository contains all concepts and commands related to hive.
# Hive Tables

     1)External table

     2)Internal Table
     
     If you drop managed table, the backened directory get deleted.
     If you drop external table, the backened directory is not deleted.

# External table
     
 1) It stores data not in the /user/hive/warehouse/ directory but internal table stores data in /user/hive/warehouse directory.
 2) It is under control of hive .i.e. once the table is dropped both the schema and data in the table get dropped.
 3) But, when droping the external table, only the schema get dropped but data remains exist.
 4) Internal tables are called managed tables.
 5) We need to mention the external keyword while creating the external table.
 6) Data is stored in the system defined /user/hive/warehouse/ directory. 
 
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
  
# Simple Project start

  ##### Go to the edge node

  hadoop fs -mkdir /user/cloudera/data1
  hadoop fs -put /home/cloudera/data/txnsd.txt /user/cloudera/data1

  ##### Go to the hive shell 
  ##### Hive

  ###### create database demo;
  
  use demo;

  create table t_data1(id int,name string,category string) row format delimited fields terminated by ',' location '/user/cloudera/t_data1';

  ##### select * from t_data1;

  create table t_data2(id int,name string,category string) row format delimited fields terminated by ',';

  insert into t_data2 select * from t_data1 where category='Gymnastics';

  ##### select * from t_data2;

  ![image](https://user-images.githubusercontent.com/70854976/149632415-46376463-a9f2-445e-9e70-17d333ae6e9d.png)
  
# How to describe the hive table attributes?
  
# ROW FORMAT:
  
     It describe the format of the data file that will be loaded to Hive. There are two possible types: DELIMITED, SERDE

# TBLPROPERTIES:
     
     The TBLPROPERTIES clause allows you to tag the table definition with your own metadata key/value pairs.
     Example: Ignoring 'n' number of lines of header or footer from file to be loaded

# STORED AS:
  
     It specifys the format of the data file. 
     The default text file is plain, other different types are ORC, PARQUET, RCFILE, AVRO, JSONFILE, SEQUENCEFILE

# Partition By:
  
     > This clasue is used to partition the data into multiple files 
     > It is good in terms of performance tunning
     > Table data is divided into multiple parts based on the low cardinality column ###### to increase the performance of the hive
     > Partitions are slices of data which helps to manage the data into more 
       managerable chunks.
     > Partition column is generally taken from the common columns that are frequently used for queries having low cardinality columns. 
     > Basically columns such as date, state, country and departments are taken as a ###### partition column.
     > It is a good way of dividing a table into parts based on the value of the partitioned attribute of the column.
     > Each partition can have its own columns, serDe and storage info
     > Partition determine how the data is stored in a manageable way to increase the ###### performance of the hive
     > Hive creates directorty for each value of partitioned column
     > Each partitioned directory stores data containing only partitioned value as specified by directory/folder name
     > Hive creates seperate directory for each value of partitioned column

# Clustered by:
  
     > Bucketing is a concept to clasify the data into more manageable parts to increase the performance of the hive.
     > Table data is classified into n number of buckets using hash function.
     > Bucketing is based on the value of the hash function of the particular column ###### of a table.
     > Bucketing is very seful for sampling and join optimization
     > Hash partition within ranges
     > For sub-sampling within a partition
     > It can speed up queries that involve sampling the data (Without bucketing hive ###### scans entire datasets)
     > Hive creates files for each value of bucket
  
# Command regarding partitioned column

Create table employees
            
     (employee_id INT,
  
     hire_date STRING,
  
     department_id INT)
  
     PARTITIONED BY (department_id INT)
  
     Row format delimited 
  
     fields terminated by  ','
  
     Stored as textfile;
  
# How to load data into partitioned table?
  
    load data local inpath ('/home/cloudera/employee.txt/') overwrite into table employees
  
# How to load data into partitioned table referring other table?
  
    load data local inpath '/home/cloudera/employee.txt') overwrite into table employees;
    INSERT OVERWRITE TABLE employees PARTITION(department_id) SELECT
    employee_id, hire_date, department_id;
    
 # Bucketing 
 
      1) Partitioning is good in case of low cardinality column 
      2) But if the data is not possible to partition because of high cardinality like emp_id in such case we need to apply bucketing 
      3) If we apply partitioning in column containing unique values, then it creates extra overhead
      4) If the data is in millions and billions then creating millions/billions of directory for each unique value will impact the performance
      5) So, bucketing is effective in such scenario
      6) Hive uses hash function on the clustered column and data is stored on the buckets as specified by the user while creating buckets
      7) =>Hash_function(bucket_column) Mod (no of buckets)
      
      Create table employees
      (  
      emp_id INT,
      hire_date STRING,
      dept_id INT
      )
      PARTITIONED BY (dept_id INT)
      CLUSTERED by (hire_date) INTO 5 buckets
      Row format delimited 
      fields terminated by  ','
      Stored as textfile;

# Hive and Spark Intregation

     1- Hive has own metastore (metadata is data about data- database information, location, tables, types of file formats, columns etc.)
        > Hive metastore is a central repository of hive metadata
        > We can connect to hive metastore and know detail information about the hive tables
        > Hive metastore stores all the structure information about tables, partitions & buckets
        > Gives HDFS directory, column and data type information 
     2- Command is - describe formatted;
     3- The details is coming from metastore. We have many types of metastore. 
     4- Hive uses derby by default (derby port is 9083 through which hive is connected) 
     5- To pull the data, hive is connected to metastore
     6- In the hive conf folder, there is a file known as hive-site.xml. If you check the hivesite.xml
        --<property>
        --<name>hive.metastore.uris</name>
        --<value>thrift:127.0.0.1:9083</value>
     7- Spark has made a good inregation with hive (hive uses tez or mapreduce engine to process data but spark uses its own engine to process the data)
     8- Metastore is same but processing engine is different. Both hive and spark can see both sides tables if intregated)
     9- Both hive and spark has hive-site.xml in their conf directory
        > check ls /etc/hive/conf
        > check ls /usr/local/spark/conf
   
   ![image](https://user-images.githubusercontent.com/70854976/149390621-713767a6-a40f-4111-9175-6773ade2c0c0.png)
    
    10- Basically, hive table can be queried directly spark.sql("")
    
      => spark.sql("select * from tablename")
      => val df = spark.sql("select * from tablename") (This is common way)
      => val df1 = spark.table("db.table") (This is another way)
      
   ![image](https://user-images.githubusercontent.com/70854976/149403080-15ae7cb9-95a9-4c46-b9f6-8a404548126a.png)

      

     

  
  
