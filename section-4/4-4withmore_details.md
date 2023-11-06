# SPARK SQL
## Spark 2.0 Way of DataFrames and DataSets

## Working with Structured Data

- RDDs have no structure; they are just blobs of data.
- DataFrames extend RDDs to create a "DataFrame" object.
- DataFrames:
  - Contain row objects.
  - Support running SQL queries.
  - Have a schema, which leads to more efficient storage.
  - Can read from and write to formats like JSON, Hive, and Parquet.
  - Communicate with JDBC/ODBC and visualization tools like Tableau.

## SparkSQL in Python

- Import the necessary modules to work with Spark SQL in Python.
  - Example:
    ```python
    from pyspark.sql import SQLContext, Row
    ```

- Create a Hive context (hiveContext) to interact with structured data.
  - Example:
    ```python
    hiveContext = HiveContext(sc)
    ```

- Load data from a JSON file into a DataFrame.
  - Example:
    ```python
    inputData = spark.read.json(dataFile)
    ```

- Create a temporary view for the DataFrame, allowing you to run SQL queries.
  - Example:
    ```python
    inputData.createOrReplaceTempView("MyStructuredStuff")
    ```

- Run a SQL query using the Hive context.
  - Example:
    ```python
    myResultDataFrame = hiveContext.sql("SELECT foo FROM bar ORDER by foobar")
    ```

## Other Operations with DataFrames

- Display the contents of a DataFrame.
  - Example:
    ```python
    myResultDataFrame.show()
    ```

- Select specific columns from a DataFrame.
  - Example:
    ```python
    myResultDataFrame.select("someFieldName")
    ```

- Filter data based on conditions.
  - Example:
    ```python
    myResultDataFrame.filter(myResultDataFrame("someFieldName") > 200)
    ```

- Perform aggregations on data.
  - Example:
    ```python
    myResultDataFrame.groupBy("someField").mean()
    ```

- Convert a DataFrame to an RDD and apply RDD operations.
  - Example:
    ```python
    myResultDataFrame.rdd().map(mapperFunction)
    ```

## Datasets

- Datasets are a new concept introduced in Spark 2.0.
- In Spark 2.0, a DataFrame is essentially a Dataset of row objects.
- Datasets offer more compile-time type checking to catch errors early in the development process.
- When using Python, the benefits of Datasets are mostly transparent due to Python's dynamic typing.
- The Spark 2.0 approach is to use Datasets over DataFrames when possible.

## Shell Access

- Spark SQL exposes a JDBC/ODBC server if Spark is built with Hive support.
- Start the server using the `sbin/start-thriftserver.sh` script.
  - Example:
    ```bash
    sbin/start-thriftserver.sh
    ```

- The server listens on port 10000 by default.
- Connect to the server using tools like Beeline.
  - Example:
    ```bash
    bin/beeline -u jdbc:hive2://localhost:10000
    ```

- You now have a SQL shell to Spark SQL, allowing you to create new tables or query existing ones that were cached using `hiveCtx.cacheTable("tableName")`.

## User-Defined Functions (UDFs)

- Import UDF types to work with User-Defined Functions.
  - Example:
    ```python
    from pyspark.sql.types import IntegerType
    ```

- Register UDFs with Spark SQL. For example, registering a UDF that squares a number:
  - Example:
    ```python
    hiveCtx.registerFunction("square", lambda x: x * x, IntegerType())
    ```

- Use registered UDFs in your SQL queries.
  - Example:
    ```python
    df = hiveCtx.sql("SELECT square('someNumericField') FROM tableName")
    ```

This comprehensive guide covers Spark SQL operations, DataFrames, Datasets, shell access, and User-Defined Functions (UDFs). You can adapt these techniques to your specific data processing needs.
