## Thermal Time

Temperature is perhaps the most important variable regulating chemical, and consequently, many biological processes. The concept of thermal time takes advantage of the temperature dependence of many biological process and allow us to use temperature information to predict the timing of particular physiological events. Thermal time is also known as growing degree-days and has units of C-d (or Cd). The formula is:


$$ TT = \Bigg[ \frac{ (T_{max} + T_{min})} {2} \Bigg] - T_{base} $$

$TT$ is the thermal time (also know as GDD, growing degree-days)

$T_{max}$ is the maximum daily air temperature

$T_{min}$ is the minimum daily air temperature

$T_{base}$ Temperature below which growth and development are assumed negligible.

In the case of agricultural crops the base temperature represents the temperature **below** which the crop has low or negligible growth (biomass accumulation) and development (meristematic differentiation) and the upper temperature represents the temperature **above** which the crop has low or negligible growth and development. Most crops have stage-specific cardinal temperatures, but in this example we will assume a single set of base and upper temperatures.

## Methods of computation

According to McMaster and Wilhelm (1997) the two most popular methods are:

>Note that `Tupper` is not always considered but it certainly makes sense to set an upper threshold for biological development.

**Method 1**

$$ if \Bigg[ \frac{ (T_{max} + T_{min})} {2} \Bigg] < T_{base}, \; then \Bigg[ \frac{ (T_{max} + T_{min})} {2} \Bigg] = T_{base} $$

$$ if \Bigg[ \frac{ (T_{max} + T_{min})} {2} \Bigg] > T_{upper}, \; then \Bigg[ \frac{ (T_{max} + T_{min})} {2} \Bigg] = T_{upper} $$

**Method 2**

$$ if \; T_{max} < T_{base}, \; then \; T_{max} = T_{base} $$

$$ if \; T_{min} < T_{base}, \; then \; T_{min} = T_{base} $$

$$ if \; T_{max} > T_{upper}, \; then \; T_{max} = T_{upper} $$

$$ if \; T_{min} > T_{upper}, \; then \; T_{min} = T_{upper} $$


```python
# Import modules
import numpy as np

```


```python
def thermaltime(TMAX,TMIN,Tbase,Tupper,method):
    """Calculates cumulative growing degree days.

    Keyword arguments:
    TMAX -- vector of air or soil maximum temperature in degrees Celsius.
    TMIN -- vector of air or soil minimum temperature in degrees Celsius.
    Tbase = Scalar in degrees Celsius. It represents the base temperature 
            below which a given crop has low or negligible growth and development.
    Tupper = Scalar in degrees Celsius. It represents the upper temperature 
             above which a given crop has low or negligible growth and development.
    method = There two methods according to McMaster and Wilhelm, 1997.
           1. The comparison to Tbase and Tupper is after computing 
              average temperature.
           2. The comparison to Tbase and Tupper is prior computing 
               average temperature.

    NOTE: The function accepts NaNs and no substitutions or estimations
          are carried out for those days.

    Outputs:
           TT = daily thermal time. Units are C-d (or Cd).
           TTcum = cumulative thermal time.

    Andres Patrignani - 19 Mar 2017.

    Code based on manuscript by McMaster, G.S. and W.W. Wilhelm. 1997. Agric.
    and Forest meteorology. 87:291-300.
    """
    
    # Compare to Tbase after computing TAVG.
    if method == 1:
        TAVG = (TMAX + TMIN)/2 # Computation of average before limits
        TAVG = np.maximum(TAVG,Tbase)
        TAVG = np.minimum(TAVG,Tupper)

    # Compare to Tbase before computing TAVG.    
    elif method == 2: 
        TMAX = np.maximum(TMAX,Tbase)
        TMIN = np.maximum(TMIN,Tbase)
        TMAX = np.minimum(TMAX,Tupper)
        TMIN = np.minimum(TMIN,Tupper)
        TAVG = (TMAX + TMIN)/2 # Computation of average after limits


    TT = TAVG - Tbase # General thermal time equation.
    TT = np.nan_to_num(TT)
    TTcum = np.cumsum(TT) # Cumulative thermal time.

    return TT,TTcum
```

## Example


```python
# Import additional modules
import pandas as pd
import matplotlib.pyplot as plt

```


```python
# Load some weather data
df = pd.read_csv("../datasets/KS_Manhattan_6_SSW.csv")
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
      <td>20031001</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>...</td>
      <td>-99.000</td>
      <td>-99.000</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>53974</td>
      <td>20031002</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>18.9</td>
      <td>2.5</td>
      <td>10.7</td>
      <td>11.7</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.000</td>
      <td>-99.000</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>53974</td>
      <td>20031003</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>22.6</td>
      <td>8.1</td>
      <td>15.4</td>
      <td>14.8</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.000</td>
      <td>-99.000</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>53974</td>
      <td>20031004</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>22.6</td>
      <td>3.8</td>
      <td>13.2</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.000</td>
      <td>-99.000</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>53974</td>
      <td>20031005</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>25.0</td>
      <td>10.6</td>
      <td>17.8</td>
      <td>17.3</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.000</td>
      <td>-99.000</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5113</th>
      <td>53974</td>
      <td>20170930</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>28.3</td>
      <td>14.6</td>
      <td>21.4</td>
      <td>21.1</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.141</td>
      <td>0.153</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>19.6</td>
      <td>20.0</td>
      <td>20.2</td>
      <td>21.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>5114</th>
      <td>53974</td>
      <td>20171001</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>27.9</td>
      <td>17.7</td>
      <td>22.8</td>
      <td>21.9</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.138</td>
      <td>0.152</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>19.7</td>
      <td>20.0</td>
      <td>20.3</td>
      <td>20.9</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>5115</th>
      <td>53974</td>
      <td>20171002</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>31.6</td>
      <td>18.5</td>
      <td>25.1</td>
      <td>24.8</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.137</td>
      <td>0.151</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>20.6</td>
      <td>20.7</td>
      <td>20.5</td>
      <td>20.8</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>5116</th>
      <td>53974</td>
      <td>20171003</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>27.6</td>
      <td>15.5</td>
      <td>21.5</td>
      <td>21.1</td>
      <td>2.8</td>
      <td>...</td>
      <td>0.134</td>
      <td>0.155</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>21.0</td>
      <td>21.3</td>
      <td>21.1</td>
      <td>20.9</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>5117</th>
      <td>53974</td>
      <td>20171004</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>16.6</td>
      <td>14.0</td>
      <td>15.3</td>
      <td>15.2</td>
      <td>10.2</td>
      <td>...</td>
      <td>0.151</td>
      <td>0.183</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>18.7</td>
      <td>19.5</td>
      <td>20.4</td>
      <td>21.0</td>
      <td>-9999.0</td>
    </tr>
  </tbody>
</table>
<p>5118 rows × 28 columns</p>
</div>




```python
# Convert dates to datetime format
df["LST_DATE"] = pd.to_datetime(df["LST_DATE"], format="%Y%m%d")
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
      <td>2003-10-01</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>53974</td>
      <td>2003-10-02</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>18.9</td>
      <td>2.5</td>
      <td>10.7</td>
      <td>11.7</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>53974</td>
      <td>2003-10-03</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>22.6</td>
      <td>8.1</td>
      <td>15.4</td>
      <td>14.8</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>53974</td>
      <td>2003-10-04</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>22.6</td>
      <td>3.8</td>
      <td>13.2</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>53974</td>
      <td>2003-10-05</td>
      <td>1.201</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>25.0</td>
      <td>10.6</td>
      <td>17.8</td>
      <td>17.3</td>
      <td>0.0</td>
      <td>...</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
      <td>-9999.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
# Set some crop parameters

# Aproximate for winter wheat
Tbase = 5
Tupper = 35
planting_date = pd.to_datetime("1-oct-2016")
harvest_date = pd.to_datetime("1-jun-2017")

```


```python
# Select growing season weather
idx_season = (df["LST_DATE"] >= planting_date) & (df["LST_DATE"] <= harvest_date)
df_season = df[idx_season]
print('Growing season length:', df_season.shape[0], 'days')
df_season.head()

```

    Growing season length: 244 days





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
      <th>4749</th>
      <td>53974</td>
      <td>2016-10-01</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>23.9</td>
      <td>7.9</td>
      <td>15.9</td>
      <td>15.2</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.426</td>
      <td>0.391</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>17.4</td>
      <td>18.1</td>
      <td>18.7</td>
      <td>20.2</td>
      <td>21.3</td>
    </tr>
    <tr>
      <th>4750</th>
      <td>53974</td>
      <td>2016-10-02</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>26.4</td>
      <td>8.7</td>
      <td>17.6</td>
      <td>17.8</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.422</td>
      <td>0.386</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>17.9</td>
      <td>18.3</td>
      <td>18.7</td>
      <td>20.0</td>
      <td>21.0</td>
    </tr>
    <tr>
      <th>4751</th>
      <td>53974</td>
      <td>2016-10-03</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>27.6</td>
      <td>12.5</td>
      <td>20.0</td>
      <td>20.4</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.413</td>
      <td>0.381</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>18.7</td>
      <td>19.0</td>
      <td>19.1</td>
      <td>19.9</td>
      <td>20.9</td>
    </tr>
    <tr>
      <th>4752</th>
      <td>53974</td>
      <td>2016-10-04</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>24.2</td>
      <td>12.8</td>
      <td>18.5</td>
      <td>19.8</td>
      <td>23.2</td>
      <td>...</td>
      <td>0.430</td>
      <td>0.399</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>18.8</td>
      <td>19.1</td>
      <td>19.4</td>
      <td>19.9</td>
      <td>20.8</td>
    </tr>
    <tr>
      <th>4753</th>
      <td>53974</td>
      <td>2016-10-05</td>
      <td>2.422</td>
      <td>-96.61</td>
      <td>39.1</td>
      <td>25.5</td>
      <td>7.7</td>
      <td>16.6</td>
      <td>16.7</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.534</td>
      <td>0.484</td>
      <td>-99.0</td>
      <td>-99</td>
      <td>-99</td>
      <td>17.7</td>
      <td>18.2</td>
      <td>18.7</td>
      <td>19.8</td>
      <td>20.6</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
DAP = np.arange(df_season.shape[0])
TMAX = df_season["T_DAILY_MAX"]
TMIN = df_season["T_DAILY_MIN"]

```


```python
# Compute thermal time using method 1
TT1, TTcum1 = thermaltime(TMAX,TMIN,Tbase,Tupper,method=1)

```


```python
# Compute thermal time using method 2
TT2, TTcum2 = thermaltime(TMAX,TMIN,Tbase,Tupper,method=2)

```


```python
diff_methods = round(np.abs(TTcum1[-1] - TTcum2[-1]))
print('The difference between methods is',diff_methods, 'degree-days')

```

    The difference between methods is 205.0 degree-days


## Compare cumulative thermal between methods


```python
# Compare cumulative thermal time for both methods
plt.figure(figsize=(10,8))
plt.plot(DAP,TTcum1, '-k', label="Method 1")
plt.plot(DAP,TTcum2, '--k', label="Method 2")
plt.xlabel("Days after planting", size=16)
plt.ylabel("Cumulative thermal time (Cd)", size=16)
plt.legend()
plt.show()

```


![png](thermal_time_files/thermal_time_14_0.png)


## Compare daily thermal between methods


```python
plt.figure(figsize=(10,8))
plt.plot(DAP,TT1, '-k', label="Method 1", alpha=0.2)
plt.plot(DAP,TT2, '--k', label="Method 2", alpha=0.2)
plt.fill_between(DAP,TT1,TT2, facecolor='g', alpha=0.5)
plt.xlabel("Days after planting", size=16)
plt.ylabel("Daily thermal time (Cd)", size=16)
plt.legend()
plt.show()

```


![png](thermal_time_files/thermal_time_16_0.png)


## References

McMaster, G.S. and Wilhelm, W., 1997. Growing degree-days: one equation, two interpretations. Agricultural and Forest Meteorology 87 (1997) 291-300
