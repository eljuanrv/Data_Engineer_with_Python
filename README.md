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

How to query a SQL Database Using Pandas
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

