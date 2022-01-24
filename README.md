# Hive-Deep-Drive
This repository contains ***all concepts and commands related to hive***.
# Hive Tables

1. `Managed/Internal table`

2. `External table`
     
  If you drop managed table ***the backened directory get deleted***.  
  
  If you drop external table, ***the backened directory is not deleted***.

# External table
     
 * `It stores data not in the /user/hive/warehouse/ directory but internal table stores data in /user/hive/warehouse directory`.
 * `It is under control of hive .i.e. once the table is dropped both the schema and data in the table get dropped.`
 * `But, when droping the external table, only the schema get dropped but data remains exist.`
 * `Internal tables are called managed tables.`
 * `We need to mention the external keyword while creating the external table.`
 * `Data is stored in the system defined /user/hive/warehouse/ directory.` 
 
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
  
# Simple Project start-1

* `Go to the edge node`

  ***hadoop fs -mkdir /user/cloudera/data1***
  ***hadoop fs -put /home/cloudera/data/txnsd.txt /user/cloudera/data1***

* `Go to the hive shell` 

* `Hive`

1. `create database demo;`
  
2. `use demo;`

3. `create table t_data1(id int,name string,category string) row format delimited fields terminated by ',' location '/user/cloudera/t_data1';`

4. `select * from t_data1;`

5. `create table t_data2(id int,name string,category string) row format delimited fields terminated by ',';`

6. `insert into t_data2 select * from t_data1 where category='Gymnastics';`

7. `select * from t_data2;`
#
  ![image](https://user-images.githubusercontent.com/70854976/149632415-46376463-a9f2-445e-9e70-17d333ae6e9d.png)
#
#

# Practice Project-2

  ##### Step 1
  
  *`create a file name txnsd.txt using touch command (touch txnsd.txt)`
  
  ##### Step 2
  `Use command cat txnsd.txt`
  
       00000002,06-01-2011,Exercise & Fitness
       00000003,06-05-2011,Gymnastics
       00000012,02-08-2011,Indoor Games
       00000014,02-25-2011,Gymnastics
       00000022,10-10-2011,Water Sports
       00000023,05-02-2011,Gymnastics
  
 ![image](https://user-images.githubusercontent.com/70854976/149676427-201cb52c-3bd0-4e7e-8f54-25a11e0f365b.png)

  ##### Step 3
  
  * `Use command- hdfs dfs -mkdir /user/cloudera/data`
  
  ![image](https://user-images.githubusercontent.com/70854976/149676523-9d6ce91e-ce96-48b4-a873-ea6c9883e89a.png)
 
 * `copy txnsd.txt to /user/cloudera/data directory`
  
 ![image](https://user-images.githubusercontent.com/70854976/149676591-ab6c80e1-2770-479e-90d1-a2e37c1d4382.png)
 
 * `Check the /user/cloudera/data`
 
 ![image](https://user-images.githubusercontent.com/70854976/149676645-465df0eb-59d1-44f7-893f-5f625480a0e1.png)
  
  ##### Step 3
  
 * `Go to the hive shell and create a database t_db and use this db;`
 
 ![image](https://user-images.githubusercontent.com/70854976/149676744-ef99510c-25ba-4f47-9dcb-592e0570ccd7.png)
  
 ![image](https://user-images.githubusercontent.com/70854976/149676802-ddd88094-9b7c-4ff2-a852-e7009f9ee8f1.png)
 
  ##### Step 4
  
  `select * from t_sdata;`
 
  ##### Step 4
  
  * `create table tardata(id int, datename string, category string) row format delimited fields terminated by ',';`
  
  * `insert into tardata select * from t_sdata where category="Gymnastics";`
 
  ##### Step 5
  
  * `Check the table`
  
  ![image](https://user-images.githubusercontent.com/70854976/149676950-3dc3cfcc-e8f1-415f-a4df-d64b9495d0b5.png)

# How to describe the hive table attributes?
  
# ROW FORMAT:
  
* `It describe the format of the data file that will be loaded to Hive. There are two possible types: DELIMITED, SERDE`

# TBLPROPERTIES:
     
     `The TBLPROPERTIES clause allows you to tag the table definition with your own metadata key/value pairs`.
     `Example: Ignoring 'n' number of lines of header or footer from file to be loaded`

# STORED AS:
  
     It specifys the format of the data file. 
     The default text file is plain, other different types are ORC, PARQUET, RCFILE, AVRO, JSONFILE, SEQUENCEFILE
#
# How to download the data from AWS S3?

     aws s3 presign s3://<bucket_name>/<folder_name>/<file_name>/
#
# Project-3 (STATIC PARTITION)

     * Project explanation- First create a dir pardata, then go to the hive shell and create database partc.
     Use this database and create table called tar_part and load the dat  from HDFS dirS given below. 
     Finally check the partition directories from hive shell.

     ==> Check in Edge Node

     ==> ls /home/cloudera/partdata/

     ==>   Hive Shell

     ==> create database partc;

  1. use partc;

  2. create table tar_part(id string,name string,check string) partitioned by (country string) row format delimited fields terminated by ','  location '/user/cloudera/tar_part';

  3. load data local inpath '/home/cloudera/partdata/INDTxns.csv' into table tar_part partition(country='INDIA');

  4. load data local inpath '/home/cloudera/partdata/UKTxns.csv' into table tar_part partition(country='UK');

  5. load data local inpath '/home/cloudera/partdata/USTxns.csv' into table tar_part partition(country='USA');

     ==> !hadoop fs -ls /user/cloudera/tar_part;
     ==> !hadoop fs -ls /user/cloudera/tar_part/country=INDIA;
     ==> !hadoop fs -ls /user/cloudera/tar_part/country=UK;
     ==> !hadoop fs -ls /user/cloudera/tar_part/country=USA;

# Project-4 (STATIC PARTITION)
     
     * Project explanation: 
     
   ***Create two tables static_part and stg, then load data from '/home/cloudera/partdata/allcountry.csv' into table stg. 
   ***Next is select the table from stg and insert into static_part***.
   ***Now you can see the directories /user/cloudera/static_part/ and /user/cloudera/static_part/country=INDIA/****.

 * `create table static_part(id int,name string,check1 string) partitioned by (country string) row format delimited fields terminated by ',' location  '/user/cloudera/static_part';`

* `create table stg(id int,name string,check1 string,country string) row format delimited fields terminated by ',' location '/user/cloudera/stg';`

* `load data local inpath '/home/cloudera/partdata/allcountry.csv' into table stg;`

     ==> ***select * from stg***;

* `insert into static_part partition(country='INDIA') select id,name,check1 from stg where country='IND';`

  ***!hadoop fs -ls /user/cloudera/static_part/***;
  ***!hadoop fs -cat /user/cloudera/static_part/country=INDIA/****;
  
 # Project-5
  
`step1 - create database checkpart;`
      ==> use checkpart;
      
`step2 - create table static_part_new(id int,name string,check1 string) partitioned by (country string) row format delimited fields terminated by ',' location` `'/user/cloudera/static_part_new';`

step3 - `create table stg_new(id int,name string,check1 string,country string) row format delimited fields terminated by ',' location '/user/cloudera/stg_new';`

step4 - `load data local inpath '/home/cloudera/partdata/allcountry.csv' into table stg_new;`

`step5 - do the following commands:`

     ==> select * from stg_new;

     ==> insert into static_part_new partition(country='INDIA') select '(country)?+.+' from stg_new where country='IND';

     ==> !hadoop fs -ls /user/cloudera/static_part_new/;
     ==> !hadoop fs -cat /user/cloudera/static_part_new/country=INDIA/*;
  
# Project-6 (Static Partition)
     
     Problem statement:-
     Create a partitioned table dest_part having columns id, name, check which is partitioned
     by column country. Next is Insert only the India data from stg to the target partiitoned table
     with the partitioned name as INDIA.
  
* `Step1 - syntax to create a partitioned table is`
  
       create table dest_part(id int, name string, check string) partitioned by (country ='India')
       row format delimited fields terminated by ',' loacation '/user/cloudera/dest_part';
     
* `Step2 - Syntax is`
 
      Insert into dest_part partition(country = 'India') select id, name, check from stg where country ='INDIA';
      
      (comment: Partitioned column is already getting filled so inly take three columns)
      (If you have many columns need to ommit then the following command is used:
       
      enable this property:
       --set hive.support.quoted.identifiers=none;
       --select `(partition_column)?+.+` from <table_name>;)
       
* `Step3 - Check the partitoned column using this syntax`
 
      !hadoop fs -ls /user/cloudera/dest_part
      (comment: you see the following partitioned column)

![image](https://user-images.githubusercontent.com/70854976/150693685-5c1cb0ff-bc72-4717-9af4-78cc614a94e9.png)

* `ste4 - Describe table, it gives the clear information about the partitioned column`

     desc dest_part;

![image](https://user-images.githubusercontent.com/70854976/150693876-cefb4251-22dc-4aa7-abb9-324588a5fcb1.png)

# `Summary of static partition`

1- The context would be in case only we need the certain portion of data from the whole data. 

2- The partitioned name is customize.

3- Static laod: If the data is already seperated and we have to load into the partition

4- Static Insert: If the data is dumped in a single file and if we are required only certain portion of data with customized partition name.

5- Dynamic Insert: If the data is dumped in a single file and required all the category of data with non partitioned name.

# Project-6 (Dynamic partition)

     Explanation:-
     `Suppose you have allcounrty.csv data consisting data related to India, US and UK at location /home/cloudera/partdata.` 
     `Create a partitioned table with columns (id, name, check) with partitioned column as country.`
     `Find a way to create partitions in target table in all the countries automatically?`

----Solutions:

`step-1 (make sure the following two things down here)`
     
   ***I have allcountries.csv in edge node (single file)***
   ***I also have table partdata in HDFS location***.
    
     ----create a database name dyn_part
     ----use dyn_part
     
 `step2 - Create a stagging table`
 
      create table stg(id int, name string, check string) partitioned by (country string)
      row format delimited fields terminated by ','
      location '/user/cloduera/dyn_part'
      
 `step3 - Create a dynamic partitioned table`
     
     create table dyn(id int, name string, check string) partitioned by (country string)
     row format delimited fields terminated by ','
     location '/user/cloduera/dyn_part'
     
`step4 - load the data into stagging table`

     ***load data local inpath '/user/cloudera/allcountries.csv into table stg***;
     
`step5- country column should be referred to the target table`

   --***enabled the property***:
     
     => set hive.exec.dynamic.partition.mode=nonstrict

     => insert into dyn_part partition(country) select id, name, check, country from stg;
     
     => (comment: Here the difference from static partition is that whatever the partitioned column we have
     we mention to the last portion of the select statement as described above)
     
![image](https://user-images.githubusercontent.com/70854976/150695040-e7c8766a-fb23-4b38-b0c1-a96c627e1c3c.png)
#
# Full code:

![image](https://user-images.githubusercontent.com/70854976/150695327-647071df-3623-4f01-8589-ff1a2ac911a4.png)
#
# Partition By:
  
1) This clasue is used to partition the data into multiple files 
2) It is good in terms of performance tunning
3) Table data is divided into multiple parts based on the low cardinality column to increase the performance of the hive
4) Partitions are slices of data which helps to manage the data into more 
       managerable chunks.
5) Partition column is generally taken from the common columns that are frequently used for queries having low cardinality columns. 
6) Basically columns such as date, state, country and departments are taken as a partition column.
7) It is a good way of dividing a table into parts based on the value of the partitioned attribute of the column.
8) Each partition can have its own columns, serDe and storage info
9) Partition determine how the data is stored in a manageable way to increase the performance of the hive
10) Hive creates directorty for each value of partitioned column
11) Each partitioned directory stores data containing only partitioned value as specified by directory/folder name
12) Hive creates seperate directory for each value of partitioned column

# Clustered by:
  
1) Bucketing is a concept to clasify the data into more manageable parts to increase the performance of the hive.
2) Table data is classified into n number of buckets using hash function.
3) Bucketing is based on the value of the hash function of the particular column of a table.
4) Bucketing is very seful for sampling and join optimization
5) Hash partition within ranges
6) For sub-sampling within a partition
7) It can speed up queries that involve sampling the data (Without bucketing hive scans entire datasets)
8) Hive creates files for each value of bucket
  
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

      

     

  
  
