Here's an example application using HBase and HiveQL for the OnlineRetail dataset with the specified columns:

i. Create and load table with Online Retail data in Hive:

```sql
-- Create Online Retail table in Hive
CREATE TABLE online_retail (
  InvoiceNo STRING,
  StockCode STRING,
  Quantity INT,
  InvoiceDate STRING,
  UnitPrice DOUBLE,
  CustomerID STRING,
  Country STRING
)


-- Load data into Online Retail table
LOAD DATA INPATH '/path/to/online_retail_data.csv' INTO TABLE online_retail;
```

Replace '/path/to/online_retail_data.csv' with the actual path to your Online Retail data file.

j. Create index on Online Retail table in Hive:
```sql
CREATE INDEX idx_unit_price ON TABLE online_retail (UnitPrice) AS 'org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler' WITH DEFERRED REBUILD;
```

k. Find the total and average sales in Hive:
```sql
-- Total sales
SELECT SUM(Quantity * UnitPrice) AS total_sales
FROM online_retail;

-- Average sales
SELECT AVG(Quantity * UnitPrice) AS average_sales
FROM online_retail;
```

l. Find order details with maximum cost:
```sql
SELECT InvoiceNo, StockCode, Quantity, InvoiceDate, UnitPrice, CustomerID, Country
FROM online_retail
WHERE (Quantity * UnitPrice) = (
  SELECT MAX(Quantity * UnitPrice)
  FROM online_retail
);
```

m. Find customer details with maximum order total:
```sql
SELECT CustomerID, SUM(Quantity * UnitPrice) AS order_total
FROM online_retail
GROUP BY CustomerID
ORDER BY order_total DESC
LIMIT 1;
```

n. Find the country with maximum and minimum sale:
```sql
-- Country with maximum sale
SELECT Country, SUM(Quantity * UnitPrice) AS total_sales
FROM online_retail
GROUP BY Country
ORDER BY total_sales DESC
LIMIT 1;

-- Country with minimum sale
SELECT Country, SUM(Quantity * UnitPrice) AS total_sales
FROM online_retail
GROUP BY Country
ORDER BY total_sales ASC
LIMIT 1;
```

o. Creating an external Hive table to connect to HBase for OnlineRetail:
```sql
CREATE EXTERNAL TABLE online_retail_hbase (
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
TBLPROPERTIES ("hbase.table.name" = "online_retail");
```

p. Display records of OnlineRetail table in HBase:
```sql
scan 'online_retail_hbase'
```

Make sure to execute these commands in the Hive shell to perform the respective operations on the OnlineRetail dataset.
