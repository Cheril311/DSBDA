Sure! Here's an example application that demonstrates the use of HBase and HiveQL for a flight information system:

go to hbase shell

```
hbase shell
```


a. Create Flight Info HBase Table:
```
create 'flight_info', 'flight', 'schedule', 'delay'
```
```
put 'flight_info', '1', 'flight:flight_id', 'F001'
put 'flight_info', '1', 'flight:airline', 'Delta Airlines'
put 'flight_info', '1', 'flight:source', 'New York'
put 'flight_info', '1', 'flight:destination', 'Los Angeles'
put 'flight_info', '1', 'schedule:departure_time', '2023-05-01 08:00:00'
put 'flight_info', '1', 'schedule:arrival_time', '2023-05-01 12:00:00'
put 'flight_info', '1', 'delay:departure_delay', '15'
put 'flight_info', '1', 'delay:arrival_delay', '0'

put 'flight_info', '2', 'flight:flight_id', 'F002'
put 'flight_info', '2', 'flight:airline', 'United Airlines'
put 'flight_info', '2', 'flight:source', 'Chicago'
put 'flight_info', '2', 'flight:destination', 'San Francisco'
put 'flight_info', '2', 'schedule:departure_time', '2023-05-01 10:30:00'
put 'flight_info', '2', 'schedule:arrival_time', '2023-05-01 15:00:00'
put 'flight_info', '2', 'delay:departure_delay', '0'
put 'flight_info', '2', 'delay:arrival_delay', '30'

put 'flight_info', '3', 'flight:flight_id', 'F003'
put 'flight_info', '3', 'flight:airline', 'American Airlines'
put 'flight_info', '3', 'flight:source', 'Los Angeles'
put 'flight_info', '3', 'flight:destination', 'New York'
put 'flight_info', '3', 'schedule:departure_time', '2023-05-01 14:45:00'
put 'flight_info', '3', 'schedule:arrival_time', '2023-05-01 22:00:00'
put 'flight_info', '3', 'delay:departure_delay', '45'
put 'flight_info', '3', 'delay:arrival_delay', '60'
```


b. Demonstrate Creating, Dropping, and Altering Database Tables in HBase:
```
# Create table
create 'my_table', 'column_family'

# Drop table
disable 'my_table'
drop 'my_table'

# Alter table
alter 'my_table', NAME => 'column_family:new_column', VERSIONS => 1
```

go to hive shell
```
hive
```


c. Creating an External Hive Table to Connect to HBase for Flight Information Table:
```sql
CREATE EXTERNAL TABLE flight_info_hbase (
  flight_id string,
  airline string,
  source string,
  destination string,
  departure_delay int,
  arrival_delay int
)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES (
  "hbase.columns.mapping" = ":key,flight:airline,flight:source,flight:destination,delay:departure_delay,delay:arrival_delay"
)
TBLPROPERTIES ("hbase.table.name" = "flight_info");
```

d. Find the Total Departure Delay in Hive:
```sql
SELECT SUM(departure_delay) AS total_departure_delay
FROM flight_info_hbase;
```

e. Find the Average Departure Delay in Hive:
```sql
SELECT AVG(departure_delay) AS average_departure_delay
FROM flight_info_hbase;
```

f. Create an Index on Flight Information Table:
```sql
CREATE INDEX idx_departure_delay ON TABLE flight_info_hbase (departure_delay) AS 'org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler';
```

Make sure to execute these commands in the Hive shell to perform the respective operations on the flight information system.
