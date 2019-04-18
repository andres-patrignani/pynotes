
# Pandas

Module about the most popular Python library to handle tabular data or time series. The library is powerful and extensive, so here I will cover some of the basic operation to help you loading some data.

The core concepts of the **Pandas** library is that you can call columns containing the same datatype by names. This allows you to handle the data without having to remember the exact location of a column. The catch is that each column has to contain data of the same type.

The most popular module of the **Pandas** library is probably *DataFrames*

Built on top of numpy. It can also handle missing values.



```python
# First let's import the library
import pandas as pd

```


```python
# Load file
root = '/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/'
df = pd.read_csv(root + 'gypsum_ks_daily_2018.csv')

```


```python
# Head function
df.head(3)
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
      <th>TIMESTAMP</th>
      <th>STATION</th>
      <th>PRESSUREAVG</th>
      <th>PRESSUREMAX</th>
      <th>PRESSUREMIN</th>
      <th>SLPAVG</th>
      <th>TEMP2MAVG</th>
      <th>TEMP2MMIN</th>
      <th>TEMP2MMAX</th>
      <th>TEMP10MAVG</th>
      <th>...</th>
      <th>SOILTMP10MAX</th>
      <th>SOILTMP10MIN</th>
      <th>SOILTMP5AVG655</th>
      <th>SOILTMP10AVG655</th>
      <th>SOILTMP20AVG655</th>
      <th>SOILTMP50AVG655</th>
      <th>VWC5CM</th>
      <th>VWC10CM</th>
      <th>VWC20CM</th>
      <th>VWC50CM</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1/1/18 0:00</td>
      <td>Gypsum</td>
      <td>99.44</td>
      <td>100.03</td>
      <td>98.73</td>
      <td>104.44</td>
      <td>-15.15</td>
      <td>-19.56</td>
      <td>-11.00</td>
      <td>-15.31</td>
      <td>...</td>
      <td>-1.18</td>
      <td>-2.45</td>
      <td>-1.33</td>
      <td>-1.14</td>
      <td>0.74</td>
      <td>3.50</td>
      <td>0.1377</td>
      <td>0.1167</td>
      <td>0.2665</td>
      <td>0.2203</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1/2/18 0:00</td>
      <td>Gypsum</td>
      <td>99.79</td>
      <td>100.14</td>
      <td>99.40</td>
      <td>104.88</td>
      <td>-16.48</td>
      <td>-22.10</td>
      <td>-10.40</td>
      <td>-16.38</td>
      <td>...</td>
      <td>-1.56</td>
      <td>-3.46</td>
      <td>-2.10</td>
      <td>-1.82</td>
      <td>0.28</td>
      <td>3.13</td>
      <td>0.1234</td>
      <td>0.1021</td>
      <td>0.2642</td>
      <td>0.2196</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1/3/18 0:00</td>
      <td>Gypsum</td>
      <td>98.87</td>
      <td>99.52</td>
      <td>97.94</td>
      <td>103.81</td>
      <td>-11.03</td>
      <td>-20.64</td>
      <td>-2.71</td>
      <td>-10.66</td>
      <td>...</td>
      <td>-1.49</td>
      <td>-3.61</td>
      <td>-2.21</td>
      <td>-1.93</td>
      <td>-0.08</td>
      <td>2.76</td>
      <td>0.1206</td>
      <td>0.0965</td>
      <td>0.2353</td>
      <td>0.2189</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 44 columns</p>
</div>




```python
# Print names of each column
df.columns
```




    Index(['TIMESTAMP', 'STATION', 'PRESSUREAVG', 'PRESSUREMAX', 'PRESSUREMIN',
           'SLPAVG', 'TEMP2MAVG', 'TEMP2MMIN', 'TEMP2MMAX', 'TEMP10MAVG',
           'TEMP10MMIN', 'TEMP10MMAX', 'RELHUM2MAVG', 'RELHUM2MMAX', 'RELHUM2MMIN',
           'RELHUM10MAVG', 'RELHUM10MMAX', 'RELHUM10MMIN', 'VPDEFAVG', 'PRECIP',
           'SRAVG', 'SR', 'WSPD2MAVG', 'WSPD2MMAX', 'WSPD10MAVG', 'WSPD10MMAX',
           'WDIR2M', 'WDIR2MSTD', 'WDIR10M', 'WDIR10MSTD', 'SOILTMP5AVG',
           'SOILTMP5MAX', 'SOILTMP5MIN', 'SOILTMP10AVG', 'SOILTMP10MAX',
           'SOILTMP10MIN', 'SOILTMP5AVG655', 'SOILTMP10AVG655', 'SOILTMP20AVG655',
           'SOILTMP50AVG655', 'VWC5CM', 'VWC10CM', 'VWC20CM', 'VWC50CM'],
          dtype='object')




```python
# Check data types of each column
df.dtypes # Why does windDirection appear as an object?
```




    TIMESTAMP           object
    STATION             object
    PRESSUREAVG        float64
    PRESSUREMAX        float64
    PRESSUREMIN        float64
    SLPAVG             float64
    TEMP2MAVG          float64
    TEMP2MMIN          float64
    TEMP2MMAX          float64
    TEMP10MAVG         float64
    TEMP10MMIN         float64
    TEMP10MMAX         float64
    RELHUM2MAVG        float64
    RELHUM2MMAX        float64
    RELHUM2MMIN        float64
    RELHUM10MAVG       float64
    RELHUM10MMAX       float64
    RELHUM10MMIN       float64
    VPDEFAVG           float64
    PRECIP             float64
    SRAVG              float64
    SR                 float64
    WSPD2MAVG          float64
    WSPD2MMAX          float64
    WSPD10MAVG         float64
    WSPD10MMAX         float64
    WDIR2M             float64
    WDIR2MSTD          float64
    WDIR10M            float64
    WDIR10MSTD         float64
    SOILTMP5AVG        float64
    SOILTMP5MAX        float64
    SOILTMP5MIN        float64
    SOILTMP10AVG       float64
    SOILTMP10MAX       float64
    SOILTMP10MIN       float64
    SOILTMP5AVG655     float64
    SOILTMP10AVG655    float64
    SOILTMP20AVG655    float64
    SOILTMP50AVG655    float64
    VWC5CM             float64
    VWC10CM            float64
    VWC20CM            float64
    VWC50CM            float64
    dtype: object




```python
# Extract a column into a separate variable
windSpeed = df.windSpeed
windSpeed
```




    0       2.2
    1       3.2
    2       2.7
    3       4.5
    4       1.8
    5   -9999.0
    Name: windSpeed, dtype: float64




```python
# Alternative way of calling column data
df['windSpeed'] # Notice that there is no 'dot' after 'df'
```




    0       2.2
    1       3.2
    2   -9999.0
    Name: windSpeed, dtype: float64




```python
# Count number of records for each variable
df.count()
```




    doy              3
    windSpeed        3
    windDirection    3
    precipitation    3
    dtype: int64




```python
# Stats
print(df.windSpeed.max())
print(df.windSpeed.min())
print(df.precipitation.cumsum())
print(df.windDirection.unique())
```

    3.2
    -9999.0
    0     0
    1    18
    2    43
    Name: precipitation, dtype: int64
    ['E' 'NW']



```python
df['windSpeed'].is
```

**Dissecting the dataframe above we find the following main components**:
1. header row containing column names
2. index (the left-most column with numbers from 0 to 4) is equivalent to a rwo name.
3. Each column has data of the same type. In this case *doy* is an integer, *windSpeed* is a float, and *windDirection* is a string


```python
# Slicing: Select portions of data by calling specific rows, columns, or both
# Slicing follows this convention in Pandas DataFrames

# df[rows]
# df[columns]
# df[rows,columns]
# Notice that if you want to pass more than one row of column you will need to group them in a list
```


```python
# Slicing by rows
# df[rows]

df[0:3]
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
      <th>doy</th>
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
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.7</td>
      <td>N</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Slicing by columns
# df[columns]

df[['windSpeed','windDirection']]

# A common mistake when slicing multiple columns is to forget grouping column names into a list
# So, the following will not work:

# df['windSpeed','windDirection']
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
      <td>2.7</td>
      <td>N</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.5</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.8</td>
      <td>SW</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-9999.0</td>
      <td>SW</td>
    </tr>
  </tbody>
</table>
</div>



# Slicing using both rows and columns

`loc` gets rows (or columns) with particular labels from the index.

`iloc` gets rows (or columns) at particular positions in the index (so it only takes integers).



```python
df.iloc[0:3,[1,2]] # Exclusive of its endpoint
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
      <td>2.7</td>
      <td>N</td>
    </tr>
  </tbody>
</table>
</div>




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
      <td>2.7</td>
      <td>N</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.loc[0:2]
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
      <th>doy</th>
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
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.7</td>
      <td>N</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[0:2] # Exclusive of its endpoint
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
      <th>doy</th>
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
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python

# Using location
df.loc[0:2,'windSpeed']
df.loc[0:2,['windSpeed','windDirection']]


# These statements will not work
#df.loc[0:2,0:1]
#df.loc[[0:2],[0:1]]
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
      <td>2.7</td>
      <td>N</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Filtering

# Select days of the year in which the wind speed was greater than 3 meters per second.
idx = df.windSpeed > 3
idx  # Let's inspect the idx variable. This is a boolean variable (either True or False)

```




    0    False
    1     True
    2    False
    3     True
    4    False
    5    False
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
      <th>doy</th>
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
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.5</td>
      <td>S</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# We can also apply the boolean variable to specific columns
# If we want to know what was the wind direction on days with wind speed greater than 3 m/s
df.loc[idx,'windDirection']

# Here a common mistake would be to do:
# df[idx,'windDirection']
```




    1    NW
    3     S
    Name: windDirection, dtype: object




```python
# Same but in one line of code. This is fine to do, sometimes (as the next example) it can get complex
# and hard to follow, so I recommend storing the boolean results in a new variable that is easier to pass
# around. This is particularly helpful if you are planning to re-use the boolean in multiple 
# lines of you code.

df.loc[df.windSpeed > 3,'windDirection']
```




    1    NW
    3     S
    Name: windDirection, dtype: object




```python
# Another popular way of filtering is to check whether an element or group of elements are within a set.
# If you come from Matlab the following example is similar to the ismember function

# Let's check whether January 1 and January 2 are in the dataframe.
idx_doy = df['doy'].isin([1,2])  #list(range(1,5))
idx_doy
```




    0     True
    1     True
    2    False
    3    False
    4    False
    5    False
    Name: doy, dtype: bool




```python
# Select and display all columns for the selected days of the year
df.loc[idx_doy,:]
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
      <th>doy</th>
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
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Replace missing values. In this particular case I assumed that missing values are not NaN or NA in the
# original dataset. So, tot ake advantage of the available machinery for missing values in Pandas we
# first need to convert missing value placeholders (such as -9999) into NA or NaN. I typically prefer
# to use NaN (personal preference since I also use Matlab frequently).

# In many datasets missing data may already by imported as NaN or NA, meaning that you can directly use
# the fillna() function within this intermediary step.

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
      <th>doy</th>
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
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5</th>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[idx_missing] = np.nan
df
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-52-3a68f6deed8f> in <module>()
    ----> 1 df[idx_missing] = np.nan
          2 df


    NameError: name 'np' is not defined



```python
df.isna() # Note that the method 'isna' works for NA and for NaN
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
      <th>doy</th>
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
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Replace missing values with average values. This is not the best option, but at least it shows you how
# to replace missing values with a calculation on-the-fly.
# In this case the mean command calculates the average ignoring missing values 
# (as long as they are NaN or NA). A missing value represented as -9999 will not be skipped.

df.fillna(df.windSpeed.mean()) # Replace missing vwind speed alues with average wind speed values.

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
      <th>doy</th>
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
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.7</td>
      <td>N</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.5</td>
      <td>S</td>
      <td>18</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1.8</td>
      <td>SW</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>-9999.0</td>
      <td>SW</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create Pandas dates
dates = pd.date_range('20190101', periods=6)
dates
```




    DatetimeIndex(['2019-01-01', '2019-01-02', '2019-01-03', '2019-01-04',
                   '2019-01-05', '2019-01-06'],
                  dtype='datetime64[ns]', freq='D')




```python
# Add dates as a new column
df['dates'] = dates
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
      <th>doy</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>dates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>2019-01-01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>0</td>
      <td>2019-01-02</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.7</td>
      <td>N</td>
      <td>0</td>
      <td>2019-01-03</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.5</td>
      <td>S</td>
      <td>18</td>
      <td>2019-01-04</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1.8</td>
      <td>SW</td>
      <td>25</td>
      <td>2019-01-05</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>-9999.0</td>
      <td>SW</td>
      <td>1</td>
      <td>2019-01-06</td>
    </tr>
  </tbody>
</table>
</div>




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
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>dates</th>
    </tr>
    <tr>
      <th>doy</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>2019-01-01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.2</td>
      <td>NW</td>
      <td>0</td>
      <td>2019-01-02</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.7</td>
      <td>N</td>
      <td>0</td>
      <td>2019-01-03</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.5</td>
      <td>S</td>
      <td>18</td>
      <td>2019-01-04</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.8</td>
      <td>SW</td>
      <td>25</td>
      <td>2019-01-05</td>
    </tr>
    <tr>
      <th>6</th>
      <td>-9999.0</td>
      <td>SW</td>
      <td>1</td>
      <td>2019-01-06</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Reset the index
df.reset_index(0)  # Note that the old index is added as a new column
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
      <th>doy</th>
      <th>windSpeed</th>
      <th>windDirection</th>
      <th>precipitation</th>
      <th>dates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
      <td>2.2</td>
      <td>E</td>
      <td>0</td>
      <td>2019-01-01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2</td>
      <td>3.2</td>
      <td>NW</td>
      <td>0</td>
      <td>2019-01-02</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>3</td>
      <td>2.7</td>
      <td>N</td>
      <td>0</td>
      <td>2019-01-03</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>4</td>
      <td>4.5</td>
      <td>S</td>
      <td>18</td>
      <td>2019-01-04</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>5</td>
      <td>1.8</td>
      <td>SW</td>
      <td>25</td>
      <td>2019-01-05</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>6</td>
      <td>-9999.0</td>
      <td>SW</td>
      <td>1</td>
      <td>2019-01-06</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
