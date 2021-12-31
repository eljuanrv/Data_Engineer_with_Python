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

```Python
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

```Py
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
```Py
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
```Py
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

```Py
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
```Py
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
```SQL
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
```Py
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
```Py
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
```Py
cat /home/repl/spark-script.py
```
You can use spark-submit as follows:
```Py
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
```Py
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

![Descripcion](https://assets.datacamp.com/production/repositories/5000/datasets/44f52c1b25308c762f24dcde116b62e275ce7fe1/DAG.png)

Assembling the frame happens first, then the body and tires and finally you paint. Let's reproduce the example above in code.

- First, the DAG needs to run on every hour at minute 0. Fill in the schedule_interval keyword argument using the crontab notation. For example, every hour at minute N would be N * * * *. Remember, you need to run at minute 0.
- The downstream flow should match what you can see in the image above. The first step has already been filled in for you.

```Py
# Create the DAG object
dag = DAG(dag_id="car_factory_simulation",
          default_args={"owner": "airflow","start_date": airflow.utils.dates.days_ago(2)},
          schedule_interval="0 * * * *")

# Task definitions
assemble_frame = BashOperator(task_id="assemble_frame", bash_command='echo "Assembling frame"', dag=dag)
place_tires = BashOperator(task_id="place_tires", bash_command='echo "Placing tires"', dag=dag)
assemble_body = BashOperator(task_id="assemble_body", bash_command='echo "Assembling body"', dag=dag)
apply_paint = BashOperator(task_id="apply_paint", bash_command='echo "Applying paint"', dag=dag)

# Complete the downstream flow
assemble_frame.set_downstream(place_tires)
assemble_frame.set_downstream(assemble_body)
assemble_body.set_downstream(apply_paint)
```
### Extract Data
Is extracting data from persistent storage that is not ready for analisis for example (*file, database or API*)
- Text files: csv or others, JSON
- Data OOn The Web through APIs: Request for Data and you get a response 
- Data on Databases: (Aplications Databases are optimized to allow lots of transactions OLTP, ROW oriented), (Analytical Databases are aptimized for analisis OLAP COLUMN oriented)
- Conection String: is a conection that has information of how to conect to a database 

**Example of conection with a Database using Python**
```Py
import sqlalchemy
import pandas as pd

conection_url = 'postgresql://repl:password@localhost:5432/pagila'
db_engine = sqlalchemy.create_engine(conection_url)

pd.read_sql("SELECT * FROM customer", db_engine)
```

**Example of conection with an API using Python**
```Py
import requests

response = requests.get('https://hackerlink.json')
print(response.json())
```

**Exercise 1: Fetch from an API**

In the last video, you've seen that you can extract data from an API by sending a request to the API and parsing the response which was in JSON format. In this exercise, you'll be doing the same by using the requests library to send a request to the Hacker News API.

Hacker News is a social news aggregation website, specifically for articles related to computer science or the tech world in general. Each post on the website has a JSON representation, which you'll see in the response of the request in the exercise.

Instructions

1. Use the requests module to get the Hacker News post's JSON object.
2. Print out the response, parsed as a JSON.
3. Parsing as JSON again, assign the "score" key of the post to post_score.

```Py
import requests

# Fetch the Hackernews post
resp = requests.get("https://hacker-news.firebaseio.com/v0/item/16222426.json")

# Print the response parsed as JSON
print(resp.json())

# Assign the score of the test to post_score
post_score = resp.json()["score"]
print(post_score)
```

**Exercise 1: Read from a database**

In this exercise, you're going to extract data that resides inside tables of a local PostgreSQL database. The data you'll be using is the Pagila example database. The database backs a fictional DVD store application, and educational resources often use it as an example database.

You'll be creating and using a function that extracts a database table into a pandas DataFrame object. The tables you'll be extracting are:

- film: the films that are rented out in the DVD store.
- customer: the customers that rented films at the DVD store.
In order to connect to the database, you'll have to use a PostgreSQL connection URI, which looks something like this:
```Py
"postgresql://[user[:password]@][host][:port][/database]"
```
- Complete the extract_table_to_pandas() function definition to include the tablename argument within the query.
- Fill in the connection URI. The username and password are repl and password, respectively. The host is localhost and port is 5432. The database is pagila.
- Complete the function calls of extract_table_to_pandas() to extract the film and customer tables.

```Py
# Function to extract table to a pandas DataFrame
def extract_table_to_pandas(tablename, db_engine):
    query = "SELECT * FROM {}".format(tablename)
    return pd.read_sql(query, db_engine)

# Connect to the database using the connection URI
connection_uri = "postgresql://repl:password@localhost:5432/pagila" 
db_engine = sqlalchemy.create_engine(connection_uri)

# Extract the film table into a pandas DataFrame
extract_table_to_pandas("film", db_engine)

# Extract the customer table into a pandas DataFrame
extract_table_to_pandas("customer", db_engine)
```

### Transform
**Example 1 of transform using Python**
```Py
import pandas as pd

customer_df #pandas dataframe with customer data

#split email column into 2 columns on the '@' symbol
split_email = customer_df.email.str.split('@', expand = True)
#at this point, split_email will have two columns, a first
# one with everything before @, and a second one with
# everything after @

# create 2 new columns using the resulting DataFrame
customer_df = customer_df.assign(
	username = split_email[0],
	domain = split_email[1],
	)
	
```
**Example 2 of transform using PySpark**
1. The extract phase needs to load the table into spark and use JDBC, JDBC is a piece of software that helps Spark connect with several sql databases
2. Put the autorization information in the properties aRGUMENT
3. Write the name of the table in the second argument
```Py
import pyspark.sql

spark = pyspark.sql.SparkSession.builder.getOrCreate()

spark.read.jdbc("jdbc:postgresql://localhost:5432/pagila",#jdbc 
	'customer',#name of the table
	properties = {
		'user':'repl',
		'password':'password'})#autorization information
``` 

**Example 3 join using PySpark**
```Py
import pyspark.sql

customer_df # PySpark DataFrame with customer data
ratings_df # PySpark Dataframe with ratings data

#Groupby ratings
ratings_per_customer = ratings_df.groupBy('customer_id').mean('rating')

#Join on customer ID
customer_df.join(
	ratings_per_customer,
	customer_df.customer_id == ratings_per_customer.customer_id)
``` 
**Exercise 1: Splitting the rental rate**

In the video exercise, you saw how to use pandas to split the email address column of the film table in order to extract the users' domain names. Suppose you would want to have a better understanding of the rates users pay for movies, so you decided to divide the rental_rate column into dollars and cents.

In this exercise, you will use the same techniques used in the video exercises to do just that! The film table has been loaded into the pandas DataFrame film_df. Remember, the goal is to split up the rental_rate column into dollars and cents.

Instructions

- Use the .astype() method to convert the rental_rate column into a column of string objects, and assign the results to rental_rate_str.
- Split rental_rate_str on '.' and expand the results into columns. Assign the results to rental_rate_expanded.
- Assign the newly created columns into films_df using the column names rental_rate_dollar and rental_rate_cents respectively, setting them to the expanded version using the appropriate index.
```Py 
# Get the rental rate column as a string
rental_rate_str = film_df.rental_rate.astype("str")

# Split up and expand the column
rental_rate_expanded = rental_rate_str.str.split(".", expand=True)

# Assign the columns to film_df
film_df = film_df.assign(
    rental_rate_dollar=rental_rate_expanded[0],
    rental_rate_cents=rental_rate_expanded[1]
)
``` 
**Exercise 2: Joining with ratings**

In the video exercise, you saw how to use transformations in PySpark by joining the film and ratings tables to create a new column that stores the average rating per customer. In this exercise, you're going to create more synergies between the film and ratings tables by using the same techniques you learned in the video exercise to calculate the average rating for every film.

The PySpark DataFrame with films, film_df and the PySpark DataFrame with ratings, rating_df, are available in your workspace.

Instructions

- Take the mean rating per film_id, and assign the result to ratings_per_film_df.
- Complete the .join() statement to join on the film_id column.
- Show the first 5 results of the resulting DataFrame.
```Py
# Use groupBy and mean to aggregate the column
ratings_per_film_df = rating_df.groupBy('film_id').mean('rating')

# Join the tables using the film_id column
film_df_with_ratings = film_df.join(
    ratings_per_film_df,
    film_df.film_id==ratings_per_film_df.film_id
)

# Show the 5 first results
print(film_df_with_ratings.show(5))
``` 

### LOAD

#### Analytics Databases
- Complex aggregate queries 
- OLAP

#### Applications Databases
- Lots of transactiones per second
- OLTP

#### Row oriented database
- Store data per record
- Addes per transaction 

#### Column Oriente database
- Store data per column
- Queries: small subsets of columns in a table
- lend themselves better for parallelization 

#### MPP Databases (Massively Prallel Processing Databases)
- They are often the objective at the end of a ETL proces
- Column oriented databases
- Optimized for analytics
- Run in a distributed fashion, queries are not executed in a single compute node
- Examples: Amazon Redshift, Azure SQL Data Warehouse, Google BigQuery

CSV files are not a good option for this pporpuse
- We often use a format call Parquet, there are several packages that hel to writ ethis kind of files

**An example Redshift**
```Py
# Pandas .to_parquet() method
df.to_parquet(".hjdcbhsbstring")

# PySpark .write.parquet() method
df.write.parquet("saknjncjscnstring")
``` 
you can conect to Redshift using a PostgreeSQL conection URL and copy the data from S3 into Redshift like this
```SQL
COPY customer 
FROM 'skassasstring'
FORMAT as parquet
``` 
** Example of how to load into PostgreeSQL **
```Py
# Transformation on data
recommendations = transform_find_recommendations(ratings_df)

# Load into PostgreSQL database
recommendations.to_sql(
			'recommendations',
			db_engine,
			schema = 'store',
			if_exists = 'replace')
``` 

**Exercise 1: Writing to a file**

In the video, you saw that files are often loaded into a MPP database like Redshift in order to make it available for analysis.

The typical workflow is to write the data into columnar data files. These data files are then uploaded to a storage system and from there, they can be copied into the data warehouse. In case of Amazon Redshift, the storage system would be S3, for example.

The first step is to write a file to the right format. For this exercises you'll choose the Apache Parquet file format.

There's a PySpark DataFrame called film_sdf and a pandas DataFrame called film_pdf in your workspace.

Instructions

- Write the pandas DataFrame film_pdf to a parquet file called "films_pdf.parquet".
- Write the PySpark DataFrame film_sdf to a parquet file called "films_sdf.parquet".
```Py
# Write the pandas DataFrame to parquet
film_pdf.to_parquet("films_pdf.parquet")

# Write the PySpark DataFrame to parquet
film_sdf.write.parquet("films_sdf.parquet")
```

**Exercise 2: Load Into Posgres**

In this exercise, you'll write out some data to a PostgreSQL data warehouse. That could be useful when you have a result of some transformations, and you want to use it in an application.

For example, the result of a transformation could have added a column with film recommendations, and you want to use them in your online store.

There's a pandas DataFrame called film_pdf in your workspace.

As a reminder, here's the structure of a connection URI for sqlalchemy:
```Py
'postgresql://[user[:password]@][host][:port][/database]'
```
Instructions

- Complete the connection URI for to create the database engine. The user and password are repl and password respectively. The host is localhost, and the port is 5432. - This time, the database is dwh.
- Finish the call so we use the "store" schema in the database. If the table exists, replace it completely.

```Py
# Finish the connection URI
connection_uri = "postgresql://repl:password@localhost:5432/dwh"
db_engine_dwh = sqlalchemy.create_engine(connection_uri)

# Transformation step, join with recommendations data
film_pdf_joined = film_pdf.join(recommendations)

# Finish the .to_sql() call to write to store.film
film_pdf_joined.to_sql("film", db_engine_dwh, schema="store", if_exists="replace")

# Run the query to fetch the data
pd.read_sql("SELECT film_id, recommended_film_ids FROM store.film", db_engine_dwh)
```
### Putting It All Together

#### The ETL Function

```Py

def extract_table_to_df(tablename, db_engine):
	return pd.read_sql('SELECT * FROM {}'.format(tablename), db_engine)

def split_columns_transform(df, column, pat, suffixes):
	#converts column into str and splits it on pat...

def load_df_into_dwh(film_df, tablename, schema, db_engine):
	return pd.to_sql(tablename, db_engine, schema = schema, if_exists = 'replace')

db_engines = { ... } #Needs to be configured
def etl():
	#extract
	film_df = extract_table_to_df('film', db_engines['store'])
	# transform
	film_df = split_columns_transform(film_df, 'rental_rate', '.', ['_dollar', '_cents'])
	#load
	load_df_into_dwh(film_df, 'film', 'store', db_engines['dwh'])	
```

#### Arflow Refresher
We need to be sure that this function runs at a specific time 

**Scheduling with DAGs in Airflow**
```Py
from airflow.models import DAG 

dag = DAG(
	dag_id = 'sample',
	...,
	schedule_interval = '0 0 * * *') # cron, runs every 0th minute in the hour
'''
 cron example
 .------------------------------- minute           (0-59)
 | .----------------------------- hour             (0-23)
 | | .--------------------------- day of the month (1-31)
 | | | .------------------------- month            (1-12)
 | | | | .----------------------- day of the week  (0-6)
 * * * * * <command>

Example
0 * * * * # Every hour at the 0th minute
'''
```
[More information about cron](https://crontab.guru)

Having creating the DAG is timme to set the ETL into motion

```Py
from airflow.models import DAG 
from airflow.operators.python_operator import PythonOperator # we are going to use a Python operator function 

dag = DAG(
	dag_id = 'sample',
	...,
	schedule_interval = '0 0 * * *') # cron, runs every 0th minute in the hour
	
# the python operator expects a callable in this case is the function we defined before
# it also expects two other parameters, task_id and dag, these are standar for all operathors
etl_task = PythonOperator(
	task_id = 'etl_task', # the identifier of this task
	python_callable = etl,
	dag = dag) # and the DAG it belogs to

etl_task.set_upstream(wait_for_this_task)
```
- we set upstream or downstream dependencies between tasks
- we can use set_upstream or set_downstream

- with upstream the etl_task will run after wait_for_this_task is completed

- Now we can write it into a Python file and place it in the DAG folder
- of Airflow
- the service detects the DAG and shows it in the interface

**Exercise 1: Defining a DAG**

In the previous exercises you applied the three steps in the ETL process:

- Extract: Extract the film PostgreSQL table into pandas.
- Transform: Split the rental_rate column of the film DataFrame.
- Load: Load a the film DataFrame into a PostgreSQL data warehouse.
- The functions extract_film_to_pandas(), transform_rental_rate() and load_dataframe_to_film() are defined in your workspace. In this exercise, you'll add an ETL task to an existing DAG. The DAG to extend and the task to wait for are defined in your workspace are defined as dag and wait_for_table respectively.

Instructions

- Complete the etl() function by making use of the functions defined in the exercise description.
- Make sure etl_task uses the etl callable.
- Set up the correct upstream dependency. Note that etl_task should wait for wait_for_table to be finished.
- The sample code contains a sample run. This means the ETL pipeline runs when you run the code.
```Py
# Define the ETL function
def etl():
    film_df = extract_film_to_pandas()
    film_df = transform_rental_rate(film_df)
    load_dataframe_to_film(film_df)

# Define the ETL task using PythonOperator
etl_task = PythonOperator(task_id='etl_film',
                          python_callable=etl,
                          dag=dag)

# Set the upstream to wait_for_table and sample run etl()
etl_task.set_upstream(wait_for_table)
etl()
```
Nicely done! Be sure to experiment with a few queries once the data pipeline has run. For example:
```Py
pd.read_sql('SELECT rating, AVG(rental_duration) FROM film GROUP BY rating ORDER BY AVG', db_engine)
```

**Exercise 2: Setting up Airflow**

In this exercise, you'll learn how to add a DAG to Airflow. To the right, you have a terminal at your disposal. The workspace comes with Airflow pre-configured, but it's [ easy to install on your own.](https://airflow.apache.org/start.html)

You'll need to move the *dag.py* file containing the DAG you defined in the previous exercise to, the DAGs folder. Here are the steps to find it:

The airflow home directory is defined in the ```AIRFLOW_HOME``` environment variable. Type ```echo $AIRFLOW_HOME``` to find out.
In this directory, find the ```airflow.cfg``` file. Use ```head``` to read the file, and find the value of the ```dags_folder```.
Now you can find the folder and move the ```dag.py``` file there: ```mv ./dag.py <dags_folder>```.

Which files does the DAGs folder have after you moved the file?

Instructions                                                            
- It has two DAG files: dag.py and dag_recommendations.py.

### Curse Ratings

**Course Table columns**
- course_id
- title
- description
- programming_language

**Rating Table Columns**
- user_id
- course_id *Is a foreign kkey to the courses table*
- rating *one to five start rating*

**Exercise 1: Querying the table**
Now that you have a grasp of what's happening in the datacamp_application database, let's go ahead and write up a query for that database.

The goal is to get a feeling for the data in this exercise. You'll get the rating data for three sample users and then use a predefined helper function, print_user_comparison(), to compare the sets of course ids these users rated.

Instructions

- Complete the connection URI. The database is called datacamp_application. The host is localhost with port 5432. The username is repl and the password is password.
- Select the ratings of users with id: 4387, 18163 and 8770.
- Fill in print_user_comparison() with the three users you selected.
```Py
# Complete the connection URI
connection_uri = "postgresql://repl:password@localhost:5432/datacamp_application" 
db_engine = sqlalchemy.create_engine(connection_uri)

# Get user with id 4387
user1 = pd.read_sql("SELECT * FROM rating WHERE user_id = 4387", db_engine)

# Get user with id 18163
user2 = pd.read_sql("SELECT * FROM rating WHERE user_id = 18163", db_engine)

# Get user with id 8770
user3 = pd.read_sql("SELECT * FROM rating WHERE user_id = 8770", db_engine)

# Use the helper function to compare the 3 users
print_user_comparison(user1, user2, user3)
```


**Exercise 2: Average rating per course**

A great way to recommend courses is to recommend top-rated courses, as DataCamp students often like courses that are highly rated by their peers.

In this exercise, you'll complete a transformation function transform_avg_rating() that aggregates the rating data using the pandas DataFrame's .groupby() method. The goal is to get a DataFrame with two columns, a course id and its average rating:
|___________________________|

|course_id   |	avg_rating  |

|123	     |	4.72	    |

|111	     |	4.62        |

|…	     |	…	    |

|___________________________|

In this exercise, you'll complete this transformation function, and apply it on raw rating data extracted via the helper function extract_rating_data() which extracts course ratings from the rating table.

Instructions

- Complete the transform_avg_rating() function by grouping by the course_id column, and taking the mean of the rating column.
- Use extract_rating_data() to extract raw ratings data. It takes in as argument the database engine db_engines.
- Use transform_avg_rating() on the raw rating data you've extracted.
```Py
# Complete the transformation function
def transform_avg_rating(rating_data):
    # Group by course_id and extract average rating per course
    avg_rating = rating_data.groupby('course_id').rating.mean()
    # Return sorted average ratings per course
    sort_rating = avg_rating.sort_values(ascending=False).reset_index()
    return sort_rating

# Extract the rating data into a DataFrame    
rating_data = extract_rating_data(db_engines)

# Use transform_avg_rating on the extracted data and print results
avg_rating_data = transform_avg_rating(rating_data)
print(avg_rating_data)
```
### From ratings to recommendations
