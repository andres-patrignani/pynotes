# Understanding Soil Temperature

Soil temperature is an essential variable for the decision-making of agricultural operations (e.g. planting date, seeding depth, fertilizer application), construction (e.g. burial depth of pipes and cables), biology of underground insects and behaviour of poikilothermic animals, and climate modeling.

Unlike other soil variables, like soil moisture, soil temperature exhibits consistent and highly predictable patterns. In this exercise we will explore a dataset of hourly soil temperature for the city of Fargo, ND collected at 14 different soil depths from 5 to 225 cm depth. the dataset goes from October-2014 to October-2018. Longer timeseries are usually required for scientific studies, but this dataset will be sufficient to capture the major oscillation patterns in soil temperature.



```python
# Import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

```


```python
# Load soil temperature data
df = pd.read_csv("../datasets/fargo_hourly_deep_soil_temperature.csv")
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
      <th>station</th>
      <th>time_cst</th>
      <th>T5cm</th>
      <th>T10cm</th>
      <th>T20cm</th>
      <th>T30cm</th>
      <th>T40cm</th>
      <th>T50cm</th>
      <th>T60cm</th>
      <th>T80cm</th>
      <th>T100cm</th>
      <th>T125cm</th>
      <th>T150cm</th>
      <th>T175cm</th>
      <th>T200cm</th>
      <th>T225cm</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Fargo</td>
      <td>10/2/14 12:00</td>
      <td>18.09</td>
      <td>15.36</td>
      <td>14.49</td>
      <td>14.64</td>
      <td>14.71</td>
      <td>14.68</td>
      <td>14.60</td>
      <td>14.28</td>
      <td>13.96</td>
      <td>13.43</td>
      <td>12.92</td>
      <td>12.37</td>
      <td>11.66</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Fargo</td>
      <td>10/2/14 13:00</td>
      <td>18.96</td>
      <td>16.08</td>
      <td>14.70</td>
      <td>14.66</td>
      <td>14.72</td>
      <td>14.68</td>
      <td>14.60</td>
      <td>14.25</td>
      <td>13.92</td>
      <td>13.38</td>
      <td>12.86</td>
      <td>12.30</td>
      <td>11.66</td>
      <td>11.15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Fargo</td>
      <td>10/2/14 14:00</td>
      <td>18.59</td>
      <td>16.45</td>
      <td>14.89</td>
      <td>14.70</td>
      <td>14.72</td>
      <td>14.65</td>
      <td>14.56</td>
      <td>14.20</td>
      <td>13.85</td>
      <td>13.30</td>
      <td>12.78</td>
      <td>12.22</td>
      <td>11.65</td>
      <td>11.10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Fargo</td>
      <td>10/2/14 15:00</td>
      <td>19.82</td>
      <td>17.01</td>
      <td>15.10</td>
      <td>14.73</td>
      <td>14.71</td>
      <td>14.65</td>
      <td>14.54</td>
      <td>14.18</td>
      <td>13.80</td>
      <td>13.26</td>
      <td>12.71</td>
      <td>12.12</td>
      <td>11.65</td>
      <td>11.13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Fargo</td>
      <td>10/2/14 16:00</td>
      <td>19.09</td>
      <td>17.14</td>
      <td>15.24</td>
      <td>14.74</td>
      <td>14.68</td>
      <td>14.59</td>
      <td>14.49</td>
      <td>14.10</td>
      <td>13.73</td>
      <td>13.14</td>
      <td>12.60</td>
      <td>11.97</td>
      <td>11.64</td>
      <td>11.08</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Convert dates from string to Pandas datetime format
df["time_cst"] = pd.to_datetime(df["time_cst"])
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
      <th>station</th>
      <th>time_cst</th>
      <th>T5cm</th>
      <th>T10cm</th>
      <th>T20cm</th>
      <th>T30cm</th>
      <th>T40cm</th>
      <th>T50cm</th>
      <th>T60cm</th>
      <th>T80cm</th>
      <th>T100cm</th>
      <th>T125cm</th>
      <th>T150cm</th>
      <th>T175cm</th>
      <th>T200cm</th>
      <th>T225cm</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Fargo</td>
      <td>2014-10-02 12:00:00</td>
      <td>18.09</td>
      <td>15.36</td>
      <td>14.49</td>
      <td>14.64</td>
      <td>14.71</td>
      <td>14.68</td>
      <td>14.60</td>
      <td>14.28</td>
      <td>13.96</td>
      <td>13.43</td>
      <td>12.92</td>
      <td>12.37</td>
      <td>11.66</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Fargo</td>
      <td>2014-10-02 13:00:00</td>
      <td>18.96</td>
      <td>16.08</td>
      <td>14.70</td>
      <td>14.66</td>
      <td>14.72</td>
      <td>14.68</td>
      <td>14.60</td>
      <td>14.25</td>
      <td>13.92</td>
      <td>13.38</td>
      <td>12.86</td>
      <td>12.30</td>
      <td>11.66</td>
      <td>11.15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Fargo</td>
      <td>2014-10-02 14:00:00</td>
      <td>18.59</td>
      <td>16.45</td>
      <td>14.89</td>
      <td>14.70</td>
      <td>14.72</td>
      <td>14.65</td>
      <td>14.56</td>
      <td>14.20</td>
      <td>13.85</td>
      <td>13.30</td>
      <td>12.78</td>
      <td>12.22</td>
      <td>11.65</td>
      <td>11.10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Fargo</td>
      <td>2014-10-02 15:00:00</td>
      <td>19.82</td>
      <td>17.01</td>
      <td>15.10</td>
      <td>14.73</td>
      <td>14.71</td>
      <td>14.65</td>
      <td>14.54</td>
      <td>14.18</td>
      <td>13.80</td>
      <td>13.26</td>
      <td>12.71</td>
      <td>12.12</td>
      <td>11.65</td>
      <td>11.13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Fargo</td>
      <td>2014-10-02 16:00:00</td>
      <td>19.09</td>
      <td>17.14</td>
      <td>15.24</td>
      <td>14.74</td>
      <td>14.68</td>
      <td>14.59</td>
      <td>14.49</td>
      <td>14.10</td>
      <td>13.73</td>
      <td>13.14</td>
      <td>12.60</td>
      <td>11.97</td>
      <td>11.64</td>
      <td>11.08</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(10, 6))
plt.plot(df["time_cst"], df["T5cm"], "-k", label="5cm")
plt.axhline(df["T5cm"].mean(), linestyle="--", color="k")
plt.plot(df["time_cst"], df["T225cm"], "-r", label="225")
plt.axhline(df["T225cm"].mean(), linestyle="--", color="r")
plt.ylabel('Soil Temperature (Celsius)')
plt.show()

```


![png](soil_temperature_files/soil_temperature_4_0.png)



```python
# Get depths for plotting purposes
depths = [float(col[1:-2]) for col in df.columns[2:]]
depths = np.transpose(depths)

```


```python
# Compute mean and amplitude for each soil depth
means = []
amplitudes = []
doymax = []
doymin = []
window = 15

for depth in df.columns[2:]:
    # Mean
    means.append(df[depth].mean())
    
    # Amplitude
    Tmax = df[depth].rolling(window=window, center=True).max().max() # Max of rolling max.
    Tmin = df[depth].rolling(window=window, center=True).min().min() # Min of rolling min.
    A = (Tmax-Tmin)/2
    amplitudes.append(A)
    
    # Time of maximum temperature
    rowmax = df[depth].rolling(window=window, center=True).max().idxmax()
    doymax.append(df.loc[rowmax,"time_cst"].dayofyear)
    
    # Time of minumum temperature
    rowmin = df[depth].rolling(window=window, center=True).min().idxmin()
    doymin.append(df.loc[rowmin,"time_cst"].dayofyear)
    
```


```python
# The previous step can be done using two different approaches using Pandas and Numpy.
# I chose the Pandas approach to chain built-in methods and improve code readability
df["T5cm"].rolling(window=window, center=True).min().idxmin()
np.argmin(df["T5cm"].rolling(window=window, center=True).min())

```




    28766



## Mean annual temperature for each depth


```python
# Plot means
plt.plot(depths,means,'o-')
plt.xlabel("Soil depth (cm)")
plt.ylabel("Annual mean temperature (Celsius)")
plt.ylim(0,30)
plt.show()
```


![png](soil_temperature_files/soil_temperature_9_0.png)



```python
print("All annual temperatures are within:", round(np.max(means)-np.min(means),2) )

```

    All annual temperatures are within: 0.5


Although not identical, the annual mean temperature for all soil layers from 5 cm to 225 cm depth are remarkably similar, and they are all within half a degree Celsius! Thre is no other variable in soil science that behaves in such predictable way. This means that by knowing the average annual temperature at the soil surface we also know the soil temperature at all other layers. Not just that, we can also approximate the temperature of the water in well or water table. 

Our findings also support the assumption of the analytical solution of the heat conduction-diffusion equation that assumes that the average soil temperature at all depths is the same and that at "infinitely" deep soil layers the temperature approaches the mean annual temperature.


## Annual amplitude for each depth


```python
# Plot amplitudes
plt.plot(depths,amplitudes,'o-')
plt.xlabel("Soil depth (cm)")
plt.ylabel("Annual thermal amplitude (Celsius)")
plt.ylim(0,30)
plt.show()

```


![png](soil_temperature_files/soil_temperature_13_0.png)


Unlike the mean annual temperature, the annual amplitude decreases exponentially as a function of soil depth. This is somewhat expected since layers closer to the soil surface are exposed to environmental fluctuations that cause heating and cooling of the soil. Deeper soil layers are somewhat insulated from these fluctuations by the preceding soil layers.

## Occurrence of maximum and minimum temperature for each depth


```python
# Plot occurrence of maximum temperature
plt.plot(depths,doymax,'o-r', label="Tmax", alpha=0.5)
plt.plot(depths,doymin,'v-b', label="Tmin", alpha=0.5)
plt.xlabel("Soil depth (cm)")
plt.ylabel("Annual thermal amplitude (Celsius)")
plt.ylim(0,365)
plt.legend()
plt.show()

```


![png](soil_temperature_files/soil_temperature_16_0.png)


The occurrence of maximum and minimum annual temperatures exhibit a similar trend: Deeper layers experience a delayed occurrence of maximum and minimum temperatures compared to layers closer to the soil surface.

The coolest soil temperature at 225 cm depth occurs in the month of May, while the warmest soil temperature for the same layer occurs in October. At the soil surface, the minimum and maximum soil temperatures occur during January and July, as expected.


## Contour plot dynamics

A contour plot will allow us to synthesize the temporal changes in soil temprature as a function of depth in a single plot. We will reduce the dataset to daily averages to reduce the number of points we pass to the contour plot function, otherwise it would take too long on a regular laptop or desktop. 


```python
df_daily = df.groupby(df["time_cst"].dt.date).mean()
df_daily.head()

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
      <th>T5cm</th>
      <th>T10cm</th>
      <th>T20cm</th>
      <th>T30cm</th>
      <th>T40cm</th>
      <th>T50cm</th>
      <th>T60cm</th>
      <th>T80cm</th>
      <th>T100cm</th>
      <th>T125cm</th>
      <th>T150cm</th>
      <th>T175cm</th>
      <th>T200cm</th>
      <th>T225cm</th>
    </tr>
    <tr>
      <th>time_cst</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>2014-10-02</th>
      <td>16.137500</td>
      <td>15.681667</td>
      <td>15.006667</td>
      <td>14.746667</td>
      <td>14.672500</td>
      <td>14.585833</td>
      <td>14.476667</td>
      <td>14.137500</td>
      <td>13.759167</td>
      <td>13.209167</td>
      <td>12.685000</td>
      <td>12.135000</td>
      <td>11.634167</td>
      <td>11.072500</td>
    </tr>
    <tr>
      <th>2014-10-03</th>
      <td>9.898792</td>
      <td>11.684583</td>
      <td>13.440000</td>
      <td>14.202083</td>
      <td>14.450000</td>
      <td>14.456667</td>
      <td>14.384167</td>
      <td>14.143750</td>
      <td>13.787500</td>
      <td>13.267500</td>
      <td>12.748333</td>
      <td>12.210417</td>
      <td>11.688750</td>
      <td>11.134583</td>
    </tr>
    <tr>
      <th>2014-10-04</th>
      <td>8.322625</td>
      <td>9.880833</td>
      <td>11.760000</td>
      <td>12.974583</td>
      <td>13.666250</td>
      <td>13.978750</td>
      <td>14.111250</td>
      <td>14.054167</td>
      <td>13.752500</td>
      <td>13.260000</td>
      <td>12.754583</td>
      <td>12.217500</td>
      <td>11.707083</td>
      <td>11.155417</td>
    </tr>
    <tr>
      <th>2014-10-05</th>
      <td>8.636125</td>
      <td>9.656250</td>
      <td>11.123333</td>
      <td>12.224167</td>
      <td>12.961250</td>
      <td>13.394167</td>
      <td>13.680833</td>
      <td>13.827917</td>
      <td>13.637917</td>
      <td>13.198750</td>
      <td>12.711250</td>
      <td>12.178750</td>
      <td>11.702083</td>
      <td>11.160417</td>
    </tr>
    <tr>
      <th>2014-10-06</th>
      <td>8.801125</td>
      <td>9.616667</td>
      <td>10.785000</td>
      <td>11.742917</td>
      <td>12.453333</td>
      <td>12.922917</td>
      <td>13.280833</td>
      <td>13.577083</td>
      <td>13.508750</td>
      <td>13.156250</td>
      <td>12.707083</td>
      <td>12.195833</td>
      <td>11.702083</td>
      <td>11.162500</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Check that our daily computations make sense
plt.plot(df_daily["T5cm"])
plt.show()

```


![png](soil_temperature_files/soil_temperature_20_0.png)



```python
days = (df_daily.index - df_daily.index[0]).days
x = np.repeat(days.values,14)
print(x.shape)
x
```

    (20678,)





    array([   0,    0,    0, ..., 1476, 1476, 1476])




```python
y = np.tile(depths*-1,df_daily.shape[0])
print(y.shape)
y
```

    (20678,)





    array([  -5.,  -10.,  -20., ..., -175., -200., -225.])




```python
z = df_daily.values.flatten()
print(z.shape)
z
```

    (20678,)





    array([16.1375    , 15.68166667, 15.00666667, ..., 12.096     ,
           12.001     , 11.788     ])




```python
days[[0:-1:10]].values
```




    array([1, 3])




```python
plt.figure(figsize=(18,4))
plt.tricontour(x, y, z, levels=10, linewidths=0.5, colors='k')
plt.tricontourf(x, y, z, levels=10, cmap="RdBu_r")
plt.xticks(days[0:-1:30].values, labels=df_daily.index[0:-1:30], rotation=90)
plt.xticks(fontsize=14)
plt.colorbar(label="Soil temperature (Celsius)")
plt.ylabel('Soil depth (cm)', size=16)
plt.yticks(fontsize=16)
plt.show()
```


![png](soil_temperature_files/soil_temperature_25_0.png)


## References

North Dakota Agricultural Weather Network. Deep soil temperature data retrieved on October 2018. Link: https://ndawn.ndsu.nodak.edu/deep-soil-temperatures.html

Akyüz, F., Ewens, M., Carcoana, R. and Mullins, B., 2008. NWS Frost depth observation with liquid-in probes performance: Two-year review. Journal of Service Climatology, 2(2), pp.1-10.
