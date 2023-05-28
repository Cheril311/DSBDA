Sure! Here's an example application that demonstrates the use of HBase and HiveQL for a customer information system:

a. Creation of Tables in Hive:
```sql
-- Customer_info table
CREATE TABLE Customer_info (
  Cust_ID INT,
  Cust_Name STRING,
  Order_ID INT
) STORED AS ORC;

-- order_info table
CREATE TABLE order_info (
  Order_ID INT,
  Item_ID INT,
  Quantity INT
) STORED AS ORC;

-- item_info table
CREATE TABLE item_info (
  Item_ID INT,
  Item_Name STRING,
  Item_Price DOUBLE
) STORED AS ORC;
```

b. Load table with data from local storage in Hive:
```sql
-- Load data into Customer_info table
LOAD DATA LOCAL INPATH '/path/to/customer_info_data.csv' INTO TABLE Customer_info;

-- Load data into order_info table
LOAD DATA LOCAL INPATH '/path/to/order_info_data.csv' INTO TABLE order_info;

-- Load data into item_info table
LOAD DATA LOCAL INPATH '/path/to/item_info_data.csv' INTO TABLE item_info;
```

c. Perform Join tables with Hive:
```sql
-- Perform a join on Customer_info, order_info, and item_info tables
SELECT ci.Cust_ID, ci.Cust_Name, oi.Order_ID, oi.Item_ID, oi.Quantity, ii.Item_Name, ii.Item_Price
FROM Customer_info ci
JOIN order_info oi ON ci.Order_ID = oi.Order_ID
JOIN item_info ii ON oi.Item_ID = ii.Item_ID;
```

d. Create an Index on Customer information system in Hive:
```sql
CREATE INDEX idx_customer_name ON TABLE Customer_info (Cust_Name) AS 'COMPACT'
WITH DEFERRED REBUILD;
ALTER INDEX idx_customer_name ON Customer_info REBUILD;
```

e. Find the total and average sales in Hive:
```sql
-- Total sales
SELECT SUM(oi.Quantity * ii.Item_Price) AS total_sales
FROM order_info oi
JOIN item_info ii ON oi.Item_ID = ii.Item_ID;

-- Average sales
SELECT AVG(oi.Quantity * ii.Item_Price) AS average_sales
FROM order_info oi
JOIN item_info ii ON oi.Item_ID = ii.Item_ID;
```

f. Find Order details with maximum cost:
```sql
SELECT oi.Order_ID, oi.Item_ID, oi.Quantity, ii.Item_Name, ii.Item_Price, (oi.Quantity * ii.Item_Price) AS total_cost
FROM order_info oi
JOIN item_info ii ON oi.Item_ID = ii.Item_ID
ORDER BY total_cost DESC
LIMIT 1;
```

g. Creating an External Hive Table to Connect to HBase for Customer Information System:
```sql
CREATE EXTERNAL TABLE customer_info_hbase (
  Cust_ID INT,
  Cust_Name STRING,
  Order_ID INT
)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES (
  "hbase.columns.mapping" = ":key,Customer_info:Cust_Name,Customer_info:Order_ID"
)
TBLPROPERTIES ("hbase.table.name" = "Customer_info");
```

h. Display records of Customer Information Table in HBase:
```sql
SELECT * FROM customer_info_hbase;
```

Make sure to execute these commands in the Hive shell to perform the respective operations on the customer information system.
