# Part2

## Problem 1
```
SELECT A.ID, A.TYPE, A.STATUS, A.AMOUNT, (A.AMOUNT-B.AVG_AMT) DIFFERENCE
FROM ACCOUNT A
JOIN (SELECT TYPE , AVG(AMOUNT) AS AVG_AMT
      FROM PROBLEM1.ACCOUNT
      GROUP BY TYPE
      ) B
ON A.TYPE = B.TYPE
WHERE A.STATUS='Active'
```

## Problem 2
```
CREATE DATABASE problem2;

USE problem2;

CREATE TABLE problem2.employee
(ID INT,
FNAME STRING,
LNAME STRING,
ADDRESS STRING,
CITY STRING,
STATE STRING,
ZIP STRING,
BIRTHDAY STRING,
HIREDAY STRING
)
STORED AS PARQUET 
LOCATION '/user/training/problem2/data/employee';
```

## Problem 3
```
CREATE TABLE PROBLEM3.SOLUTION AS
SELECT ID, FNAME, LNAME, HPHONE
FROM CUSTOMER A
JOIN ACCOUNT B ON A.ID = B.CUSTID
WHERE B.AMOUNT < 0;
```

## Problem 4
```

```

## Problem 5
```
SELECT concat(A.FNAME,'	',A.LNAME,'	',A.CITY,'	',A.STATE)
FROM(
SELECT FNAME, LNAME, CITY, STATE FROM CUSTOMER
WHERE CITY = 'Palo Alto'
AND STATE = 'CA'
UNION ALL
SELECT FNAME, LNAME, CITY, STATE FROM EMPLOYEE
WHERE CITY = 'Palo Alto'
AND STATE = 'CA'
)A
```

## problem 6
```
CREATE TABLE PROBLEM6.SOLUTION AS
SELECT ID, FNAME, LNAME, ADDRESS, CITY, STATE, ZIP, SUBSTR(BIRTHDAY,1,5) as BIRTHDAY
FROM PROBLEM6.EMPLOYEE A
```

## problem 7
```
SELECT A.FULLNAME FROM
(
SELECT FNAME ,CONCAT(FNAME,' ',LNAME) AS FULLNAME
FROM PROBLEM7.EMPLOYEE A
WHERE CITY = 'Seattle'
ORDER BY FNAME
)A ;
```

## problem 8
```
sqoop export --connect jdbc:mysql://localhost/problem8 --username cloudera --password cloudera --export-dir /user/training/problem8/data/customer --table solution
```

## problem 9
```
CREATE EXTERNAL TABLE problem9.solution(                      
`id` string,                                                 
`fname` string,                                              
`lname` string,                                              
`address` string,                                            
`city` string,                                               
`state` string,                                              
`zip` string,                                                
`birthday` string)                                           
ROW FORMAT SERDE                                               
'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'         
WITH SERDEPROPERTIES (                                         
'field.delim'='\t',                                          
'serialization.format'='\t')                                 
STORED AS INPUTFORMAT                                          
'org.apache.hadoop.mapred.TextInputFormat'                   
OUTPUTFORMAT                                      
'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION   
'hdfs://localhost:8020/user/training/problem9/solution';

INSERT OVERWRITE TABLE PROBLEM9.SOLUTION
SELECT CONCAT('A',string(ID)) AS ID, FNAME, LNAME, ADDRESS, CITY, STATE, ZIP, BIRTHDAY
FROM PROBLEM9.CUSTOMER;
```

## problem 10
```
CREATE VIEW problem10.solution AS
select a.id, a.fname, a.lname, a.city, a.state, b.charge, substr(b.tstamp,1,10) as billdata
from customer a 
join billing b on a.id = b.id;
```

```
add texting
```

```
add text2
```

```
add text3
```
















