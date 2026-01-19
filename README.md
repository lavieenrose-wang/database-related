# Database Shell Scripts

```
author: Zilu Wang (Faculty of Computer Science, Dalhousie University)

[1] Oracle Help Center. Database Sample Schemas. 2019 [cited 2026 Jan 18]. Available from: https://docs.oracle.com/en/database/oracle/oracle-database/19/comsc/index.html
```



## Legacy scott samples

Connect your pluggable database and run this command

```SQL
SQL> @/PATH/TO/YOUR/SCRIPT/utlsampl.sql <scott_password_you_want> <target_pdb_net_service_name>
```

For example

```SQL
SQL> @/PATH/TO/YOUR/SCRIPT/utlsampl.sql tiger PDBPROD2
```



## Oracle 19c Database Samples

**Firstly**, this project is based on db-sample-schemas *<u>[1]</u>*, ***Oracle and/or its affiliates***.

```
This project is based on db-sample-schemas, Oracle and/or its affiliates
licensed under the MIT License.

Modifications and additional code by Zilu Wang.
```

Make sure that user system has a password, and you actually know what the password is. You can alter the password of system in root container database, and do not forget to set container to all

```sql
ALTER USER system IDENTIFIED BY oracle_4U CONTAINER = all;
```

Connect a pluggable database that you want to install these samples, grant unlimited tablespace privilege to system

```sql
GRANT UNLIMITED TABLESPACE TO system;
```

Create a tablespace named EXAMPLE, it must be a permanent tablespace

```SQL
CREATE TABLESPACE EXAMPLE 
	DATAFILE '/u01/app/oracle/oradata/PRODCDB/PDBPROD2/example01.dbf' 
	SIZE 500M AUTOEXTEND ON NEXT 100M MAXSIZE 2G 
	SEGMENT SPACE MANAGEMENT AUTO 
	EXTENT MANAGEMENT LOCAL AUTOALLOCATE;
```

Make sure that temporary tablespace TEMP and EXAMPLE have already established correctly in this pluggable database

```SQL
SELECT
    t.tablespace_name,
    f.file_name,
    f.bytes/1024/1024 "SIZE (MB)",
    f.autoextensible
FROM
    dba_temp_files f
JOIN
    dba_tablespaces t
ON
    f.tablespace_name = t.tablespace_name
ORDER BY
    t.tablespace_name;
    
TABLESPACE_NAME		FILE_NAME																						SIZE (MB) AUT
----------------- --------------------------------------------------- --------- -----------------
TEMP							/u01/app/oracle/oradata/PRODCDB/PDBPROD2/temp01.dbf	36 				YES

==================================================================================================

SELECT
    t.tablespace_name,
    f.file_name,
    f.bytes/1024/1024 "SIZE (MB)",
    f.autoextensible
FROM
    dba_data_files f
JOIN
    dba_tablespaces t
ON
    f.tablespace_name = t.tablespace_name
WHERE
		upper(t.tablespace_name) like '%EXAMPLE%' OR upper(t.tablespace_name) like '%USERS%'
ORDER BY
    t.tablespace_name;
    
TABLESPACE_NAME		FILE_NAME																								SIZE (MB) AUT
----------------- ------------------------------------------------------- --------- -----------------
EXAMPLE						/u01/app/oracle/oradata/PRODCDB/PDBPROD2/example01.dbf	500				YES
USERS							/u01/app/oracle/oradata/PRODCDB/PDBPROD2/users01.dbf		5					YES
```

Please upload sample_schemas to the path '/u01/app/oracle'($ORACLE_BASE), then change the directory to it by 'cd' command, make sure you are in sample_schemas and run this command below

```SQL
perl -p -i.bak -e 's#__SUB__CWD__#'$(pwd)'#g' *.sql */*.sql */*.dat
```

Connect that pluggable database you decide to install these samples by system, and run this command

```sql
sqlplus system/oracle_4U@PDBPROD2 @/u01/app/oracle/sample_schemas/mksample.sql oracle_4U oracle_4U hr oe pm ix sh bi EXAMPLE TEMP /u01/app/oracle/sample_schemas/log PDBPROD2
```







