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



#### 1. Install

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

Connect that pluggable database you decide to install these samples by **user SYSTEM**, and run this command

```sql
sqlplus system/oracle_4U@PDBPROD2 @/u01/app/oracle/sample_schemas/mksample.sql oracle_4U oracle_4U hr oe pm ix sh bi EXAMPLE TEMP /u01/app/oracle/sample_schemas/log PDBPROD2
```



#### 2. Details

Objects in these samples, first table is for **HR schema**

| Owner | Object Name             | Object Type |
| ----- | ----------------------- | ----------- |
| HR    | LOC_COUNTRY_IX          | INDEX       |
| HR    | EMP_DEPARTMENT_IX       | INDEX       |
| HR    | LOC_STATE_PROVINCE_IX   | INDEX       |
| HR    | COUNTRY_C_ID_PK         | INDEX       |
| HR    | LOC_CITY_IX             | INDEX       |
| HR    | LOC_ID_PK               | INDEX       |
| HR    | JHIST_DEPARTMENT_IX     | INDEX       |
| HR    | JHIST_EMPLOYEE_IX       | INDEX       |
| HR    | DEPT_ID_PK              | INDEX       |
| HR    | JHIST_JOB_IX            | INDEX       |
| HR    | DEPT_LOCATION_IX        | INDEX       |
| HR    | JOB_ID_PK               | INDEX       |
| HR    | EMP_NAME_IX             | INDEX       |
| HR    | EMP_EMAIL_UK            | INDEX       |
| HR    | EMP_EMP_ID_PK           | INDEX       |
| HR    | EMP_MANAGER_IX          | INDEX       |
| HR    | EMP_JOB_IX              | INDEX       |
| HR    | JHIST_EMP_ID_ST_DATE_PK | INDEX       |
| HR    | REG_ID_PK               | INDEX       |
| HR    | ADD_JOB_HISTORY         | PROCEDURE   |
| HR    | SECURE_DML              | PROCEDURE   |
| HR    | LOCATIONS_SEQ           | SEQUENCE    |
| HR    | EMPLOYEES_SEQ           | SEQUENCE    |
| HR    | DEPARTMENTS_SEQ         | SEQUENCE    |
| HR    | DEPARTMENTS             | TABLE       |
| HR    | JOBS                    | TABLE       |
| HR    | EMPLOYEES               | TABLE       |
| HR    | LOCATIONS               | TABLE       |
| HR    | JOB_HISTORY             | TABLE       |
| HR    | REGIONS                 | TABLE       |
| HR    | COUNTRIES               | TABLE       |
| HR    | SECURE_EMPLOYEES        | TRIGGER     |
| HR    | UPDATE_JOB_HISTORY      | TRIGGER     |
| HR    | EMP_DETAILS_VIEW        | VIEW        |



Next table is associated with **SH schema**, duplicated partitions will be shown only once.

| Owner | Object Name                | Object Type       |
| ----- | -------------------------- | ----------------- |
| SH    | PROMO_PK                   | INDEX             |
| SH    | CUSTOMERS_PK               | INDEX             |
| SH    | PRODUCTS_PK                | INDEX             |
| SH    | TIMES_PK                   | INDEX             |
| SH    | CHANNELS_PK                | INDEX             |
| SH    | COUNTRIES_PK               | INDEX             |
| SH    | SALES_PROD_BIX             | INDEX             |
| SH    | SALES_CUST_BIX             | INDEX             |
| SH    | SALES_TIME_BIX             | INDEX             |
| SH    | SALES_CHANNEL_BIX          | INDEX             |
| SH    | SALES_PROMO_BIX            | INDEX             |
| SH    | SUP_TEXT_IDX               | INDEX             |
| SH    | SYS_IOT_TOP_74771          | INDEX             |
| SH    | SYS_C007710                | INDEX             |
| SH    | DR$SUP_TEXT_IDX$X          | INDEX             |
| SH    | DR$SUP_TEXT_IDX$KD         | INDEX             |
| SH    | DR$SUP_TEXT_IDX$KR         | INDEX             |
| SH    | COSTS_PROD_BIX             | INDEX             |
| SH    | COSTS_TIME_BIX             | INDEX             |
| SH    | PRODUCTS_PROD_STATUS_BIX   | INDEX             |
| SH    | PRODUCTS_PROD_SUBCAT_IX    | INDEX             |
| SH    | PRODUCTS_PROD_CAT_IX       | INDEX             |
| SH    | CUSTOMERS_GENDER_BIX       | INDEX             |
| SH    | CUSTOMERS_MARITAL_BIX      | INDEX             |
| SH    | CUSTOMERS_YOB_BIX          | INDEX             |
| SH    | FW_PSC_S_MV_SUBCAT_BIX     | INDEX             |
| SH    | FW_PSC_S_MV_CHAN_BIX       | INDEX             |
| SH    | FW_PSC_S_MV_PROMO_BIX      | INDEX             |
| SH    | FW_PSC_S_MV_WD_BIX         | INDEX             |
| SH    | SALES_PROD_BIX             | INDEX PARTITION   |
| SH    | SALES_CUST_BIX             | INDEX PARTITION   |
| SH    | SALES_TIME_BIX             | INDEX PARTITION   |
| SH    | SALES_CHANNEL_BIX          | INDEX PARTITION   |
| SH    | SALES_PROMO_BIX            | INDEX PARTITION   |
| SH    | COSTS_PROD_BIX             | INDEX PARTITION   |
| SH    | COSTS_TIME_BIX             | INDEX PARTITION   |
| SH    | FWEEK_PSCAT_SALES_MV       | MATERIALIZED VIEW |
| SH    | CAL_MONTH_SALES_MV         | MATERIALIZED VIEW |
| SH    | SUPPLEMENTARY_DEMOGRAPHICS | TABLE             |
| SH    | SALES_TRANSACTIONS_EXT     | TABLE             |
| SH    | DR$SUP_TEXT_IDX$I          | TABLE             |
| SH    | DR$SUP_TEXT_IDX$K          | TABLE             |
| SH    | DR$SUP_TEXT_IDX$N          | TABLE             |
| SH    | DR$SUP_TEXT_IDX$U          | TABLE             |
| SH    | CAL_MONTH_SALES_MV         | TABLE             |
| SH    | FWEEK_PSCAT_SALES_MV       | TABLE             |
| SH    | SALES                      | TABLE             |
| SH    | COSTS                      | TABLE             |
| SH    | TIMES                      | TABLE             |
| SH    | PRODUCTS                   | TABLE             |
| SH    | CHANNELS                   | TABLE             |
| SH    | PROMOTIONS                 | TABLE             |
| SH    | CUSTOMERS                  | TABLE             |
| SH    | COUNTRIES                  | TABLE             |
| SH    | SALES                      | TABLE PARTITION   |
| SH    | COSTS                      | TABLE PARTITION   |
| SH    | PROFITS                    | VIEW              |



Third table for **OE schema**, duplicated object name will show only once

| Owner | Object Name                    | Object Type |
| ----- | ------------------------------ | ----------- |
| OE    | GET_PHONE_NUMBER_F             | FUNCTION    |
| OE    | ACTION_TABLE_MEMBERS           | INDEX       |
| OE    | CUSTOMERS_PK                   | INDEX       |
| OE    | CUST_ACCOUNT_MANAGER_IX        | INDEX       |
| OE    | CUST_EMAIL_IX                  | INDEX       |
| OE    | CUST_LNAME_IX                  | INDEX       |
| OE    | CUST_UPPER_NAME_IX             | INDEX       |
| OE    | INVENTORY_IX                   | INDEX       |
| OE    | INV_PRODUCT_IX                 | INDEX       |
| OE    | ITEM_ORDER_IX                  | INDEX       |
| OE    | ITEM_PRODUCT_IX                | INDEX       |
| OE    | LINEITEM_TABLE_MEMBERS         | INDEX       |
| OE    | ORDER_ITEMS_PK                 | INDEX       |
| OE    | ORDER_ITEMS_UK                 | INDEX       |
| OE    | ORDER_PK                       | INDEX       |
| OE    | ORD_CUSTOMER_IX                | INDEX       |
| OE    | ORD_ORDER_DATE_IX              | INDEX       |
| OE    | ORD_SALES_REP_IX               | INDEX       |
| OE    | PRD_DESC_PK                    | INDEX       |
| OE    | PRODUCT_INFORMATION_PK         | INDEX       |
| OE    | PROD_NAME_IX                   | INDEX       |
| OE    | PROD_SUPPLIER_IX               | INDEX       |
| OE    | PROMO_ID_PK                    | INDEX       |
| OE    | SYS_C007539                    | INDEX       |
| OE    | SYS_C007540                    | INDEX       |
| OE    | SYS_C007543                    | INDEX       |
| OE    | SYS_C007544                    | INDEX       |
| OE    | SYS_C007545                    | INDEX       |
| OE    | SYS_C007546                    | INDEX       |
| OE    | SYS_C007547                    | INDEX       |
| OE    | SYS_FK0000074439N00007$        | INDEX       |
| OE    | SYS_FK0000074439N00009$        | INDEX       |
| OE    | WAREHOUSES_PK                  | INDEX       |
| OE    | WHS_LOCATION_IX                | INDEX       |
| OE    | ORDERS_SEQ                     | SEQUENCE    |
| OE    | COUNTRIES                      | SYNONYM     |
| OE    | DEPARTMENTS                    | SYNONYM     |
| OE    | EMPLOYEES                      | SYNONYM     |
| OE    | JOBS                           | SYNONYM     |
| OE    | JOB_HISTORY                    | SYNONYM     |
| OE    | LOCATIONS                      | SYNONYM     |
| OE    | ACTION_TABLE                   | TABLE       |
| OE    | CATEGORIES_TAB                 | TABLE       |
| OE    | CUSTOMERS                      | TABLE       |
| OE    | INVENTORIES                    | TABLE       |
| OE    | LINEITEM_TABLE                 | TABLE       |
| OE    | ORDERS                         | TABLE       |
| OE    | ORDER_ITEMS                    | TABLE       |
| OE    | PRODUCT_DESCRIPTIONS           | TABLE       |
| OE    | PRODUCT_INFORMATION            | TABLE       |
| OE    | PRODUCT_REF_LIST_NESTEDTAB     | TABLE       |
| OE    | PROMOTIONS                     | TABLE       |
| OE    | PURCHASEORDER                  | TABLE       |
| OE    | SUBCATEGORY_REF_LIST_NESTEDTAB | TABLE       |
| OE    | WAREHOUSES                     | TABLE       |
| OE    | INSERT_ORD_LINE                | TRIGGER     |
| OE    | ORDERS_ITEMS_TRG               | TRIGGER     |
| OE    | ORDERS_TRG                     | TRIGGER     |
| OE    | PURCHASEORDER$xd               | TRIGGER     |
| OE    | ACTIONS_T                      | TYPE        |
| OE    | ACTION_T                       | TYPE        |
| OE    | ACTION_V                       | TYPE        |
| OE    | CATALOG_TYP                    | TYPE        |
| OE    | CATEGORY_TYP                   | TYPE        |
| OE    | COMPOSITE_CATEGORY_TYP         | TYPE        |
| OE    | CORPORATE_CUSTOMER_TYP         | TYPE        |
| OE    | CUSTOMER_TYP                   | TYPE        |
| OE    | CUST_ADDRESS_TYP               | TYPE        |
| OE    | INVENTORY_LIST_TYP             | TYPE        |
| OE    | INVENTORY_TYP                  | TYPE        |
| OE    | LEAF_CATEGORY_TYP              | TYPE        |
| OE    | LINEITEMS_T                    | TYPE        |
| OE    | LINEITEM_T                     | TYPE        |
| OE    | LINEITEM_V                     | TYPE        |
| OE    | ORDER_ITEM_LIST_TYP            | TYPE        |
| OE    | ORDER_ITEM_TYP                 | TYPE        |
| OE    | ORDER_LIST_TYP                 | TYPE        |
| OE    | ORDER_TYP                      | TYPE        |
| OE    | PART_T                         | TYPE        |
| OE    | PHONE_LIST_TYP                 | TYPE        |
| OE    | PRODUCT_INFORMATION_TYP        | TYPE        |
| OE    | PRODUCT_REF_LIST_TYP           | TYPE        |
| OE    | PURCHASEORDER_T                | TYPE        |
| OE    | REJECTION_T                    | TYPE        |
| OE    | SHIPPING_INSTRUCTIONS_T        | TYPE        |
| OE    | SUBCATEGORY_REF_LIST_TYP       | TYPE        |
| OE    | SYS_YOID0000074448$            | TYPE        |
| OE    | SYS_YOID0000074450$            | TYPE        |
| OE    | SYS_YOID0000074452$            | TYPE        |
| OE    | SYS_YOID0000074454$            | TYPE        |
| OE    | SYS_YOID0000074456$            | TYPE        |
| OE    | WAREHOUSE_TYP                  | TYPE        |
| OE    | CATALOG_TYP                    | TYPE BODY   |
| OE    | COMPOSITE_CATEGORY_TYP         | TYPE BODY   |
| OE    | LEAF_CATEGORY_TYP              | TYPE BODY   |
| OE    | ACCOUNT_MANAGERS               | VIEW        |
| OE    | BOMBAY_INVENTORY               | VIEW        |
| OE    | CUSTOMERS_VIEW                 | VIEW        |



Fourth table for **PM schema**

| Owner | Object Name             | Object Type |
| ----- | ----------------------- | ----------- |
| PM    | PRINTMEDIA_PK           | INDEX       |
| PM    | SYS_C007548             | INDEX       |
| PM    | SYS_FK0000074468N00007$ | INDEX       |
| PM    | PRINT_MEDIA             | TABLE       |
| PM    | TEXTDOCS_NESTEDTAB      | TABLE       |
| PM    | ADHEADER_TYP            | TYPE        |
| PM    | TEXTDOC_TAB             | TYPE        |
| PM    | TEXTDOC_TYP             | TYPE        |



Next table for **BI schema**

| Owner | Object Name | Object Type |
| ----- | ----------- | ----------- |
| BI    | CHANNELS    | SYNONYM     |
| BI    | COSTS       | SYNONYM     |
| BI    | COUNTRIES   | SYNONYM     |
| BI    | CUSTOMERS   | SYNONYM     |
| BI    | PRODUCTS    | SYNONYM     |
| BI    | PROMOTIONS  | SYNONYM     |
| BI    | SALES       | SYNONYM     |
| BI    | TIMES       | SYNONYM     |



Last table for **IX schema**

| Owner | Object Name               | Object Type        |
| ----- | ------------------------- | ------------------ |
| IX    | AQ$_ORDERS_QUEUETABLE_V   | EVALUATION CONTEXT |
| IX    | AQ$_STREAMS_QUEUE_TABLE_V | EVALUATION CONTEXT |
| IX    | AQ$_STREAMS_QUEUE_TABLE_Y | INDEX              |
| IX    | SYS_C007551               | INDEX              |
| IX    | SYS_C007554               | INDEX              |
| IX    | SYS_C007563               | INDEX              |
| IX    | SYS_C007566               | INDEX              |
| IX    | SYS_IOT_TOP_74497         | INDEX              |
| IX    | SYS_IOT_TOP_74499         | INDEX              |
| IX    | SYS_IOT_TOP_74502         | INDEX              |
| IX    | SYS_IOT_TOP_74505         | INDEX              |
| IX    | SYS_IOT_TOP_74526         | INDEX              |
| IX    | SYS_IOT_TOP_74528         | INDEX              |
| IX    | SYS_IOT_TOP_74531         | INDEX              |
| IX    | SYS_IOT_TOP_74534         | INDEX              |
| IX    | SYS_IOT_TOP_74536         | INDEX              |
| IX    | ORDERS_QUEUE_N            | RULE SET           |
| IX    | ORDERS_QUEUE_R            | RULE SET           |
| IX    | STREAMS_QUEUE_N           | RULE SET           |
| IX    | STREAMS_QUEUE_R           | RULE SET           |
| IX    | AQ$_ORDERS_QUEUETABLE_N   | SEQUENCE           |
| IX    | AQ$_STREAMS_QUEUE_TABLE_N | SEQUENCE           |
| IX    | AQ$_ORDERS_QUEUETABLE_G   | TABLE              |
| IX    | AQ$_ORDERS_QUEUETABLE_H   | TABLE              |
| IX    | AQ$_ORDERS_QUEUETABLE_I   | TABLE              |
| IX    | AQ$_ORDERS_QUEUETABLE_L   | TABLE              |
| IX    | AQ$_ORDERS_QUEUETABLE_S   | TABLE              |
| IX    | AQ$_ORDERS_QUEUETABLE_T   | TABLE              |
| IX    | AQ$_STREAMS_QUEUE_TABLE_C | TABLE              |
| IX    | AQ$_STREAMS_QUEUE_TABLE_G | TABLE              |
| IX    | AQ$_STREAMS_QUEUE_TABLE_H | TABLE              |
| IX    | AQ$_STREAMS_QUEUE_TABLE_I | TABLE              |
| IX    | AQ$_STREAMS_QUEUE_TABLE_L | TABLE              |
| IX    | AQ$_STREAMS_QUEUE_TABLE_S | TABLE              |
| IX    | AQ$_STREAMS_QUEUE_TABLE_T | TABLE              |
| IX    | ORDERS_QUEUETABLE         | TABLE              |
| IX    | STREAMS_QUEUE_TABLE       | TABLE              |
| IX    | SYS_IOT_OVER_74502        | TABLE              |
| IX    | SYS_IOT_OVER_74531        | TABLE              |
| IX    | ORDER_EVENT_TYP           | TYPE               |
| IX    | AQ$ORDERS_QUEUETABLE      | VIEW               |
| IX    | AQ$ORDERS_QUEUETABLE_R    | VIEW               |
| IX    | AQ$ORDERS_QUEUETABLE_S    | VIEW               |
| IX    | AQ$STREAMS_QUEUE_TABLE    | VIEW               |
| IX    | AQ$STREAMS_QUEUE_TABLE_R  | VIEW               |
| IX    | AQ$STREAMS_QUEUE_TABLE_S  | VIEW               |
| IX    | AQ$_ORDERS_QUEUETABLE_F   | VIEW               |
| IX    | AQ$_STREAMS_QUEUE_TABLE_F | VIEW               |



#### 3. Statistic Information

The list below demostrates how many lines will exist in each table of users who actually have table objects

| Owner | Table Name                     | Row    |
| ----- | ------------------------------ | ------ |
| HR    | EMPLOYEES                      | 107    |
| HR    | DEPARTMENTS                    | 27     |
| HR    | COUNTRIES                      | 25     |
| HR    | LOCATIONS                      | 23     |
| HR    | JOBS                           | 19     |
| HR    | JOB_HISTORY                    | 10     |
| HR    | REGIONS                        | 4      |
|       |                                |        |
| IX    | AQ$_ORDERS_QUEUETABLE_S        | 4      |
| IX    | AQ$_ORDERS_QUEUETABLE_I        | 2      |
| IX    | AQ$_ORDERS_QUEUETABLE_H        | 2      |
| IX    | AQ$_ORDERS_QUEUETABLE_L        | 2      |
| IX    | ORDERS_QUEUETABLE              | 1      |
| IX    | AQ$_STREAMS_QUEUE_TABLE_S      | 1      |
| IX    | SYS_IOT_OVER_74502             | 0      |
| IX    | AQ$_ORDERS_QUEUETABLE_G        | 0      |
| IX    | AQ$_ORDERS_QUEUETABLE_T        | 0      |
| IX    | AQ$_STREAMS_QUEUE_TABLE_T      | 0      |
| IX    | AQ$_STREAMS_QUEUE_TABLE_H      | 0      |
| IX    | AQ$_STREAMS_QUEUE_TABLE_I      | 0      |
| IX    | AQ$_STREAMS_QUEUE_TABLE_C      | 0      |
| IX    | SYS_IOT_OVER_74531             | 0      |
| IX    | AQ$_STREAMS_QUEUE_TABLE_L      | 0      |
| IX    | STREAMS_QUEUE_TABLE            | 0      |
| IX    | AQ$_STREAMS_QUEUE_TABLE_G      | 0      |
|       |                                |        |
| OE    | PRODUCT_DESCRIPTIONS           | 8640   |
| OE    | INVENTORIES                    | 1112   |
| OE    | ORDER_ITEMS                    | 665    |
| OE    | CUSTOMERS                      | 319    |
| OE    | PRODUCT_REF_LIST_NESTEDTAB     | 288    |
| OE    | PRODUCT_INFORMATION            | 288    |
| OE    | ORDERS                         | 105    |
| OE    | SUBCATEGORY_REF_LIST_NESTEDTAB | 21     |
| OE    | WAREHOUSES                     | 9      |
| OE    | PROMOTIONS                     | 2      |
|       |                                |        |
| PM    | PRINT_MEDIA                    | 4      |
|       |                                |        |
| SH    | DR$SUP_TEXT_IDX$N              | (null) |
| SH    | DR$SUP_TEXT_IDX$U              | (null) |
| SH    | DR$SUP_TEXT_IDX$I              | (null) |
| SH    | DR$SUP_TEXT_IDX$K              | (null) |
| SH    | SALES                          | 918843 |
| SH    | SALES_TRANSACTIONS_EXT         | 916039 |
| SH    | COSTS                          | 82112  |
| SH    | CUSTOMERS                      | 55500  |
| SH    | FWEEK_PSCAT_SALES_MV           | 11266  |
| SH    | SUPPLEMENTARY_DEMOGRAPHICS     | 4500   |
| SH    | TIMES                          | 1826   |
| SH    | PROMOTIONS                     | 503    |
| SH    | PRODUCTS                       | 72     |
| SH    | CAL_MONTH_SALES_MV             | 48     |
| SH    | COUNTRIES                      | 23     |
| SH    | CHANNELS                       | 5      |



Actually, you can check completeness of samples by using SQL commands below

```SQL
SELECT 
    owner "Owner", 
    table_name "Table Name", 
    num_rows "Rows" 
FROM 
    all_tables 
WHERE 
		REGEXP_LIKE(owner, 'HR|SH|PM|IX|BI|OE')
ORDER BY 1, 3 DESC;

----------------------------------------------------------------------------------------------

SELECT 
    owner "Owner",
    object_name "Object Name", 
    object_type "Object Type" 
FROM 
    all_objects 
WHERE 
    REGEXP_LIKE(owner, 'HR|SH|PM|IX|BI|OE') 
ORDER BY 1, 3;
```

