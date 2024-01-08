# Home_Sales

# Home Sales README

This README provides instructions for the Home Sales assignment, including the steps to perform using PySpark SQL functions. Ensure you follow these steps to complete the assignment successfully.

## Instructions

### 1. Rename the Notebook

Rename the provided notebook file `Home_Sales_starter_code.ipynb` to `Home_Sales.ipynb` before proceeding with the assignment.

### 2. Import Necessary PySpark SQL Functions

Make sure to import the necessary PySpark SQL functions at the beginning of your notebook. Common functions include those from `pyspark.sql.functions`.

```python
from pyspark.sql.functions import *
```

### 3. Read CSV Data

Read the `home_sales_revised.csv` data into a Spark DataFrame. Use the appropriate PySpark functions for reading CSV data.

```python
# Example: Reading CSV data
df = spark.read.csv("path/to/home_sales_revised.csv", header=True, inferSchema=True)
```

### 4. Create Temporary Table

Create a temporary table called `home_sales` using the `createOrReplaceTempView` method.

```python
# Example: Creating a temporary table
df.createOrReplaceTempView('home_sales')
```

### 5. Answer Questions using SparkSQL

Answer the questions using SparkSQL queries. For example:

```python
# Example: Average price for a four-bedroom house sold for each year
query = """
    SELECT
        YEAR(Sale_Date) AS Year,
        ROUND(AVG(Price), 2) AS Avg_Price
    FROM
        home_sales
    WHERE
        Bedrooms = 4
    GROUP BY
        YEAR(Sale_Date)
    ORDER BY
        Year
"""
result = spark.sql(query)
result.show()
```

### 6. Cache Temporary Table

Cache the `home_sales` temporary table using the `cache()` method.

```python
# Example: Caching the temporary table
spark.sql("SELECT * FROM home_sales").cache()
```

### 7. Check if Temporary Table is Cached

Check if the `home_sales` temporary table is cached using the `isCached` method.

```python
# Example: Checking if the table is cached
is_cached = spark.catalog.isCached('home_sales')
print(is_cached)
```

### 8. Run Query on Cached Data

Run a query on the cached data and determine the runtime. Compare it with the uncached runtime.

```python
# Example: Query on cached data and runtime measurement
start_time_cached = time.time()
query_cached = """
    SELECT
        AVG(Price) AS Avg_Price
    FROM
        home_sales
    WHERE
        Price < 350000
"""
result_cached = spark.sql(query_cached)
result_cached.show()
end_time_cached = time.time()
print("--- Cached Query Runtime: %s seconds ---" % (end_time_cached - start_time_cached))
```

### 9. Partition by "date_built" Field on Formatted Parquet Data

Partition the data by the "date_built" field when writing it in Parquet format. Use the `write.partitionBy` method.

```python
# Example: Partitioning by "date_built" field
df.write.partitionBy("date_built").parquet("/path/to/output")
```

### 10. Create Temporary Table for Parquet Data

Create a temporary table for the Parquet data.

```python
# Example: Creating a temporary table for Parquet data
parquet_df.createOrReplaceTempView('parquet_temp')
```

### 11. Run Query on Parquet Data

Run a query on the Parquet data and determine the runtime. Compare it with the uncached runtime.

```python
# Example: Query on Parquet data and runtime measurement
start_time_parquet = time.time()
query_parquet = """
    SELECT
        AVG(Price) AS Avg_Price
    FROM
        parquet_temp
    WHERE
        Price < 350000
"""
result_parquet = spark.sql(query_parquet)
result_parquet.show()
end_time_parquet = time.time()
print("--- Parquet Query Runtime: %s seconds ---" % (end_time_parquet - start_time_parquet))
```

### 12. Uncache Temporary Table

Uncache the `home_sales` temporary table using the `unpersist` method.

```python
# Example: Uncaching the temporary table
spark.catalog.uncacheTable('home_sales')
```

### 13. Verify Temporary Table is Uncached

Verify that the `home_sales` temporary table is no longer cached using PySpark.

```python
# Example: Verifying if the table is uncached
is_cached_after_uncache = spark.catalog.isCached('home_sales')
print(is_cached_after_uncache)
```

### 14. Download and Upload

Download your `Home_Sales.ipynb` file and upload it into your "Home_Sales" GitHub repository. Ensure your notebook reflects all the necessary code, explanations, and results.

Feel free to adjust examples based on your specific DataFrame and column names in your notebook.
