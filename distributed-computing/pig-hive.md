# Pig, Hive

## Apache Pig

Developed at Yahoo to make MapReduce accessible to everyone.

```text
bass_prices = LOAD 'input-path' USING PigStorage(',') as (model: chararray, brand: chararray, price: float, strings: int);
grp_by_brand = GROUP bass_prices BY brand;
max_price = FOREACH grp_by_brand GENERATE group, MAX(bass_prices.price) as max_price;
STORE max_price INTO 'output-path' using PigStorage(',');
```

**Apache Pig** takes a set of instructions and converts to MapReduce jobs. It operates on **client side** and allows for both **structured and unstructured** data.

## Apache Hive

Developed by Facebook. Enables to create table like strucutres and run SQL queries. It is very similar to Pig, as they were developed during the same time. While Pig is more suited for nightly tasks, Hive is more appropiate for EDA.

```text
hive> CREATE EXTERNAL TABLE IF NOT EXISTS bass_prices (
model STRING,
brand STRING,
price FLOAT,
strings INT)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
LOCATION 'output_path'

hive> SELECT brand, max(price) max_price) FROM bass_prices
GROUP BY brand
```

In Hive, **Tables** can be either Managed or External. With **Managed** tables, when you drop the data you have, the data in the file will be dropped/deleted. With **External** tables, Hive does not delete the dataset when dropping the Hive table.

Hive operates on **server-side** and allows only for **structured** data.

While Hive's data is stored in HDFS, the metadata is store locally or in RDBMs because HDFS read/write operations are time-consuming.

