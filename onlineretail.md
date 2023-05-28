Based on the columns you provided, here's an example of how you can create a Hive table for your data:

```sql
CREATE TABLE invoices (
  InvoiceNo STRING,
  StockCode STRING,
  Quantity INT,
  InvoiceDate STRING,
  UnitPrice DOUBLE,
  CustomerID STRING,
  Country STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE;
```

This creates a Hive table named "invoices" with the specified columns. The table is defined with a row format delimited by a comma (',') and stored as a text file.

You can then load data into this table using the `LOAD DATA` statement:

```sql
LOAD DATA INPATH '/path/to/invoices_data.csv' INTO TABLE invoices;
```

Replace '/path/to/invoices_data.csv' with the actual path to your data file. This statement will load the data from the CSV file into the "invoices" table.

Now, you can perform various operations on the "invoices" table in Hive using SQL queries.

Certainly! Here are the operations you requested, performed on the "invoices" table:

a. Find the total and average sales in Hive:
```sql
-- Total sales
SELECT SUM(Quantity * UnitPrice) AS total_sales
FROM invoices;

-- Average sales
SELECT AVG(Quantity * UnitPrice) AS average_sales
FROM invoices;
```

b. Find order details with the maximum cost:
```sql
SELECT InvoiceNo, StockCode, Quantity, InvoiceDate, UnitPrice, CustomerID, Country
FROM invoices
WHERE (Quantity * UnitPrice) = (
  SELECT MAX(Quantity * UnitPrice)
  FROM invoices
);
```

c. Creating an external Hive table to connect to HBase for Customer Information System:
```sql
CREATE EXTERNAL TABLE customer_info_hbase (
  InvoiceNo STRING,
  StockCode STRING,
  Quantity INT,
  InvoiceDate STRING,
  UnitPrice DOUBLE,
  CustomerID STRING,
  Country STRING
)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES (
  "hbase.columns.mapping" = ":key,info:StockCode,info:Quantity,info:InvoiceDate,info:UnitPrice,info:CustomerID,info:Country"
)
TBLPROPERTIES ("hbase.table.name" = "invoices");
```

d. Display records of Customer Information Table in HBase:
```sql
SELECT * FROM customer_info_hbase;
```

Make sure to execute these commands in the Hive shell to perform the respective operations on the "invoices" table.
