# Soil Evaporation

Soil evaporation is the process by which water leaves the pore spaces in the soil to become part of the surrounding atmopshere. The process of evaporating soil moisture requires energy, either directly supplied by the sun or from the internal energy of the soil.

Assuming that we start from a fairly moist soil, the evaporative process undergoes two distinct stages. The first stage is characterized by a constant evaporation rate that is only limited by the energy available for evaporation (energy-limited). During this stage water in the pore spaces right beneath the surface can still reach the surface by means of capillarity. A second stage characterized by a decreasing evaporative rate is evident when the soil cannot keep up with the evaporative rate of the first stage. The second stage is limited by the soil's ability to conduct water to the evaporative surface (supply-limited). During long evaporative processes, a third stage of almost no evaporation rate (evaporation asymptotically approaches zero) can be identified.


```python
# Import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

```


```python
# Load dataset
df = pd.read_csv('../datasets/evaporation.csv')
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
      <th>sample_mass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10/18/17 17:21</td>
      <td>297.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10/18/17 17:26</td>
      <td>297.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10/18/17 17:31</td>
      <td>297.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10/18/17 17:36</td>
      <td>297.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10/18/17 17:41</td>
      <td>296.9</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Convert dates to Pandas datetime format
df["timestamp"] = pd.to_datetime(df["timestamp"], format="%m/%d/%y %H:%M")

```


```python
# Compute elapsed time since beginning of the experiment in days
df["elapsed_time"] = (df["timestamp"] - df["timestamp"][0]).dt.total_seconds()/86400
df["day"] = df["timestamp"].dt.day
df["hours"] = df["timestamp"].dt.hour

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
      <th>sample_mass</th>
      <th>elapsed_time</th>
      <th>day</th>
      <th>hours</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2017-10-18 17:21:00</td>
      <td>297.1</td>
      <td>0.000000</td>
      <td>18</td>
      <td>17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017-10-18 17:26:00</td>
      <td>297.0</td>
      <td>0.003472</td>
      <td>18</td>
      <td>17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2017-10-18 17:31:00</td>
      <td>297.0</td>
      <td>0.006944</td>
      <td>18</td>
      <td>17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2017-10-18 17:36:00</td>
      <td>297.0</td>
      <td>0.010417</td>
      <td>18</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2017-10-18 17:41:00</td>
      <td>296.9</td>
      <td>0.013889</td>
      <td>18</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Inspect data
plt.figure()
plt.plot(df["elapsed_time"], df["sample_mass"])
plt.xlabel("Days since start of the experiment")
plt.ylabel("Samples mass (g)")
plt.show()

```


![png](evaporation_files/evaporation_5_0.png)



```python
# Smooth samples mass data
df["smoothed_sample_mass"] = df["sample_mass"].rolling(window=15).mean()

```


```python
# Compute evaporation rate
E_rate = np.diff(df["smoothed_sample_mass"].iloc[0::20]) / np.diff(df["elapsed_time"].iloc[0::20])
E_rate = E_rate * -1

```


```python
plt.figure()
plt.scatter(df["elapsed_time"].iloc[20::20], E_rate)
plt.axvline(1.25, linestyle='--', color='k')
plt.axvline(4,linestyle='--', color='k')
plt.show()
```


![png](evaporation_files/evaporation_8_0.png)

