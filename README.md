# Aram Radif - deacademy-dbt

## Project Overview

Snowflake dbt Projects

### Objective

Repository to contain dbt code

Snowflake Code
============

CREATE OR REPLACE TABLE DBT_DB.PUBLIC.EMPLOYEE_RAW 
(
EMPID INTEGER,
NAME STRING,
SALARY INTEGER,
HIREDATE DATE,
ADDRESS STRING
);


INSERT INTO DBT_DB.PUBLIC.EMPLOYEE_RAW (EMPID, NAME, SALARY, HIREDATE, ADDRESS) VALUES
(1, 'John Smith', 75000, '2021-03-15', '10 Downing Street, London, UK, SW1A 2AA'),
(2, 'Emily Davis', 82000, '2020-07-22', '350 Bay Street, Toronto, Canada, M5H 2S6'),
(3, 'Michael Johnson', 91000, '2019-11-05', '1 Macquarie Street, Sydney, Australia, 2000'),
(4, 'Rajesh Kumar', 67000, '2022-01-10', 'Gateway of India, Mumbai, India, 400001'),
(5, 'William Taylor', 98000, '2018-09-30', 'Pariser Platz, Berlin, Germany, 10117');


select * from DBT_DB.PUBLIC.EMPLOYEE_RAW ;

select * from DBT_DB.PUBLIC.EMPLOYEE;


DBT Model Code
============

{{
    config
    (
        materialized='table'
    )
}}
 
with employee_raw as
(
    select
    EMPID as emp_id,
    split_part(NAME,' ',1)  as emp_firstname,
    split_part(NAME,' ',2)  as emp_lastname,
    SALARY as emp_salary,
    HIREDATE as emp_hiredate,
    split_part(ADDRESS,',',1) as emp_street,
    split_part(ADDRESS,',',2) as emp_city,
    split_part(ADDRESS,',',3) as emp_country,
    split_part(ADDRESS,',',4) as emp_zipcode
    from {{source('employee','EMPLOYEE_RAW')}}
)
select * from employee_raw


Yml file code
==========

version: 2


models:
  - name: employee
    description: "DBT Model for employee table"
   
sources:
  - name: employee
    database: DBT_DB
    schema: PUBLIC
    tables:
      - name: EMPLOYEE_RAW

---

## Author
Aram Radif

