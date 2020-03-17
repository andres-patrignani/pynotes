# Atmospheric Water Deficit

In environmental sciences many times we are concerned with soil water deficits that can affect vegetation growth and limit root growth, above ground biomass accumulation, and grain yield. Estimating soil water deficits can be hard, even if we use a model. A more simplistic approach consists of using observations of precipitation and potential evapotrasnpiration observations to approximate the state of the soil mositure conditions. Of course, this is just a metric based on atmopsheric variables, but both precipitation and potential evapotranspiration are strongly linked to soil moisture dynamics and in some cases might be enough for practical hydrological applications and climate characterization.



```python
# Import modules
import pandas as pd
import matplotlib.pyplot as plt

```


```python
# Load data for Hollis, OK as in Figure
data = pd.read_csv('../datasets/hollis_ok_precip_et.csv')
data.head()

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
      <th>date</th>
      <th>doy</th>
      <th>precip</th>
      <th>et</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2/28/1997</td>
      <td>59</td>
      <td>0.0</td>
      <td>1.57</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3/1/1997</td>
      <td>60</td>
      <td>0.0</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3/2/1997</td>
      <td>61</td>
      <td>0.0</td>
      <td>2.01</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3/3/1997</td>
      <td>62</td>
      <td>0.0</td>
      <td>4.34</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3/4/1997</td>
      <td>63</td>
      <td>0.0</td>
      <td>3.20</td>
    </tr>
  </tbody>
</table>
</div>



Before we proceed, let's convert the dates from `string` format to `datetime` format. This will allow us to easily generate plots involving dates with Matplotlib and it will also allow us to compute the day of the year.


```python
# Let's prove that the current format is string
type(data["date"][0])

# Convert to datetime using Pandas
data.date = pd.to_datetime(data["date"])

# Let's prove that the new format is indeed in datetime format
type(data["date"][0])

```




    pandas._libs.tslibs.timestamps.Timestamp



The next step constits of computing the cumulative sum of precipitation and evapotranspiration. In this case we will match the window of 15 days used in the reference manuscript by Torres et al., 2013.

We will set a `window of 7 days` and we will not make any computation for the first 15 values of the time series. this will prevent that we compute extremely low values of atmospheric water deficit due to lack of observations in the window. In other word, we will start computing the 15-day rolling sum after day number 15 (full window with data).


```python
# Compute cumulative precipitation
window = 7 # days
data["precip_cum"] = data["precip"].rolling(window).sum()

# Compute cumulative evapotranspiration
data["et_cum"] = data["et"].rolling(window).sum()
data.head(10)
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
      <th>date</th>
      <th>doy</th>
      <th>precip</th>
      <th>et</th>
      <th>precip_cum</th>
      <th>et_cum</th>
      <th>awd</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1997-02-28</td>
      <td>59</td>
      <td>0.0</td>
      <td>1.57</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1997-03-01</td>
      <td>60</td>
      <td>0.0</td>
      <td>2.24</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1997-03-02</td>
      <td>61</td>
      <td>0.0</td>
      <td>2.01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1997-03-03</td>
      <td>62</td>
      <td>0.0</td>
      <td>4.34</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1997-03-04</td>
      <td>63</td>
      <td>0.0</td>
      <td>3.20</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1997-03-05</td>
      <td>64</td>
      <td>0.0</td>
      <td>3.96</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1997-03-06</td>
      <td>65</td>
      <td>0.0</td>
      <td>3.80</td>
      <td>0.0</td>
      <td>21.12</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1997-03-07</td>
      <td>66</td>
      <td>0.0</td>
      <td>4.23</td>
      <td>0.0</td>
      <td>23.78</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1997-03-08</td>
      <td>67</td>
      <td>0.0</td>
      <td>0.91</td>
      <td>0.0</td>
      <td>22.45</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1997-03-09</td>
      <td>68</td>
      <td>0.0</td>
      <td>4.27</td>
      <td>0.0</td>
      <td>24.71</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Compute atmospheric water deficit
data["awd"] = data["et_cum"] - data["precip_cum"]

```


```python
# Compute the day of the year for each date
data["doy"] = data["date"].dt.dayofyear
data.head()
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
      <th>date</th>
      <th>doy</th>
      <th>precip</th>
      <th>et</th>
      <th>precip_cum</th>
      <th>et_cum</th>
      <th>awd</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1997-02-28</td>
      <td>59</td>
      <td>0.0</td>
      <td>1.57</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1997-03-01</td>
      <td>60</td>
      <td>0.0</td>
      <td>2.24</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1997-03-02</td>
      <td>61</td>
      <td>0.0</td>
      <td>2.01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1997-03-03</td>
      <td>62</td>
      <td>0.0</td>
      <td>4.34</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1997-03-04</td>
      <td>63</td>
      <td>0.0</td>
      <td>3.20</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# We do this simplify the following statements
grouped_awd_data = data["awd"].groupby(data["doy"]) # This is an object

# Compute median
mean_awd = grouped_awd_data.mean()

# Get groups (which are the unique days of the year)
doy_awd = list(grouped_awd_data.groups.keys())

```


```python
# Pitfall 1: Convert dictionary keys into list using square brackets
doy_awd_pitfall_1 = [ grouped_awd_data.groups.keys() ] 

# Print the variable to see the nesting of the keys: [dict_keys([1, 2, 3, 4, 5,

```


```python
# Pitfall 2: Get unique day of the year
doy_awd_pitfall_2 = data["doy"].unique()

# Show first few elements to illustrate the difference between the two appraoches
print('Aligned with grouped AWD data:',doy_awd[0:10])
print('Not aligned with grouped AWD data:',doy_awd_pitfall_2[0:10])
```

    Aligned with grouped AWD data: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    Not aligned with grouped AWD data: [59 60 61 62 63 64 65 66 67 68]



```python
# Plot current variables
plt.figure(figsize=(8,6))
plt.title("Hollis, OK", size=16)
plt.plot(doy_awd, mean_awd, '-o')
plt.xlabel('Day of Year', size=16)
plt.ylabel('Atmospheric water deficit (mm)', size=16)
plt.show()

```


![png](atmospheric_water_deficit_files/atmospheric_water_deficit_12_0.png)



```python
# Calculate probability of AWD > 50 mm
drought_threshold = 50
idx = data["awd"] > drought_threshold
awd_prob_50mm = idx.groupby(data["doy"]).sum() / grouped_awd_data.size()

```


```python
# Plot probability chart
plt.figure(figsize=(8,6))
plt.title("Hollis, OK", size=16)
plt.plot(doy_awd, awd_prob_50mm, '-o')
plt.xlabel('Day of Year', size=16)
plt.ylabel('Drought probability', size=16)
plt.show()

```


![png](atmospheric_water_deficit_files/atmospheric_water_deficit_14_0.png)


## References

Torres, G.M., Lollato, R.P. and Ochsner, T.E., 2013. Comparison of drought probability assessments based on atmospheric water deficit and soil water deficit. Agronomy Journal, 105(2), pp.428-436.
