# dbt-agent
```
USE ROLE ACCOUNTADMIN;

-- Create the `data` role
CREATE ROLE IF NOT EXISTS data;
GRANT ROLE data TO ROLE ACCOUNTADMIN;

-- Create or use the default warehouse
CREATE WAREHOUSE IF NOT EXISTS COMPUTE;
GRANT OPERATE ON WAREHOUSE COMPUTE TO ROLE data;

-- Create the `deta` user and assign to role
CREATE USER IF NOT EXISTS deta
  PASSWORD='1234'
  LOGIN_NAME='deta'
  MUST_CHANGE_PASSWORD=FALSE
  DEFAULT_WAREHOUSE='COMPUTE'
  DEFAULT_ROLE='data'
  DEFAULT_NAMESPACE='SALES.RAW'
  COMMENT='DBT user used for data transformation';

GRANT ROLE data TO USER deta;

-- Create database and schema
CREATE DATABASE IF NOT EXISTS SALES;
CREATE SCHEMA IF NOT EXISTS SALES.RAW;

-- Set permissions for `data` role
GRANT ALL ON WAREHOUSE COMPUTE TO ROLE data;
GRANT ALL ON DATABASE SALES TO ROLE data;
GRANT ALL ON ALL SCHEMAS IN DATABASE SALES TO ROLE data;
GRANT ALL ON FUTURE SCHEMAS IN DATABASE SALES TO ROLE data;
GRANT ALL ON ALL TABLES IN SCHEMA SALES.RAW TO ROLE data;
GRANT ALL ON FUTURE TABLES IN SCHEMA SALES.RAW TO ROLE data;


-- Use the appropriate database and schema
USE DATABASE SALES;
USE SCHEMA RAW;

USE DATABASE SALES;
USE SCHEMA RAW;



-- Orders table
CREATE OR REPLACE TABLE SALES.RAW.Orders (
    OrderID STRING,
    OrderDate STRING,
    CustomerID STRING,
    ProductID STRING,
    Quantity INTEGER,
    TotalAmount FLOAT,
    PaymentMethod STRING
);

-- Products table
CREATE OR REPLACE TABLE SALES.RAW.Products (
    ProductID STRING,
    ProductName STRING,
    Category STRING,
    Stock INTEGER,
    UnitPrice FLOAT
);

-- Customers table
CREATE OR REPLACE TABLE SALES.RAW.Customers (
    CustomerID STRING,
    CustomerName STRING,
    Email STRING,
    Location STRING,
    SignupDate STRING
);

-- Reviews table
CREATE OR REPLACE TABLE SALES.RAW.Reviews (
    timestamp STRING,
    customer_id STRING,
    product_id STRING,
    rating INTEGER,
    review_text STRING
);

-- Social_Media table
CREATE OR REPLACE TABLE SALES.RAW.Social_Media (
    timestamp STRING,
    platform STRING,
    content STRING,
    sentiment STRING
);

-- Web_Logs table
CREATE OR REPLACE TABLE SALES.RAW.Web_Logs (
    timestamp STRING,
    user_id STRING,
    page STRING,
    action STRING
);
```
