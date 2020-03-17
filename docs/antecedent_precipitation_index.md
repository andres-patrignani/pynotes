# Antecedent Precipitation Index

The API is a well-known, parsimonious, recursive model for predicting soil moisture solely based on precipitation records. The API is most commonly implemented using daily precipitation records, but it is possible to work at finer temporal scales if both precipitation (model input) and soil moisture (for validation purposes) are available. The equation describing the simples version of the model is:

$$API_{t} = \gamma \; API_{t-1} + P_t$$


$API_t$: Soil water content at time $t$ (today)

$API_{t-1}$: Soil water content at time $t-1$ (yesterday)

$\gamma$: Loss coefficient. Range between 0 and 1

$P_t$: Precipitation at time $t$ (today)

In its simplest version, the API has only one parameter, the loss parameter, which is often represented by $\gamma$. The loss coefficient modulates the rate at which soil moisture decreases over time. The loss coefficient is a lumped parameter that accounts for the combined losses due to run-off, drainage, evaporation, transpiration, and any other factor contributing to the loss of soil water from the soil profile under study. 

The API is a recursive model that requires knolwedge of the **initial conditions** or initial state of the soil (i.e. the amount of soil water at the beginning of the simulation). The initial state of the soil can be obtained from observations, it can also be approximated based on knowledge of recent rainfall events, or simply guessed. A convenient feature of the API is that it has the same units as precipitation (e.g. millimeters, inches). 


In the simplest version of the model, the loss coefficient can be considered constant, but this assumption often leads to under/over soil water estimation since the amount of soil water that is lost each day is subtantially different along the year. An improved version would be to capture seasonal changes using a proxy variable such as air temperature, vapor pressure deficit, or even something as simple as the day of the year. The latter alternative will not capture intra-seasonal variability and will also ignore the effects of the soil and actively growing vegetation, but it should improve the model compared to a constant value. The following model describes a time variant loss coefficient:

$$\gamma = C + A \sin \bigg(\omega t  + \phi\bigg) $$

$\gamma$ = Loss coefficient. Dimnesionless. Fraction of soil water remaining from the previous time step. For instance, if we are working with daily data and $\gamma$ is 0.95, this means that today we have in the soil 95% of the water that we had yesterday. In other words, we are losing 5% of the soil water per day. Note that this a fracton from the previous time step, meaning that the loss is becoming smaller with each time step becuse the previous time step is also becoming smaller.

$C$ = Annual average value for the loss coefficient

$A$ = Annual amplitude for the loss coefficient. We will assume that the wave is symmetric and that the highest value of the wave is around 1, which means that there is no water loss (today's soil water is equal to yesterday's soil water). A value of $\gamma = 1$ would typically occur around the day of the lowest atmopsheric demand (e.g. coldest day of the year). Now we rarely reach a perfect value of 1. So, in this example we will use 0.99, meaning that we are assuming a minimum loss of 1%.

$t$ = Day of the year

$\omega$ = Angular frequency = $2 \pi f$

$f$ = Frequency = $\frac{DOY}{Period}$

$Period$ = 365 days

$\phi$ = Phase or shift constant = $\frac{\pi}{2} + \omega t_o$. The term $\frac{\pi}{2}$ is added to align the sine wave, so that $t_o$ has a clear physical meaning. You can change this term to match $t_o$ with the day of highest atmospheric demand rather than the lowest.

$t_o$ = Day of the year of largest $\gamma$ value (lowest loss, day of lowest atmospheric demand)




```python
# Import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit
```

## Load data

For this example we will use data from the Bedford station, IN obtained from the US Climate Reference Network. The data spans the period 3-Oct-2007 to 24-Oct-2017. There is no particular reason why we are exploring this dataset and this period. It's just some data that I gather and compiled from the [USCRN website](https://www.ncdc.noaa.gov/crn/).


```python
# Load data from .CSV file
df = pd.read_csv('../datasets/IN_Bedford_5_WNW.txt')

```


```python
# Let's examine the first few entries
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
      <th>WBANNO</th>
      <th>LST_DATE</th>
      <th>CRX_VN</th>
      <th>LONGITUDE</th>
      <th>LATITUDE</th>
      <th>T_DAILY_MAX</th>
      <th>T_DAILY_MIN</th>
      <th>T_DAILY_MEAN</th>
      <th>T_DAILY_AVG</th>
      <th>P_DAILY_CALC</th>
      <th>...</th>
      <th>SOIL_MOISTURE_5_DAILY</th>
      <th>SOIL_MOISTURE_10_DAILY</th>
      <th>SOIL_MOISTURE_20_DAILY</th>
      <th>SOIL_MOISTURE_50_DAILY</th>
      <th>SOIL_MOISTURE_100_DAILY</th>
      <th>SOIL_TEMP_5_DAILY</th>
      <th>SOIL_TEMP_10_DAILY</th>
      <th>SOIL_TEMP_20_DAILY</th>
      <th>SOIL_TEMP_50_DAILY</th>
      <th>SOIL_TEMP_100_DAILY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63898</td>
      <td>20071003</td>
      <td>1.302</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>63898</td>
      <td>20071004</td>
      <td>1.302</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>28.9</td>
      <td>14.8</td>
      <td>21.9</td>
      <td>21.9</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>63898</td>
      <td>20071005</td>
      <td>1.302</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>29.3</td>
      <td>19.0</td>
      <td>24.2</td>
      <td>23.5</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 28 columns</p>
</div>




```python
# Let's examine the last few values of the dataset
df.tail(3)

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
      <th>WBANNO</th>
      <th>LST_DATE</th>
      <th>CRX_VN</th>
      <th>LONGITUDE</th>
      <th>LATITUDE</th>
      <th>T_DAILY_MAX</th>
      <th>T_DAILY_MIN</th>
      <th>T_DAILY_MEAN</th>
      <th>T_DAILY_AVG</th>
      <th>P_DAILY_CALC</th>
      <th>...</th>
      <th>SOIL_MOISTURE_5_DAILY</th>
      <th>SOIL_MOISTURE_10_DAILY</th>
      <th>SOIL_MOISTURE_20_DAILY</th>
      <th>SOIL_MOISTURE_50_DAILY</th>
      <th>SOIL_MOISTURE_100_DAILY</th>
      <th>SOIL_TEMP_5_DAILY</th>
      <th>SOIL_TEMP_10_DAILY</th>
      <th>SOIL_TEMP_20_DAILY</th>
      <th>SOIL_TEMP_50_DAILY</th>
      <th>SOIL_TEMP_100_DAILY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3652</th>
      <td>63898</td>
      <td>20171002</td>
      <td>2.422</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>26.2</td>
      <td>11.4</td>
      <td>18.8</td>
      <td>18.7</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.166</td>
      <td>0.199</td>
      <td>0.220</td>
      <td>0.461</td>
      <td>0.339</td>
      <td>17.3</td>
      <td>17.5</td>
      <td>17.6</td>
      <td>20.2</td>
      <td>19.2</td>
    </tr>
    <tr>
      <th>3653</th>
      <td>63898</td>
      <td>20171003</td>
      <td>2.422</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>27.4</td>
      <td>15.1</td>
      <td>21.2</td>
      <td>20.9</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.164</td>
      <td>0.198</td>
      <td>0.219</td>
      <td>0.461</td>
      <td>0.338</td>
      <td>18.3</td>
      <td>18.2</td>
      <td>18.0</td>
      <td>20.1</td>
      <td>19.1</td>
    </tr>
    <tr>
      <th>3654</th>
      <td>63898</td>
      <td>20171004</td>
      <td>2.422</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>26.8</td>
      <td>18.7</td>
      <td>22.8</td>
      <td>22.1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.163</td>
      <td>0.198</td>
      <td>0.219</td>
      <td>0.460</td>
      <td>0.335</td>
      <td>19.3</td>
      <td>19.1</td>
      <td>18.7</td>
      <td>20.2</td>
      <td>18.9</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 28 columns</p>
</div>




```python
# Replace missing values (-9999 and -99)
df = df.replace(-9999,np.nan)
df = df.replace(-99,np.nan)

# Replace NaN values in precipitation by zeros
df['P_DAILY_CALC'] = df['P_DAILY_CALC'].replace(np.nan,0)

# Print few lines to ensure changes make sense
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
      <th>WBANNO</th>
      <th>LST_DATE</th>
      <th>CRX_VN</th>
      <th>LONGITUDE</th>
      <th>LATITUDE</th>
      <th>T_DAILY_MAX</th>
      <th>T_DAILY_MIN</th>
      <th>T_DAILY_MEAN</th>
      <th>T_DAILY_AVG</th>
      <th>P_DAILY_CALC</th>
      <th>...</th>
      <th>SOIL_MOISTURE_5_DAILY</th>
      <th>SOIL_MOISTURE_10_DAILY</th>
      <th>SOIL_MOISTURE_20_DAILY</th>
      <th>SOIL_MOISTURE_50_DAILY</th>
      <th>SOIL_MOISTURE_100_DAILY</th>
      <th>SOIL_TEMP_5_DAILY</th>
      <th>SOIL_TEMP_10_DAILY</th>
      <th>SOIL_TEMP_20_DAILY</th>
      <th>SOIL_TEMP_50_DAILY</th>
      <th>SOIL_TEMP_100_DAILY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63898</td>
      <td>20071003</td>
      <td>1.302</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>63898</td>
      <td>20071004</td>
      <td>1.302</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>28.9</td>
      <td>14.8</td>
      <td>21.9</td>
      <td>21.9</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>63898</td>
      <td>20071005</td>
      <td>1.302</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>29.3</td>
      <td>19.0</td>
      <td>24.2</td>
      <td>23.5</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 28 columns</p>
</div>




```python
# Convert date to Pandas datetime format
df['LST_DATE'] = pd.to_datetime(df['LST_DATE'].apply(str))
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
      <th>WBANNO</th>
      <th>LST_DATE</th>
      <th>CRX_VN</th>
      <th>LONGITUDE</th>
      <th>LATITUDE</th>
      <th>T_DAILY_MAX</th>
      <th>T_DAILY_MIN</th>
      <th>T_DAILY_MEAN</th>
      <th>T_DAILY_AVG</th>
      <th>P_DAILY_CALC</th>
      <th>...</th>
      <th>SOIL_MOISTURE_5_DAILY</th>
      <th>SOIL_MOISTURE_10_DAILY</th>
      <th>SOIL_MOISTURE_20_DAILY</th>
      <th>SOIL_MOISTURE_50_DAILY</th>
      <th>SOIL_MOISTURE_100_DAILY</th>
      <th>SOIL_TEMP_5_DAILY</th>
      <th>SOIL_TEMP_10_DAILY</th>
      <th>SOIL_TEMP_20_DAILY</th>
      <th>SOIL_TEMP_50_DAILY</th>
      <th>SOIL_TEMP_100_DAILY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63898</td>
      <td>2007-10-03</td>
      <td>1.302</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>63898</td>
      <td>2007-10-04</td>
      <td>1.302</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>28.9</td>
      <td>14.8</td>
      <td>21.9</td>
      <td>21.9</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>63898</td>
      <td>2007-10-05</td>
      <td>1.302</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>29.3</td>
      <td>19.0</td>
      <td>24.2</td>
      <td>23.5</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 28 columns</p>
</div>




```python
# Plot and examine data before moving too far in the code
plt.figure(figsize=(8,6))
plt.plot(df['LST_DATE'],df['P_DAILY_CALC'])
plt.plot(df['LST_DATE'],df['SOIL_MOISTURE_5_DAILY']*50) # volumetric water into mm
plt.ylabel('Daily Precipitation (mm)')
plt.show()
```


![png](antecedent_precipitation_index_files/antecedent_precipitation_index_8_0.png)


## Trim dates with no records

From the previous plot we learned that during part of the precipitation record there are not obsevrations of volumetric water content. So, we should remove years 2008 and 2009 that do not contain any soil moisture records.

Unfortunately, in this case is hard to fill missing observations since the gap is at the beginning of the time series. There are methods for hindcasting soil moisture based on precipitation, but we will keep this analysis simple.



```python
# Trim data and reset index
df = df[730:]
df = df.reset_index() 
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
      <th>index</th>
      <th>WBANNO</th>
      <th>LST_DATE</th>
      <th>CRX_VN</th>
      <th>LONGITUDE</th>
      <th>LATITUDE</th>
      <th>T_DAILY_MAX</th>
      <th>T_DAILY_MIN</th>
      <th>T_DAILY_MEAN</th>
      <th>T_DAILY_AVG</th>
      <th>...</th>
      <th>SOIL_MOISTURE_5_DAILY</th>
      <th>SOIL_MOISTURE_10_DAILY</th>
      <th>SOIL_MOISTURE_20_DAILY</th>
      <th>SOIL_MOISTURE_50_DAILY</th>
      <th>SOIL_MOISTURE_100_DAILY</th>
      <th>SOIL_TEMP_5_DAILY</th>
      <th>SOIL_TEMP_10_DAILY</th>
      <th>SOIL_TEMP_20_DAILY</th>
      <th>SOIL_TEMP_50_DAILY</th>
      <th>SOIL_TEMP_100_DAILY</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>730</td>
      <td>63898</td>
      <td>2009-10-02</td>
      <td>2.402</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>17.9</td>
      <td>12.2</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>...</td>
      <td>0.412</td>
      <td>0.382</td>
      <td>0.381</td>
      <td>0.427</td>
      <td>0.456</td>
      <td>15.6</td>
      <td>15.7</td>
      <td>16.0</td>
      <td>17.1</td>
      <td>18.3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>731</td>
      <td>63898</td>
      <td>2009-10-03</td>
      <td>2.402</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>13.8</td>
      <td>9.7</td>
      <td>11.7</td>
      <td>12.1</td>
      <td>...</td>
      <td>0.393</td>
      <td>0.369</td>
      <td>0.378</td>
      <td>0.427</td>
      <td>0.456</td>
      <td>14.8</td>
      <td>15.2</td>
      <td>15.7</td>
      <td>16.9</td>
      <td>18.1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>732</td>
      <td>63898</td>
      <td>2009-10-04</td>
      <td>2.402</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>17.0</td>
      <td>6.0</td>
      <td>11.5</td>
      <td>11.4</td>
      <td>...</td>
      <td>0.381</td>
      <td>0.360</td>
      <td>0.373</td>
      <td>0.423</td>
      <td>0.457</td>
      <td>14.2</td>
      <td>14.6</td>
      <td>15.1</td>
      <td>16.6</td>
      <td>17.9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>733</td>
      <td>63898</td>
      <td>2009-10-05</td>
      <td>2.402</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>19.4</td>
      <td>4.5</td>
      <td>12.0</td>
      <td>11.5</td>
      <td>...</td>
      <td>0.370</td>
      <td>0.352</td>
      <td>0.368</td>
      <td>0.419</td>
      <td>0.456</td>
      <td>13.8</td>
      <td>14.2</td>
      <td>14.7</td>
      <td>16.3</td>
      <td>17.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>734</td>
      <td>63898</td>
      <td>2009-10-06</td>
      <td>2.402</td>
      <td>-86.57</td>
      <td>38.89</td>
      <td>17.3</td>
      <td>7.6</td>
      <td>12.4</td>
      <td>13.6</td>
      <td>...</td>
      <td>0.389</td>
      <td>0.361</td>
      <td>0.367</td>
      <td>0.417</td>
      <td>0.456</td>
      <td>14.0</td>
      <td>14.2</td>
      <td>14.6</td>
      <td>16.0</td>
      <td>17.4</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 29 columns</p>
</div>



## Convert volumetric water content to millimeters

We will calculate the total amount of water in the top 50 cm of the soil profile using information from the soil moisture sensors at 5, 10, 20, and 50 cm depth. The resulting soil water content will be the weighted sum of all the layers between sensors. For instance, the soil water content in the layer 0-5 cm will be assumed equal to the soil water content of the sensor at 5 cm. This is because we don't have a sensor at the surface. We know that the soil moisture near the surface may be lower or higher, but this is the best we can do at this point. Similarly the soil water content in the layer 5-10 cm is approximated as the average of the sensor at 5 and 10 cm. To work in millimeters we simply multiply the volumetric water content reading of the sensor times the depth of the soil layer also expressed in millimeters. If we then add the millimeters from all four layers we end up with an estimate of the amount of millimeters of soil water in the top 50 cm of the soil profile.


```python
# Calcualte soil moisture in the top 50 cm. 
swc_50cm_obs = df['SOIL_MOISTURE_5_DAILY'] * 50 + \
              (df['SOIL_MOISTURE_5_DAILY'] + df['SOIL_MOISTURE_10_DAILY'])/2 * 50 + \
              (df['SOIL_MOISTURE_10_DAILY'] + df['SOIL_MOISTURE_20_DAILY'])/2 * 100 + \
              (df['SOIL_MOISTURE_20_DAILY'] + df['SOIL_MOISTURE_50_DAILY'])/2 * 300

# Check for NaNs
# np.sum(np.isnan(swc_50cm_obs))

# Replace NaNs using backward fill
swc_50cm_obs = swc_50cm_obs.replace(np.nan,method='bfill')
```


```python
plt.figure(figsize=(8,6))
plt.plot(df['LST_DATE'],df['P_DAILY_CALC'])
plt.plot(df['LST_DATE'],swc_50cm_obs)
plt.ylabel('Daily precipitation or soil moisture (mm)')
plt.show()
```


![png](antecedent_precipitation_index_files/antecedent_precipitation_index_13_0.png)



```python
# Define gamma model
gamma_model = lambda doy,C,phi: C + (0.99 - C)*np.sin(2*np.pi*(doy - phi)/365 + np.pi/2)

```


```python
# Plot gamma model for days of the year 1 to 365 to ensure the model works as expected
plt.plot(np.arange(1,366),gamma_model(np.arange(1,366),0.95,15))
plt.xlabel('Day of the year')
plt.ylabel('Loss coefficient')
plt.show()

```


![png](antecedent_precipitation_index_files/antecedent_precipitation_index_15_0.png)


## Optimizing API parameters

We will pass in a dictionary called `X` the `x` variable (in this case DOY) and all the paramters that we know and we don't want to optimize (e.g. ul and ll). If we measured or know the value of some parameters it is a good practice to provide this infromation to the model. We assume that observations are more accurate than the optimized value.

Our function has two timeseries as input (`precipitation` and `day of the year`) and three scalars (`upperLimit`, `lowerLimit`, and `initialConditions`)


```python
# Create function containing both the API and the gamma model so that we can optimize the
# function parameters

def api_model(X,C,phi):

    gamma_model = lambda x,C,phi: C + (0.99 - C)*np.sin(2*np.pi*(x - phi)/365 + np.pi/2)
    gamma = gamma_model(X['doy'],C,phi)
    
    api = [X['initialConditions']]
    for i in range(1,len(X['doy'])):
        current_api = np.minimum(X['lowerLimit'] + (api[i-1] - X['lowerLimit']) * gamma[i] + X['rainfall'][i],
                                 X['upperLimit']);
        api.append(current_api)
    
    return api
    
```


```python
# Set function inputs
X = {'doy': df['LST_DATE'].dt.dayofyear, 
     'rainfall':df['P_DAILY_CALC'], 
     'upperLimit':np.max(swc_50cm_obs), 
     'lowerLimit':np.min(swc_50cm_obs),
     'initialConditions': (np.max(swc_50cm_obs) + np.min(swc_50cm_obs))/2}

```

## Guessing parameters values

This steps is not needed, but I want to show you that when parameters have clear physical meaning


```python
# Plot model with guessed values
C_guessed = 0.95
phi_guessed = 15.0
plt.plot(df['LST_DATE'],api_model(X,C_guessed, phi_guessed),label='Predicted')
plt.plot(df['LST_DATE'],swc_50cm_obs,label='Observed')
plt.ylabel('Soil moisture in top 50 cm (mm)')
plt.legend()
plt.show()

```


![png](antecedent_precipitation_index_files/antecedent_precipitation_index_20_0.png)


## Optimize parameters



```python
# Optimize parameters
par_opt, par_cov = curve_fit(api_model, X, swc_50cm_obs)
print('Annual mean gamma value is',round(par_opt[0],2))
print('Day of the year with the lowest atmospheric demand is',round(par_opt[1]))

```

    Annual mean gamma value is 0.97
    Day of the year with the lowest atmospheric demand is 11.0



```python
# Compare observed versus predicted timeries with OPTIMIZED parameters
swc_50cm_pred = api_model(X,par_opt[0],par_opt[1])
plt.plot(df['LST_DATE'], swc_50cm_pred, label='Predicted')
plt.plot(df['LST_DATE'], swc_50cm_obs, label='Observed')
plt.legend()
plt.show()

```


![png](antecedent_precipitation_index_files/antecedent_precipitation_index_23_0.png)


## Compute error metrics

For simplicity I will use the root mean squared error (RMSE) and median absolute error (MAE)


```python
# Root Mean Squared Error
RMSE = np.sqrt(np.mean((swc_50cm_obs - swc_50cm_pred)**2))
print('RMSE =', round(RMSE,2),'mm')

# Mean Absolute Error
MAE = np.mean(np.abs(swc_50cm_obs - swc_50cm_pred))
print('MAE =', round(MAE,2),'mm')
```

    RMSE = 16.62 mm
    MAE = 13.53 mm


## Comments

- THe model is not too bad considering that we are only using rainfall and few extra parameters to keep the model within realistic boundaries (i.e. between zero moisture and saturation).

- You can try to compute the error metrics for the model with guessed parameters

- You can try to compute the soil moisture for a different depth from the soil surface.

## References

Saxton, K.E. and Lenz, A.T., 1967. Antecedent retention indexes predict soil moisture. Journal of the Hydraulics Division.

Crow, W.T. and Ryu, D., 2009. A new data assimilation approach for improving runoff prediction using remotely-sensed soil moisture retrievals. Hydrology and Earth System Sciences, 13(1), pp.1-16.
