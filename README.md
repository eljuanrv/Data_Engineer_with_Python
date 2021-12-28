# Data_Engineer_with_Python
Este repositorio es para llevar el seguimiento del curso de Data Engineer with Python que estoy tomando en Data Camp
## INTRODUCCION A LA INGENIERIA DE DATOS
### Herramientas del Ingeniero de Datos
Un ingeniero de datos ueve datos de diferents fuentes, procesos o los limpia y finalmnte los carga en una base de datos anaitica.
Hace eso utilizando varias herramientas 
- Bases de datos
- Procesar datos (clean, agregate, join )
- Scheduling
Existing tools
- (MySQL, PostgreeSQL)
- (Spark, Hive)
- (Apache Airflow, oozie)

### CLOUD PROVIDERS TBT(AWS, AZURE, GOOGLE)
- Data Processing in the cloud (clusters of compute power)
- Data Storage In the Cloud (Reliability)

Offers
- Storage: AWS S3, Azure Blob Storage, Google Cloud Storage
- Computation: AWS AC2, Azure Virtual Machines, Google Compute Engine
- Databases: AWS RDS, Azures SQL Database, Google Cloud SQL 

### Databases
They are usually a large collection of data organized especially for rapid search and retrieval
- Structured Data (tabular data)
- Semistructured Data (JSON)
- Unstructured Data (Videos)
- SQL
- NoSQL 
Example
How to query a SQL Database Using Pandas

Instructions

- Complete the SELECT statement so it selects the first_name and the last_name in the "Customer" table. 
- Make sure to order by the last name first and the first name second.
- Use the .head() method to show the first 3 rows of data.
- Use .info() to show some general information about data.

``` 
import pandas as pd

# Complete the SELECT statement
data = pd.read_sql("""
SELECT first_name, last_name FROM "Customer"
ORDER BY last_name, first_name
""", db_engine)

# Show the first 3 rows of the DataFrame
print(data.head(3))

# Show the info of the DataFrame
print(data.info()) 
```
Es solo un ejemplo


Example 2

Complete the SELECT statement, so it joins the "Customer" with the "Order" table.
Print the id column of data. What do you see?

```
# Complete the SELECT statement
data = pd.read_sql("""
SELECT * FROM "Customer"
INNER JOIN "Order"
ON "Order"."customer_id"="Customer"."id"
""", db_engine)

# Show the id column of data
print(data.id)
```

### Parallel Computing

Example 1
*multiprocessing.Pool*
```
from multiprocessing import Pool
import pandas as pd

def take_mean_age(year_and_group):
	year, group = year_and_group
	return pd.Dataframe({"Age":group["Age"].mean()}, index=[year])

with Pool(4) as p:
	results = p.map(take_mean_age, athlete_events.groupby("Year"))
	
result_df=pd.concat(results)
```
Example 2
*dask*
```
import dask.dataframe as dd

# Partition dataframe into 4
athlete_events_dask = dd.from_pandas(athlete_events, npartitions = 4)

#Run parallel computations on each partition
result_df = athlete_events_dask.groupby("Year").Age.mean().compute()
```
Exercise  1

*From task to subtasks*

For this exercise, you will be using parallel computing to apply the function take_mean_age() that calculates the average athlete's age in a given year in the Olympics events dataset. The DataFrame athlete_events has been loaded for you and contains amongst others, two columns:

- Year: the year the Olympic event took place
- Age: the age of the Olympian
You will be using the multiprocessor.Pool API which allows you to distribute your workload over several processes. The function parallel_apply() is defined in the sample code. It takes in as input the function being applied, the grouping used, and the number of cores needed for the analysis. Note that the @print_timing decorator is used to time each operation.

Instructions

Complete the code, so you apply take_mean_age with 1 core first, then 2 and finally 4 cores.

```
# Function to apply a function over multiple cores
@print_timing
def parallel_apply(apply_func, groups, nb_cores):
    with Pool(nb_cores) as p:
        results = p.map(apply_func, groups)
    return pd.concat(results)

# Parallel apply using 1 core
parallel_apply(take_mean_age, athlete_events.groupby('Year'), 1)

# Parallel apply using 2 cores
parallel_apply(take_mean_age, athlete_events.groupby('Year'), 2)

# Parallel apply using 4 cores
parallel_apply(take_mean_age, athlete_events.groupby('Year'), 4)
```

Excersise 2

In the previous exercise, you saw how to split up a task and use the low-level python multiprocessing.Pool API to do calculations on several processing units.

It's essential to understand this on a lower level, but in reality, you'll never use this kind of APIs. A more convenient way to parallelize an apply over several groups is using the dask framework and its abstraction of the pandas DataFrame, for example.

The pandas DataFrame, athlete_events, is available in your workspace.


- Create 4 partitions of the athletes_events DataFrame using dd.from_pandas().
If you forgot the parameters of dd.from_pandas(), check out the slides again, or type help(dd.from_pandas) in the console!

- Print out the mean age for each Year. Remember dask uses lazy evaluation.
```
import dask.dataframe as dd

# Set the number of partitions
athlete_events_dask = dd.from_pandas(athlete_events, npartitions=4)

# Calculate the mean Age per Year
print(athlete_events_dask.groupby('Year').Age.mean().compute())
```

### Parallel Computing Frameworks

*Apache Hadoop(projects outdated)*
- MapReduce: split problems in multiple tasks and each of them are located in a single computer (It was hard to write this maapReduce jobs)
- HDFS: Distributed file sistem like my computer but the files reside in several computers
- Hive: Solve the MapReduce problem (Hive SQL)
example of Hive query
```
SELECT year, AVG(age)
FROM views.athlete_events
GROUP BY year
```
This example can be executed by a cluster of computers

*Apache Spark*

- Distributes data processing tasks between clusters of computers
- Tries to keep as much processing as possible in memory
- An answer to the limitations of MapReduced
- Originates in the University of California
- *Resiliente Distributed Datasets (RDD)*: Spark relies on them, data structure that mantaind data distributed between multiple nodes
- (RDDs) dont have name columns, are like a list of tuples
- We can do two types of operations like Transformations(map, filter, result is a transformed RDDs) or Actions (count, first, result is a single result)
- *PySpark*: Interfaz programming language to spark (Dataframe abstractions and similar to pandas)

*Example PySpark*
```
(athlete_events_spark
	.groupBy('Year')
	.mean('Age')
	.show())
```

Excrcise

*A PySpark groupby*

- You've seen how to use the dask framework and its DataFrame abstraction to do some calculations. However, as you've seen in the video, in the big data world Spark is probably a more popular choice for data processing.

- In this exercise, you'll use the PySpark package to handle a Spark DataFrame. The data is the same as in previous exercises: participants of Olympic events between 1896 and 2016.

- The Spark Dataframe, athlete_events_spark is available in your workspace.

- The methods you're going to use in this exercise are:

- .printSchema(): helps print the schema of a Spark DataFrame.
- .groupBy(): grouping statement for an aggregation.
- .mean(): take the mean over each group.
- .show(): show the results.

Instructions

1. Find out the type of athlete_events_spark.
2. Find out the schema of athlete_events_spark.
3. Print out the mean age of the Olympians, grouped by year. Notice that spark has not actually calculated anything yet. You can call this lazy evaluation.
4. Take the previous result, and call .show() on the result to calculate the mean age.
```
# Print the type of athlete_events_spark
print(type(athlete_events_spark))

# Print the schema of athlete_events_spark
print(athlete_events_spark.printSchema())

# Group by the Year, and find the mean Age
print(athlete_events_spark.groupBy('Year').mean('Age'))

# Group by the Year, and find the mean Age
print(athlete_events_spark.groupBy('Year').mean('Age').show())
```

Excercise 2

Running PySpark files
In this exercise, you're going to run a PySpark file using spark-submit. This tool can help you submit your application to a spark cluster.

For the sake of this exercise, you're going to work with a local Spark instance running on 4 threads. The file you need to submit is in /home/repl/spark-script.py. Feel free to read the file:
```
cat /home/repl/spark-script.py
```
You can use spark-submit as follows:
```
spark-submit \
  --master local[4] \
  /home/repl/spark-script.py
 ```
What does this output? Note that it may take a few seconds to get your results.
- A DataFrame with average Olympian heights by year.

### WorkFlow Scheduling Frameworks

Orchestrate jobs using pipelines

*Directed Acyclic Graph (DAGs)*
- A DAG is a set of nodes that are conected by directed edges

*Tools*
- Linux  ```cron ``` 
- SpotIfY  ```LUIGI ```
- Apache  ```AIRFLOW ```

*Example of Apache Spark*
1. start_cluster
2. ingest_customer_data
3. ingest_product_data
4. enrich_customer_data

primero debe ocurrir *1* despues *2 y 3* y hasta el fnal *4*

*Example in code*
```
# Create the DAG object
dag = DAG(dag_id = 'example_dag', ..., schedule_interval = '0 * * * *')


#Define operations
start_cluster = StartClusterOperator(task_id = 'start_cluster', dag=dag)
ingest_customer_data = SparkJobOperator(task_id = 'ingest_customer_data', dag=dag)
ingest_product_data = SparkJobOperator(task_id = 'ingest_product_data', dag=dag)
enrich_customer_data = PythonOperator(task_id = 'enrich_customer_data', ..., dag = dag)

# Set up dependency flow
start_cluster.set_downstream(ingest_customer_data)
ingest_customer_data.set_downstream(enrich_customer_data)
ingest_product_data.set_downstream(enrich_customer_data)
```

Exercise 

Airflow DAGs
In Airflow, a pipeline is represented as a Directed Acyclic Graph or DAG. The nodes of the graph represent tasks that are executed. The directed connections between nodes represent dependencies between the tasks.

Representing a data pipeline as a DAG makes much sense, as some tasks need to finish before others can start. You could compare this to an assembly line in a car factory. The tasks build up, and each task can depend on previous tasks being finished. A fictional DAG could look something like this:

Example DAG

Assembling the frame happens first, then the body and tires and finally you paint. Let's reproduce the example above in code.

