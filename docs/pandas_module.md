# Pandas Module

## Introduction

Pandas is built on top of Numpy and is the most popular Python library for handling tabular data. The library is powerful and extensive, so here we will cover some of the basic features. The most popular module of the **Pandas** library is probably *DataFrames*. We will learn how to create a daframe, retrieve data, replace missing values, and compute simple statistics.

The concept of the **Pandas** library is that we can call data stored in rows or columns by name. This allows us to handle data without having to remember the exact location of a column. The catch is that each column has to contain data of the same type.



```python
import pandas as pd
import numpy as np  # We will need some Numpy commands later on
```

## Create DataFrame from existing variable

After importing the module we have two possible directions. We import data from a file or we convert an existing variable into a Pandas DataFrame. Here we will create a simple DatFrame to learn the basics. This way we will be able to display the result of our operations without worrying about extensive datasets.

Let's create a dictionary with some weather data and missing values (represented by `-9999`).


```python
data = {'dayOfYear': [1,2,3,4,5], 
        'windSpeed': [2.2, 3.2, -9999.0, 4.1, 2.9], 
        'windDirection': ['E', 'NW', 'NW', 'N', 'S'],
        'precipitation': [0, 18, 25, 2, 0]}
```

The next step consists of converting the dictionary into a Pandas DataFrame. This is straight forward using the DataFrame method of the Pandas modules `pd.DataFrame()`


```python
df = pd.DataFrame(data)
```

## Load tabular data into a DataFrame

Pandas DataFrame excels at loading tabular data from comma-separated value files (.csv) and text files (.txt). Files typically have a single line of column headers and each column has the same data type.


```python
df = pd.read_csv('example_weather_data.csv')
```

In this case we created a DataFrame using an existing variable (perhaps as the result of a preceding computation in your code) and from a file containing comma-separated values. For the same dataset, these are two mutually exclusive alternatives. Most times we just want to load data stored in a `.csv` file, but just in case I wanted to show you how we can create a DataFrame from an existing variable.

## Display DataFrame

To display the DataFrame content simply use the print command or type the name of the DataFrame and hit `ctrl + Enter`. Note that by default Jupyter Lab highlights the different table rows when using the second option, so for readability purposes I will use the second option from now on.


```python
print(df)
df
```

       dayOfYear  windSpeed windDirection  precipitation
    0          1        2.2             E              0
    1          2        3.2            NW             18
    2          3    -9999.0            NW             25
    3          4        4.1             N              2
    4          5        2.9             S              0





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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>-9999.0</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>-9999.0</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
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




    Index(['dayOfYear', 'windSpeed', 'windDirection', 'precipitation'], dtype='object')




```python
df.dtypes  # Data type for each column
```




    dayOfYear          int64
    windSpeed        float64
    windDirection     object
    precipitation      int64
    dtype: object




```python
df.size  # Total number of elements
```




    20




```python
df.shape  # Number of rows and columns
```




    (5, 4)



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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
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
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
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
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
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
    </tr>
  </tbody>
</table>
</div>



## Quick statistics

DataFrames have a variety of methods to calculate simple statistics. To obtain an overall summary we can use the `describe()` method.


```python
print(df.describe())              # Summary stats for all columns
```

           dayOfYear  windSpeed  precipitation
    count   5.000000   4.000000         5.0000
    mean    3.000000   3.100000         9.0000
    std     1.581139   0.787401        11.7047
    min     1.000000   2.200000         0.0000
    25%     2.000000   2.725000         0.0000
    50%     3.000000   3.050000         2.0000
    75%     4.000000   3.425000        18.0000
    max     5.000000   4.100000        25.0000



```python
# Metric ignoring NaN values
print(df["windSpeed"].max())         # Maximum value for each column
print(df["windSpeed"].mean())        # Average value for each column
print(df["windSpeed"].min())         # Minimum value for each column
print(df["windSpeed"].std())         # Standard deviation value for each column
print(df["windSpeed"].var())         # Variance value for each column
print(df["windSpeed"].median())         # Variance value for each column
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
print(df.precipitation.cumsum())  # Cumulative sum. Useful to compute cumulative precipitation
```

    0     0
    1    18
    2    43
    3    45
    4    45
    Name: precipitation, dtype: int64



```python
print(df.windDirection.unique())  # Unique values. Useful to compute unique wind directions
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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>



Although a bit more verbose and perhaps less *pythonic*, I prefer to specify `all the columns` using the `:` character. In my opinion this notation is more explicit and clearly states the rows and columns of the slicing operation. So, `df.iloc[0:2,:]` is preferred over `df.iloc[0:2]`.

### Using `loc` method

`loc` gets rows (or columns) with specific labels. **Inclusive of its endpoint**


```python
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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc[0:2,'windSpeed']  # First three elements of a single column
df.loc[0:2,['windSpeed','windDirection']] # First three elements of multiple columns
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
df[idx] # Now let's apply the boolean variable to the dataframe
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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc[idx,'windDirection'] # We can also apply the boolean to specific columns
```




    1    NW
    3     N
    Name: windDirection, dtype: object



It's also possible to write the previous command with the conditional statement in a single line of code. This is fine to do, but sometimes nesting too many conditions can create commands that are hard to understand. I typically recommend storing the boolean results in a new variable that is easier to pass around. This is particularly helpful if you are planning to re-use the boolean in multiple lines of you code.


```python
df.loc[df.windSpeed > 3,'windDirection'] # Same in a single line of code
```




    1    NW
    3     N
    Name: windDirection, dtype: object



Another popular way of filtering is to check whether an element or group of elements are within a set. If you come from Matlab the following example is similar to the `ismember()` function.

Let's check whether January 1 and January 2 are in the dataframe.


```python
idx_doy = df['dayOfYear'].isin([1,2])  #list(range(1,5))
idx_doy
```




    0     True
    1     True
    2    False
    3    False
    4    False
    Name: dayOfYear, dtype: bool




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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>



## Pandas dates

In this particular case we have the day of the year to indicate time, however, in many occasions is better to handle dates. Often times dates are already present in the dataset, but if they aren't then we can create and handle dates with Pandas.


```python
dates = pd.date_range('20200101', periods=df.shape[0]) # Used df.shape[0] to find the total number of rows
dates
```




    DatetimeIndex(['2020-01-01', '2020-01-02', '2020-01-03', '2020-01-04',
                   '2020-01-05'],
                  dtype='datetime64[ns]', freq='D')




```python
pd.date_range('20200101', periods=df.shape[0], freq='m') # Specify the frequency to months
```




    DatetimeIndex(['2020-01-31', '2020-02-29', '2020-03-31', '2020-04-30',
                   '2020-05-31'],
                  dtype='datetime64[ns]', freq='M')



## Add and remove columns

The `insert()` and `drop()` methods allow us to add or remove columns to/from the DataFrame. The most common use of these functions is as follows:

`df.insert(indexOfNewColumn, nameOfNewColumn, dataArrayOfNewColumn)`

`df.drop(nameOfColumnToBeRemoved)`


```python
df.insert(0, 'dates', dates) # Similar to: df['dates'] = dates
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
      <th>dates</th>
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-01-01</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-01-02</td>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-01-03</td>
      <td>3</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-01-04</td>
      <td>4</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-01-05</td>
      <td>5</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.drop(columns=['dates'])
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
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Reset DataFrame index


```python
# Replace the index by a variables of our choice
df.set_index('dayOfYear')
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
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
    <tr>
      <th>dayOfYear</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2020-01-01</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-01-02</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-01-03</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-01-04</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2020-01-05</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Reset the index
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
      <th>dates</th>
      <th>dayOfYear</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>2020-01-01</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2020-01-02</td>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2020-01-03</td>
      <td>3</td>
      <td>NaN</td>
      <td>NW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>2020-01-04</td>
      <td>4</td>
      <td>4.1</td>
      <td>N</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>2020-01-05</td>
      <td>5</td>
      <td>2.9</td>
      <td>S</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Pandas to Numpy


```python
# Convert a Pandas dataframe into a numpy array
df["windSpeed"].values
```




    array([2.2, 3.2, nan, 4.1, 2.9])


