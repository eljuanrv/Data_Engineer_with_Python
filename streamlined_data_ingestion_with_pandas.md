# Streamlined Data Ingestion with pandas
**Course Description**

Before you can analyze data, you first have to acquire it. This course teaches you how to build pipelines to import data kept in common storage formats. You’ll use pandas, a major Python library for analytics, to get data
from a variety of sources, from spreadsheets of survey responses, to a database of public service requests, to an API for a popular review site. Along the way, you’ll learn how to fine-tune imports to get only what you need and to address issues like incorrect data types. Finally, you’ll assemble a custom dataset from a mix of sources.

## Importing Data from Flat Files
Practice using pandas to get just the data you want from flat files, learn how to wrangle data types and handle errors, and look into some U.S. tax data along the way.

### Dataframes
- *pandas* specific structure for two dimentional data (column and rows)

### Flat files
- Simple, easy to produce format
- data store as plain text
- one row per line
- values for deferent fields are separated by a delimiter, usually a **,** and such files are called csv

### Loading CSVS Files
- sample of us_tax_data_2016.csv

```Py
import pandas as pd #import the library

df=pd.read_csv(us_tax_data_2016.csv) # load the data into a dataframe

df.head() # analize the first 5 rows of the data

```

### Loading other flat files
- tab separated values
```Py
import pandas as pd #import the library

df=pd.read_csv('us_tax_data_2016.csv', sep='\t') # to indicate tab separated values we add a **\t**

df.head() # analize the first 5 rows of the data

```

### Excercises

**Get data from other flat files**

While CSVs are the most common kind of flat file, you will sometimes find files that use different delimiters. read_csv() can load all of these with the help of the sep keyword argument. By default, pandas assumes that the separator is a comma, which is why we do not need to specify sep for CSVs.

The version of Vermont tax data here is a tab-separated values file (TSV), so you will need to use sep to pass in the correct delimiter when reading the file. Remember that tabs are represented as \t. Once the file has been loaded, the remaining code groups the N1 field, which contains income range categories, to create a chart of tax returns by income category.

Instructions
- Import pandas with the alias pd.
- Load vt_tax_data_2016.tsv, making sure to set the correct delimiter with the sep keyword argument.

```Py
# Import pandas with the alias pd
import pandas as pd

# Load TSV using the sep keyword argument to set delimiter
data = pd.read_csv('vt_tax_data_2016.tsv', sep='\t')

# Plot the total number of tax returns by income group
counts = data.groupby("agi_stub").N1.sum()
counts.plot.bar()
plt.show()
```

### Modifying flat file import
```Py
# Import pandas with the alias pd
import pandas as pd

# Load TSV using the sep keyword argument to set delimiter
data = pd.read_csv('vt_tax_data_2016.tsv', sep='\t')

data.shape #shows the number of rows and columns
```
### Limiting Columns
- chose columns to load with the ```usecols``` keyword argument
- Acepts a list of column numbers or names, or a function to filter column names

```Py
col_names = ['STATEFIPS', 'STATE', 'zipcode', 'agi_stub', 'N1']
col_nums = [0,1,2,3,4]

#choose columns to load by name
tax_data_v1 = pd.read_csv('us_tax_data_2016.csv', usecols = col_names)

#choose columns to load by number
tax_data_v2 = pd.read_csv('us_tax_data_2016.csv', usecols = col_nums)

print(tax_data_v1.equals(tax_data_v2))

```





