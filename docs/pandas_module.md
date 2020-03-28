# Pandas Module

## Introduction

Pandas is built on top of Numpy and is the most popular Python library for handling tabular data. The library is powerful and extensive, so here we will cover some of the basic features. The most popular module of the **Pandas** library is probably *DataFrames*. We will learn how to create a daframe, retrieve data, replace missing values, and compute simple statistics.

The concept of the **Pandas** library is that we can call data stored in rows or columns by name. This allows us to handle data without having to remember the exact location of a column.

Pandas DataFrame excels at loading tabular data from comma-separated value files (.csv) and text files (.txt). Files typically have a single line of column headers and each column has the same data type.



```python
# Import modules
import pandas as pd
import numpy as np

```

## Create DataFrame from existing variable

After importing the module we have two possible directions. We import data from a file or we convert an existing variable into a Pandas DataFrame. Here we will create a simple DatFrame to learn the basics. This way we will be able to display the result of our operations without worrying about extensive datasets.

Let's create a dictionary with some weather data and missing values (represented by `-9999`).


```python
# Create dictionary with some weather data
data = {'timestamp': ['1/1/2000','2/1/2000','3/1/2000','4/1/2000','5/1/2000'], 
        'windSpeed': [2.2, 3.2, -9999.0, 4.1, 2.9], 
        'windDirection': ['E', 'NW', 'NW', 'N', 'S'],
        'precipitation': [0, 18, 25, 2, 0]}

```

The next step consists of converting the dictionary into a Pandas DataFrame. This is straight forward using the DataFrame method of the Pandas modules `pd.DataFrame()`


```python
# Convert dictionary into DataFrame
df = pd.DataFrame(data)
df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1/1/2000</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2/1/2000</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3/1/2000</td>
      <td>-9999.0</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4/1/2000</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5/1/2000</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



>To display the DataFrame content simply use the `head()` and `tail()` methods.
As an alternative you can use the `print()` function or type the name of the DataFrame and hit `ctrl + Enter`. Note that by default Jupyter Lab highlights the different table rows when using the second option, so for readability purposes I will use the second option from now on.

Dissecting the DataFrame above we find the following main components:

1. header row containing column names

2. index (the left-most column with numbers from 0 to 4) is equivalent to a row name.

3. Each column has data of the same type.

## Basic methods and properties

Pandas DataFrame has dedicated functions to display a limited number of heading and tailing rows.


```python
df.head(3) # First three rows
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1/1/2000</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2/1/2000</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3/1/2000</td>
      <td>-9999.0</td>
      <td>NW</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.tail(3) # Last three rows
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>3/1/2000</td>
      <td>-9999.0</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4/1/2000</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5/1/2000</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



To start handling our data we need to learn how to retrieve data from the Pandas DataFrame. If we don't know the column names of the dataset we can print them using the column property. We can also inspect the data type of each column as well as its total number of elements and shape.


```python
df.columns  # Column names
```




    Index(['timestamp', 'windSpeed', 'windDirection', 'precipitation'], dtype='object')




```python
df.size  # Total number of elements
```




    20




```python
df.shape  # Number of rows and columns
```




    (5, 4)




```python
df.dtypes  # Data type for each column
```




    timestamp         object
    windSpeed        float64
    windDirection     object
    precipitation      int64
    dtype: object



## Convert strings to datetime


```python
# Convert dates in string format to Pandas datetime format
# %d = day in format 00 days
# %m = month in format 00 months
# %Y = full year

df["timestamp"] = pd.to_datetime(df["timestamp"], format="%d/%m/%Y")
df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>-9999.0</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-04</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-05</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Note that the format of our `timestamp` column change to datetime format
df.dtypes

```




    timestamp        datetime64[ns]
    windSpeed               float64
    windDirection            object
    precipitation             int64
    dtype: object



## Extract information from the timestamp

Here we can obtain the day, month, year, and even other components such hours, minutes and nanoseconds fromt he timestamp. Having a separate column for some of these components can be extremely helpful in case we want to aggregate data. For instnance, to compute the monthly mean air temperature we need to know in what month each temperature record was obtained.

For this we will use the `dt` submodule within `Pandas`.



```python
# Get the day of the year
df["doy"] = df["timestamp"].dt.dayofyear
df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>-9999.0</td>
      <td>NW</td>
      <td>25</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-04</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-05</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



>Note that the new column was placed at the end. This the default when creating a new column.

The next example makes use of the `insert()` method to add the new column on a specific location. Typically for dates and date components we want to have the columns at the beginning, close to the datetime. For other variables the previous approach that appends the new column at the end of the DataFrame will work just fine.



```python
# Get month from timstamp and create new column

#.insert(positionOfNewColumn, nameOfNewColumn, dataOfNewColumn)

df.insert(1,'month',df["timestamp"].dt.month)
df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>1</td>
      <td>-9999.0</td>
      <td>NW</td>
      <td>25</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-04</td>
      <td>1</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-05</td>
      <td>1</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



>If you re-run the previous cell you will get an error since Pandas prevents having two columns with the same name.

## Missing values

One of the most common operations when working with data is to handle missing values. Almost every dataset has missing data and there is no universal way of denoting missing values. Most common placeholders are: `NaN`, `NA`, `-99`, `-9999`, `missing`. To find out more about how missing data is represented in your dataset read associated documentation files.

I typically prefer to use `NaN` (personal preference since I also use Matlab frequently). In many datasets missing data may already by imported as NaN or NA, meaning that you can directly use the `fillna()` method without this intermediary step.

To replace missing values we will follow these steps:

1. Identify the cells with `-9999` values. Output will be a logical DataFrame having the same dimensions as `df`.

2. Replace `-9999` with `NaN` values. Note that I'm using `NaN` values from **Numpy**.

3. Check our work using the `isna()` method (optional)

> Pandas offers a machinery to deal with missing data, meaning that it is not necessary to replace these values in order to make computations with the data. We just need to ensure that missing values are in the right format. In some cases an option would be to replace missing data with an estimate value, like using the average of the values immediately above and below or using the average for the entire data (not ideal)using the `fillna()` method. For instance: `df.fillna(df.windSpeed.mean())`



```python
# Step 1: find -9999 values across the entire dataframe

idx_missing = df.isin([-9999])
idx_missing

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Find missing vlaues in only one column
df["windSpeed"] == -9999

```




    0    False
    1    False
    2     True
    3    False
    4    False
    Name: windSpeed, dtype: bool




```python
# Step 2: Replace missing values with NaN

df[idx_missing] = np.nan
df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-04</td>
      <td>1</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-05</td>
      <td>1</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
# NaNs are of type float
type(np.nan)

```




    float




```python
# Step 3: Check our work
df.isna()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



## Quick statistics

DataFrames have a variety of methods to calculate simple statistics. To obtain an overall summary we can use the `describe()` method.


```python
# Summary stats for all columns
print(df.describe())

```

           month  windSpeed  precipitation       doy
    count    5.0   4.000000         5.0000  5.000000
    mean     1.0   3.100000         9.0000  3.000000
    std      0.0   0.787401        11.7047  1.581139
    min      1.0   2.200000         0.0000  1.000000
    25%      1.0   2.725000         0.0000  2.000000
    50%      1.0   3.050000         2.0000  3.000000
    75%      1.0   3.425000        18.0000  4.000000
    max      1.0   4.100000        25.0000  5.000000



```python
# Metric ignoring NaN values
print(df["windSpeed"].max())         # Maximum value for each column
print(df["windSpeed"].mean())        # Average value for each column
print(df["windSpeed"].min())         # Minimum value for each column
print(df["windSpeed"].std())         # Standard deviation value for each column
print(df["windSpeed"].var())         # Variance value for each column
print(df["windSpeed"].median())      # Variance value for each column
print(df["windSpeed"].quantile(0.95))

```

    4.1
    3.1
    2.2
    0.7874007874011809
    0.6199999999999997
    3.05
    3.9649999999999994



```python
# Cumulative sum. Useful to compute cumulative precipitation
print(df.precipitation.cumsum())

```

    0     0
    1    18
    2    43
    3    45
    4    45
    Name: precipitation, dtype: int64



```python
# Unique values. Useful to compute unique wind directions
print(df.windDirection.unique())

```

    ['E' 'NW' 'N' 'S']


## Indexing and slicing


### Using index operator `[]`
To start making computations with need to access the data insde the Pandas DataFrame. Indexing and slicing are useful operations to select portions of data by calling specific rows, columns, or a combination of both. The index operator `[]` is primarily intended to be used with column labels (e.g. `df[columnName]`), however, it can also handle row slices (e.g. `df[rows]`). A common notation useful to understand how the slicing works is as follows:

## Select rows


```python
df[0:3] # First three rows
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



## Select columns

We can call individual columns using the `dot` or `bracket` notation. Note that in option 2 there is no `.` between `df` and `['windSpeed']`


```python
df.windSpeed    # Option 1
df['windSpeed'] # Option 2
```




    0    2.2
    1    3.2
    2    NaN
    3    4.1
    4    2.9
    Name: windSpeed, dtype: float64



To pass more than one row of column you will need to group them in a list.


```python
# Select multiple columns at once
df[['windSpeed','windDirection']]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>windSpeed</th>
      <th>windDirection</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.2</td>
      <td>E</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.2</td>
      <td>NW</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NW</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.1</td>
      <td>N</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2.9</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



A common mistake when slicing multiple columns is to forget grouping column names into a list, so the following command will not work:

`df['windSpeed','windDirection']`

## Slicing rows and columns

### Using `iloc` method

`iloc`: Integer-location. `iloc` gets rows (or columns) at specific indexes. It only takes integers as input. **Exclusive of its endpoint**


```python
# Top 3 rows and columns 1 and 2
df.iloc[0:3,[1,2]]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>month</th>
      <th>windSpeed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>3.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top 2 rows and all columns
df.iloc[0:2,:] # Same as: df.iloc[0:2]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



Although a bit more verbose and perhaps less *pythonic*, I prefer to specify `all the columns` using the `:` character. In my opinion this notation is more explicit and clearly states the rows and columns of the slicing operation. So, `df.iloc[0:2,:]` is preferred over `df.iloc[0:2]`.

### Using `loc` method

`loc` gets rows (or columns) with specific labels. **Inclusive of its endpoint**


```python
# Select multiple rows and columns at once using the loc method
df.loc[0:2,['windSpeed','windDirection']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>windSpeed</th>
      <th>windDirection</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.2</td>
      <td>E</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.2</td>
      <td>NW</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NW</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Some rows and all columns
df.loc[0:1,:]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# First three elements of a single column
df.loc[0:2,'windSpeed']  

```




    0    2.2
    1    3.2
    2    NaN
    Name: windSpeed, dtype: float64




```python
# First three elements of multiple columns
df.loc[0:2,['windSpeed','windDirection']]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>windSpeed</th>
      <th>windDirection</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.2</td>
      <td>E</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.2</td>
      <td>NW</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NW</td>
    </tr>
  </tbody>
</table>
</div>



These statements will not work with `loc`: 

`df.loc[0:2,0:1]`

`df.loc[[0:2],[0:1]]`

## Filter data using boolean indexing

Boolean indexing (a.k.a. logical indexing) consists of creating a boolean array with True/False values as a consequence of conditional statement that can be use to select the rows that meet the specified condition. Logical indexing are often not intuitive for students learning how to program, but are incredibly powerful and I highly encourage its use.

Let's select all the data for days that have wind speed greater than 3 meters per second. We will first select the rows of `df.windSpeed` that are greater than 3 m/s, and then we will use the resulting boolean to slice the DataFrame.


```python
idx = df.windSpeed > 3 # Rows in which the wind speed is greater than 
idx  # Let's inspect the idx variable.

```




    0    False
    1     True
    2    False
    3     True
    4    False
    Name: windSpeed, dtype: bool




```python
# Now let's apply the boolean variable to the dataframe
df[idx]

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-04</td>
      <td>1</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
# We can also apply the boolean to specific columns
df.loc[idx,'windDirection']

```




    1    NW
    3     N
    Name: windDirection, dtype: object



It's also possible to write the previous command with the conditional statement in a single line of code. This is fine to do, but sometimes nesting too many conditions can create commands that are hard to understand. I typically recommend storing the boolean results in a new variable that is easier to pass around. This is particularly helpful if you are planning to re-use the boolean in multiple lines of you code.


```python
# Same in a single line of code
df.loc[df.windSpeed > 3,'windDirection']

```




    1    NW
    3     N
    Name: windDirection, dtype: object



Another popular way of filtering is to check whether an element or group of elements are within a set. If you come from Matlab the following example is similar to the `ismember()` function.

Let's check whether January 1 and January 2 are in the dataframe.


```python
idx_doy = df['doy'].isin([1,2])  #list(range(1,5))
idx_doy
```




    0     True
    1     True
    2    False
    3    False
    4    False
    Name: doy, dtype: bool




```python
df.loc[idx_doy,:] # Select all columns for the selected days of the year
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



## Pandas custom date range

In this particular case we have the day of the year to indicate time, however, in many occasions is better to handle dates. Often times dates are already present in the dataset, but if they aren't then we can create and handle dates with Pandas.


```python
subset_dates = pd.date_range('20000102', periods=2, freq='D') # Used df.shape[0] to find the total number of rows
subset_dates
```




    DatetimeIndex(['2000-01-02', '2000-01-03'], dtype='datetime64[ns]', freq='D')




```python
# The same to generate months
pd.date_range('20200101', periods=df.shape[0], freq='M') # Specify the frequency to months

```




    DatetimeIndex(['2020-01-31', '2020-02-29', '2020-03-31', '2020-04-30',
                   '2020-05-31'],
                  dtype='datetime64[ns]', freq='M')



## Select range of dates with boolean indexing

Now that we covered both boolean indexing and pandas dates we can use these concepts to select data from a specific window of time. This is a pretty common operation when trying to select a subset of the entire DataFrame by a specific date range.



```python
# Generate boolean for rows that match the subset of dates generated earlier
idx_subset = df["timestamp"].isin(subset_dates)
idx_subset

```




    0    False
    1     True
    2     True
    3    False
    4    False
    Name: timestamp, dtype: bool




```python
# Generate a new DataFrame using only the rows with matching dates
df_subset = df.loc[idx_subset]
df_subset

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# It isn't always necessary to generate a new DataFrame
# So you can access a specific column like this
df.loc[idx_subset,"precipitation"]

```




    1    18
    2    25
    Name: precipitation, dtype: int64



## Add and remove columns

The `insert()` and `drop()` methods allow us to add or remove columns to/from the DataFrame. The most common use of these functions is as follows:

`df.insert(indexOfNewColumn, nameOfNewColumn, dataArrayOfNewColumn)`

`df.drop(nameOfColumnToBeRemoved)`


```python
# Add new column at a specific location
df.insert(2, 'airTemperature', [25.4, 26, 27.1, 28.9, 30.2]) # Similar to: df['dates'] = dates
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>airTemperature</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1</td>
      <td>25.4</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>26.0</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>1</td>
      <td>27.1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-04</td>
      <td>1</td>
      <td>28.9</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-05</td>
      <td>1</td>
      <td>30.2</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Remove specific column
df.drop(columns=['airTemperature'])

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-04</td>
      <td>1</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-05</td>
      <td>1</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



## Reset DataFrame index


```python
# Replace the index by a variables of our choice
df.set_index('doy')

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>airTemperature</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
    <tr>
      <th>doy</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2000-01-01</td>
      <td>1</td>
      <td>25.4</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-02</td>
      <td>1</td>
      <td>26.0</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-03</td>
      <td>1</td>
      <td>27.1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-04</td>
      <td>1</td>
      <td>28.9</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2000-01-05</td>
      <td>1</td>
      <td>30.2</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Reset the index (see that 'doy' goes back to the end of the DataFrame again)
df.reset_index(0)

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>timestamp</th>
      <th>month</th>
      <th>airTemperature</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>2000-01-01</td>
      <td>1</td>
      <td>25.4</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2000-01-02</td>
      <td>1</td>
      <td>26.0</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2000-01-03</td>
      <td>1</td>
      <td>27.1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>2000-01-04</td>
      <td>1</td>
      <td>28.9</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>2000-01-05</td>
      <td>1</td>
      <td>30.2</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



## Merge two dataframes


```python
# Create a new DataFrame (follows dates)

# Dictionary
data2 = {'timestamp': ['6/1/2000','7/1/2000','8/1/2000','9/1/2000','10/1/2000'], 
        'windSpeed': [4.3, 2.1, 0.5, 2.7, 1.9], 
        'windDirection': ['N', 'N', 'SW', 'E', 'NW'],
        'precipitation': [0, 0, 0, 25, 0]}

# Dcitionary to DataFrame
df2 = pd.DataFrame(data2)

# Convert strings to pandas datetime
df2["timestamp"] = pd.to_datetime(df2["timestamp"], format="%d/%m/%Y") 

df2.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-06</td>
      <td>4.3</td>
      <td>N</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-07</td>
      <td>2.1</td>
      <td>N</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-08</td>
      <td>0.5</td>
      <td>SW</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-09</td>
      <td>2.7</td>
      <td>E</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-10</td>
      <td>1.9</td>
      <td>NW</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



>Not using the `format="%d/%m/%y"` in the previous cell results in the wrong datetime conversion. It is always recommended to specify the format.


```python
df_merged = df.append(df2)
df_merged

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>airTemperature</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1.0</td>
      <td>25.4</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1.0</td>
      <td>26.0</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>1.0</td>
      <td>27.1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-04</td>
      <td>1.0</td>
      <td>28.9</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-05</td>
      <td>1.0</td>
      <td>30.2</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>0</th>
      <td>2000-01-06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.3</td>
      <td>N</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.1</td>
      <td>N</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-08</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.5</td>
      <td>SW</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-09</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.7</td>
      <td>E</td>
      <td>25</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.9</td>
      <td>NW</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



>Note how `NaN` values were assigned to variables not present in the new DataFrame


```python
# Create another DataFrame with more limited data. Values every other day
data3 = {'timestamp': ['1/1/2000','3/1/2000','5/1/2000','7/1/2000','9/1/2000'], 
         'pressure': [980, 987, 985, 991, 990]}  # Pressure in millibars
df3 = pd.DataFrame(data3)
df3["timestamp"] = pd.to_datetime(df3["timestamp"], format="%d/%m/%Y")
df3.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>pressure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>980</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-03</td>
      <td>987</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-05</td>
      <td>985</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-07</td>
      <td>991</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-09</td>
      <td>990</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Only the matching rows will be merged
df_merged.merge(df3,on="timestamp")

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>airTemperature</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
      <th>pressure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1.0</td>
      <td>25.4</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1.0</td>
      <td>980</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-03</td>
      <td>1.0</td>
      <td>27.1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
      <td>3.0</td>
      <td>987</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-05</td>
      <td>1.0</td>
      <td>30.2</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
      <td>5.0</td>
      <td>985</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.1</td>
      <td>N</td>
      <td>0</td>
      <td>NaN</td>
      <td>991</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-09</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.7</td>
      <td>E</td>
      <td>25</td>
      <td>NaN</td>
      <td>990</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Only add values from the new, more sporadic, variable where there is a match.
df_merged.merge(df3, how="left")

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>timestamp</th>
      <th>month</th>
      <th>airTemperature</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>doy</th>
      <th>pressure</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000-01-01</td>
      <td>1.0</td>
      <td>25.4</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>1.0</td>
      <td>980.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000-01-02</td>
      <td>1.0</td>
      <td>26.0</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000-01-03</td>
      <td>1.0</td>
      <td>27.1</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
      <td>3.0</td>
      <td>987.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000-01-04</td>
      <td>1.0</td>
      <td>28.9</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
      <td>4.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000-01-05</td>
      <td>1.0</td>
      <td>30.2</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
      <td>5.0</td>
      <td>985.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2000-01-06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.3</td>
      <td>N</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2000-01-07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.1</td>
      <td>N</td>
      <td>0</td>
      <td>NaN</td>
      <td>991.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2000-01-08</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.5</td>
      <td>SW</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2000-01-09</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.7</td>
      <td>E</td>
      <td>25</td>
      <td>NaN</td>
      <td>990.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2000-01-10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.9</td>
      <td>NW</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Pandas <-> Numpy


```python
# Get numpy values from Pandas series
df["windSpeed"].values

```




    array([2.2, 3.2, nan, 4.1, 2.9])




```python
# Confirm the data type is Numpy array
print(type(df["windSpeed"].values))

```

    <class 'numpy.ndarray'>



```python
# For reproducibility
np.random.seed(1) 

# Create some synthetic data
dates = pd.date_range('20200101', periods=365*3, freq='D') # 3 years of synthetic data
y = np.sin((x/365)*2*np.pi)  + np.random.randn(x.size)*0.2

```


```python
type(doy.values)
```




    numpy.ndarray




```python
# Convert data to Numpy array, where each variable is a column
A = np.array([dates,y])
A.shape

```




    (2, 1095)




```python
# Transpose data so that days are along the rows and the variables along the columns
A = np.transpose(A)
A.shape

```




    (1095, 2)




```python
# Create DataFrame
df4 = pd.DataFrame(A , columns=["dates","y"])
df4.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>dates</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-01-01</td>
      <td>0.342082</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-01-02</td>
      <td>-0.0879297</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-01-03</td>
      <td>-0.0540147</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-01-04</td>
      <td>-0.145791</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-01-05</td>
      <td>0.259046</td>
    </tr>
  </tbody>
</table>
</div>




```python
# To convert a single numpy array into Pandas series just use:
ypd = pd.Series(y)
print(type(ypd))

```

    <class 'pandas.core.series.Series'>



```python
import matplotlib.pyplot as plt

plt.figure(figsize=(12,4))
plt.plot(df4["dates"],df4["y"])
plt.ylabel('Random Y')
plt.show()

```


![png](pandas_module_files/pandas_module_94_0.png)



```python
# Moving average
ypd_smooth_forward = df4["y"].rolling(90).mean()
ypd_smooth_center = df4["y"].rolling(90, center=True).mean()

```

In case you are just using a Pandas series converted previously (`ypd`), you can perform the same computation as follows:
    
```python
ypd.rolling(90).mean()
```


```python
# Show differences in forward and centered moving averages
plt.figure(figsize=(12,4))
plt.plot(df4["dates"],df4["y"])

# Add the moving averages. We did not add the computed moving averages to our DataFrame,
# so we just need to call the variables
plt.plot(df4["dates"],ypd_smooth_forward, '--k',label='Forward moving average')
plt.plot(df4["dates"],ypd_smooth_center, '-r', label='Center moving average')
plt.legend(loc="lower left")
plt.show()

```


![png](pandas_module_files/pandas_module_97_0.png)


In this case, the selection of a forward or centered moving average affects the timing of maximum and minimum values of the wave. A centered moving average is ideal to preserve important features of the original signal in the resulting smoothed timeseries.

