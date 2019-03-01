# Data migration from Mysql to Hive and Hbase
Data is going to migrate from mysql database to hbase and hive



***************Data migration mysql to hbase:
on mysql
1.grant all on *.* to  'root'@'localhost' with grant option;

on hbase shell
1.create 'sagveek','personalinfo'

on linux shell:

1.sqoop import --driver com.mysql.jdbc.Driver --connect 'jdbc:mysql://localhost/swaroop' 
--username root --password admin --table emp --hbase-table sagveek --column-family personalinfo
--hbase-row-key id --m 1





********data migration mysql to hive
on mysql database:
grant all on *.* to 'root'@'localhost' with grant option;
flush privileges;

on linux:
sqoop import --driver com.mysql.jdbc.Driver --connect 'jdbc:mysql://localhost/swaroop' --username root --password admin --split-by id --columns id,name --table emp --target-dir /datanew --hive-import --create-hive-table --hive-table default.empnew --m 1

on hive:

CREATE TABLE empnewone(
id string, 
name string)
COMMENT 'Imported by sqoop on 2016/03/01 13:01:49'
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
LINES TERMINATED BY '\n' 
STORED AS INPUTFORMAT 
'org.apache.hadoop.mapred.TextInputFormat' 
OUTPUTFORMAT 
'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
'/user/hive/warehouse/emp'
TBLPROPERTIES (
'COLUMN_STATS_ACCURATE'='true', 
'numFiles'='4', 
'totalSize'='77', 
'transient_lastDdlTime'='1456866115')



select * from empnew;
