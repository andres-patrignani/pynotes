# Statistics module

A simple module part of the Python Standard Library to handle basic statistical descriptive metrics. Most of these functions can be accessed through `Numpy` or `Pandas` library, but this module is still relevant when simple statistcs are required without the need for importing larger modules. 

For this exercise we will use data from FAO STATS about arable land in the US. The data set contains arable land data in millions of hectares from 1961 to 2016 for the USA.



```python
import pandas as pd
import matplotlib.pyplot as plt
import statistics as stats

```


```python
# Navigate to Datasets directory and load example file
df = pd.read_csv('../datasets/faostats_usa_arable_land.csv')
df.head(5)

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
      <th>Domain</th>
      <th>Area</th>
      <th>Variable</th>
      <th>Year</th>
      <th>Unit</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Land Use</td>
      <td>United States of America</td>
      <td>Arable land</td>
      <td>1961</td>
      <td>1000000 ha</td>
      <td>180.630</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Land Use</td>
      <td>United States of America</td>
      <td>Arable land</td>
      <td>1962</td>
      <td>1000000 ha</td>
      <td>177.095</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Land Use</td>
      <td>United States of America</td>
      <td>Arable land</td>
      <td>1963</td>
      <td>1000000 ha</td>
      <td>179.574</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Land Use</td>
      <td>United States of America</td>
      <td>Arable land</td>
      <td>1964</td>
      <td>1000000 ha</td>
      <td>177.966</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Land Use</td>
      <td>United States of America</td>
      <td>Arable land</td>
      <td>1965</td>
      <td>1000000 ha</td>
      <td>177.000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plot data
plt.figure()
plt.plot(df['Year'], df['Value'])
plt.ylabel('Arable land [million hectares]')
plt.show()

```


![png](statistics_module_files/statistics_module_3_0.png)



```python
# Average
print("Mean:",stats.mean(df["Value"]))

# Sample standard deviation of data.
print(stats.stdev(df["Value"]))

# Variance
print(stats.variance(df["Value"]))

# Median 50th percentile of data.
print(stats.median(df["Value"]))

# Low median of data.
print(stats.median_low(df["Value"]))

# High median of data.
print(stats.median_high(df["Value"]))

# Median, or 50th percentile, of grouped data.
print(stats.median_grouped(df["Value"]))

# Most frequent data
print(stats.mode(df["Value"]))

# Population standard deviation of data.
print(stats.pstdev(df["Value"]))

# Population variance of data.
print(stats.pvariance(df["Value"]))

```

    Mean: 177.05856785714286
    12.048125424089195
    145.15732623458445
    180.815
    180.63
    181.0
    180.5
    187.765
    11.940068304798451
    142.56523112325257


## References

Visit official documentation at: <https://docs.python.org/3.4/library/statistics.html>
