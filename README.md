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


