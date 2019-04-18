
# **Request data from URLs**

Example of retrieving data from the US Climate reference Network


## Documentation

Daily data: https://www1.ncdc.noaa.gov/pub/data/uscrn/products/daily01/

Daily data documentation: https://www1.ncdc.noaa.gov/pub/data/uscrn/products/daily01/README.txt

## Reference

- Diamond, H. J., T. R. Karl, M. A. Palecki, C. B. Baker, J. E. Bell, R. D. Leeper, D. R. Easterling, J. H. Lawrimore, T. P. Meyers, M. R. Helfert, G. Goodge, and P. W. Thorne, 2013: U.S. Climate Reference Network after one decade of operations: status and assessment. Bull. Amer. Meteor. Soc., 94, 489-498. doi: 10.1175/BAMS-D-12-00170.1


```python
import pandas as pd
import numpy as np

%matplotlib inline
import matplotlib.pyplot as plt
```


```python
# URL link and header variables
url = 'https://www1.ncdc.noaa.gov/pub/data/uscrn/products/daily01/2018/CRND0103-2018-KS_Manhattan_6_SSW.txt'
header_names = ['WBANNO','LST_DATE','CRX_VN','LONGITUDE','LATITUDE','T_DAILY_MAX','T_DAILY_MIN','T_DAILY_MEAN','T_DAILY_AVG','P_DAILY_CALC','SOLARAD_DAILY','SUR_TEMP_DAILY_TYPE','SUR_TEMP_DAILY_MAX','SUR_TEMP_DAILY_MIN','SUR_TEMP_DAILY_AVG','RH_DAILY_MAX','RH_DAILY_MIN','RH_DAILY_AVG','SOIL_MOISTURE_5_DAILY','SOIL_MOISTURE_10_DAILY','SOIL_MOISTURE_20_DAILY','SOIL_MOISTURE_50_DAILY','SOIL_MOISTURE_100_DAILY','SOIL_TEMP_5_DAILY','SOIL_TEMP_10_DAILY','SOIL_TEMP_20_DAILY','SOIL_TEMP_50_DAILY','SOIL_TEMP_100_DAILY'];        

```


```python
# Read fixed width data
df = pd.read_fwf(url, names=header_names)
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
      <td>53974</td>
      <td>20180101</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-11.0</td>
      <td>-23.4</td>
      <td>-17.2</td>
      <td>-17.1</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-2.7</td>
      <td>-0.8</td>
      <td>0.8</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>53974</td>
      <td>20180102</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-4.4</td>
      <td>-20.8</td>
      <td>-12.6</td>
      <td>-11.6</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-2.5</td>
      <td>-1.0</td>
      <td>0.1</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>53974</td>
      <td>20180103</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-1.5</td>
      <td>-13.3</td>
      <td>-7.4</td>
      <td>-6.1</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-1.8</td>
      <td>-0.7</td>
      <td>-0.1</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>53974</td>
      <td>20180104</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>3.2</td>
      <td>-16.3</td>
      <td>-6.5</td>
      <td>-6.5</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-1.9</td>
      <td>-0.8</td>
      <td>-0.2</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>53974</td>
      <td>20180105</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-0.6</td>
      <td>-11.9</td>
      <td>-6.2</td>
      <td>-6.7</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-1.6</td>
      <td>-0.6</td>
      <td>-0.1</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
# Convert date from string to datetime format
df['LST_DATE'] = pd.to_datetime(df['LST_DATE'],format='%Y%m%d')
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
      <td>53974</td>
      <td>2018-01-01</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-11.0</td>
      <td>-23.4</td>
      <td>-17.2</td>
      <td>-17.1</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-2.7</td>
      <td>-0.8</td>
      <td>0.8</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>53974</td>
      <td>2018-01-02</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-4.4</td>
      <td>-20.8</td>
      <td>-12.6</td>
      <td>-11.6</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-2.5</td>
      <td>-1.0</td>
      <td>0.1</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>53974</td>
      <td>2018-01-03</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-1.5</td>
      <td>-13.3</td>
      <td>-7.4</td>
      <td>-6.1</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-1.8</td>
      <td>-0.7</td>
      <td>-0.1</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>53974</td>
      <td>2018-01-04</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>3.2</td>
      <td>-16.3</td>
      <td>-6.5</td>
      <td>-6.5</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-1.9</td>
      <td>-0.8</td>
      <td>-0.2</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>53974</td>
      <td>2018-01-05</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-0.6</td>
      <td>-11.9</td>
      <td>-6.2</td>
      <td>-6.7</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-1.6</td>
      <td>-0.6</td>
      <td>-0.1</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
# Replace missing values (-99 and -9999)
df = df.replace(-9999, np.nan)
df = df.replace(-99, np.nan)
df = df.replace(999, np.nan)
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
      <td>53974</td>
      <td>2018-01-01</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-11.0</td>
      <td>-23.4</td>
      <td>-17.2</td>
      <td>-17.1</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-2.7</td>
      <td>-0.8</td>
      <td>0.8</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>53974</td>
      <td>2018-01-02</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-4.4</td>
      <td>-20.8</td>
      <td>-12.6</td>
      <td>-11.6</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-2.5</td>
      <td>-1.0</td>
      <td>0.1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>53974</td>
      <td>2018-01-03</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-1.5</td>
      <td>-13.3</td>
      <td>-7.4</td>
      <td>-6.1</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-1.8</td>
      <td>-0.7</td>
      <td>-0.1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>53974</td>
      <td>2018-01-04</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>3.2</td>
      <td>-16.3</td>
      <td>-6.5</td>
      <td>-6.5</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-1.9</td>
      <td>-0.8</td>
      <td>-0.2</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>53974</td>
      <td>2018-01-05</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-0.6</td>
      <td>-11.9</td>
      <td>-6.2</td>
      <td>-6.7</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-1.6</td>
      <td>-0.6</td>
      <td>-0.1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
# Plot figure
plt.figure(figsize=(12,6))
plt.plot(df.LST_DATE,df.T_DAILY_AVG,'-k')
plt.plot(df.LST_DATE,df.T_DAILY_MAX, '--r')
plt.plot(df.LST_DATE,df.T_DAILY_MIN, '--b')
plt.axhline(df.T_DAILY_AVG.mean())
plt.ylabel('Air temperature (degrees Celsius)')
plt.legend(['Tavg','Tmax','Tmin'])
plt.show()
```


![png](output_6_0.png)

