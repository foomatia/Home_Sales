# Home_Sales

# Home Sales README

This README provides instructions for the Home Sales assignment, including the steps to perform using PySpark SQL functions. Ensure you follow these steps to complete the assignment successfully.

## Instructions

This assignment was completed using Google Colab.

### 1. Import Necessary PySpark SQL Functions

Make sure to import the necessary PySpark SQL functions at the beginning of your notebook. Common functions include those from `pyspark.sql.functions`.

```import os
# Find the latest version of spark 3.x  from http://www.apache.org/dist/spark/ and enter as the spark version
# For example:
# spark_version = 'spark-3.5.0'
spark_version = 'spark-3.5.0'
os.environ['SPARK_VERSION']=spark_version

# Install Spark and Java
!apt-get update
!apt-get install openjdk-11-jdk-headless -qq > /dev/null
!wget -q http://archive.apache.org/dist/spark/$SPARK_VERSION/$SPARK_VERSION-bin-hadoop3.tgz
!tar xf $SPARK_VERSION-bin-hadoop3.tgz
!pip install -q findspark

# Set Environment Variables
os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-11-openjdk-amd64"
os.environ["SPARK_HOME"] = f"/content/{spark_version}-bin-hadoop3"

# Start a SparkSession
import findspark
findspark.init()
from pyspark.sql import SparkSession
import time
from pyspark import SparkFiles
```

### 3. Read CSV Data

Read the `home_sales_revised.csv` data into a Spark DataFrame. Use the appropriate PySpark functions for reading CSV data.

```url = "https://2u-data-curriculum-team.s3.amazonaws.com/dataviz-classroom/v1.2/22-big-data/home_sales_revised.csv"

spark.sparkContext.addFile(url)
df = spark.read.csv("file://" + SparkFiles.get("home_sales_revised.csv"), header=True, inferSchema=True)
```

### 4. Create Temporary Table

Create a temporary table called `home_sales` using the `createOrReplaceTempView` method.

```df.createOrReplaceTempView('home_sales')

result = spark.sql("SELECT * FROM home_sales")
result.show()
```

### 5. Answer Questions using SparkSQL

Answer the following questions using SparkSQL queries.
1. What is the average price for a four bedroom house sold in each year rounded to two decimal places?
2. What is the average price of a home for each year the home was built that have 3 bedrooms and 3 bathrooms rounded to two decimal places?
3. What is the average price of a home for each year built that have 3 bedrooms, 3 bathrooms, with two floors, and are greater than or equal to 2,000 square feet rounded to two decimal places?
4. What is the "view" rating for the average price of a home, rounded to two decimal places, where the homes are greater than or equal to $350,000? Although this is a small dataset, determine the run time for this query.

### 6. Cache Temporary Table

Cache the `home_sales` temporary table using the `cache()` method.

```spark.sql("cache table home_sales")
```

### 7. Check if Temporary Table is Cached

Check if the `home_sales` temporary table is cached using the `isCached` method.

```is_cached = spark.catalog.isCached('home_sales')
print(is_cached)
```

### 8. Run Query on Cached Data

Using the cached data, run the query that filters out the view ratings with average price greater than or equal to $350,000. Determine the runtime and compare it to uncached runtime.

+------------------+
|         Avg_Price|
+------------------+
|235148.14186445266|
+------------------+

--- Cached Query Runtime: 0.380997896194458 seconds ---

### 9. Partition by "date_built" Field on Formatted Parquet Data

Partition the data by the "date_built" field when writing it in Parquet format. Use the `write.partitionBy` method.

```df.write.partitionBy("date_built").parquet("/path/to/output")
```

### 10. Create Temporary Table for Parquet Data

Create a temporary table for the Parquet data.

```parquet_path = "/path/to/output"
read_df = spark.read.parquet(parquet_path)
```

### 11. Run Query on Parquet Data

Run a query that filters out the view ratings with average price of greater than or equal to $350,000 on the Parquet data and determine the runtime. Compare it with the uncached runtime.

+------------------+
|         Avg_Price|
+------------------+
|235148.14186445266|
+------------------+

--- Parquet Query Runtime: 0.6985626220703125 seconds ---

### 12. Uncache Temporary Table

Uncache the `home_sales` temporary table.

```spark.sql("uncache table home_sales")
```

### 13. Verify Temporary Table is Uncached

Verify that the `home_sales` temporary table is no longer cached using PySpark.

```is_cached_after_uncache = spark.catalog.isCached("home_sales")
print(is_cached_after_uncache)
```

### 14. Download and Upload

Download your `Home_Sales.ipynb` file and upload it into your "Home_Sales" GitHub repository. Ensure your notebook reflects all the necessary code, explanations, and results.

