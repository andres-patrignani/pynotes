# Rainfall Generator

Weather data is ubiquitous and important for most daily activities. In agriculture, weather plays a preponderant role since it dictates the growth and eventual crop yield (fiber, biomass, or grain).

Air temperature and precipitation are by far the most commonly monitored variables, followed by relative humidity, wind speed and direccion, solar radiation, soil temperature, and soil moisture.

We will create a weather generator to generate daily minimum and maximum air temperature and precipitation scenarios. To understand the process we will examine long-term data for a specific location. Based on the trends we will build the weather generator.

> This weather generator is for teaching purposes only and lacks the statistical rigor of research tools. The exercise is only intended as a primer for students learning how to creating simple scripts using random modules. For more advanced tools, please visit the references section.



```python
# Import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from scipy.stats import gamma
from scipy.optimize import curve_fit

```


```python
# Load sample data
df = pd.read_csv("../datasets/KS_Manhattan_6_SSW.csv")
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
# Convert date string to pandas datetime format
df["LST_DATE"] = pd.to_datetime(df["LST_DATE"], format="%Y%m%d")
df["LST_DATE"].head() # CHeck our conversion.

```




    0   2003-10-01
    1   2003-10-02
    2   2003-10-03
    3   2003-10-04
    4   2003-10-05
    Name: LST_DATE, dtype: datetime64[ns]




```python
# Convert missing values represented as -9999 to NaN
df[df == -9999] = np.nan

```


```python
# Add year, month, and day of the year to summarize data in future steps.
df["YEAR"] = df["LST_DATE"].dt.year
df["MONTH"] = df["LST_DATE"].dt.month
df["DOY"] = df["LST_DATE"].dt.dayofyear

```


```python
# Observe trends in precipitation data (2004 to 2016)
plt.figure(figsize=(8,6))
plt.xlim(0,400)
for year in range(2004,2017):
    idx_year = df["YEAR"] == year
    cdf = df["P_DAILY_CALC"][idx_year].cumsum()
    plt.plot(range(1,len(cdf)+1),cdf)
    #plt.annotate(str(year), xy=(370,cdf.values[-1]))
    
```


![png](rainfall_generator_files/rainfall_generator_6_0.png)



```python
# Observe histograms of precipitation

plt.figure(figsize=(8,6))
for year in range(2004,2017):
    idx_year = df["YEAR"] == year
    rainfall_year = df["P_DAILY_CALC"][idx_year]
    rainfall_year = rainfall_year[rainfall_year > 1]
    plt.hist(rainfall_year, bins=25)

```


![png](rainfall_generator_files/rainfall_generator_7_0.png)


## Modeling rainfall as a Markov chain. 

This type of rainfall generators is kwnon as the Richardson-type (Richardson and Wright, 1984). The model computes the rainfall at time *t* as a function of time *t-1* in three steps. 

To be consistent with the original manuscript by Richardson and Wright, 1984 we will use the term "wet" for a day with measurable rainfall and "dry" for a day without measurable rainfall.

**Step 1**: Find if the previous (i.e. *t-1*) day was dry (no measurable rainfall) or wet (some measurable rainfall)

**Step 2**: Then compute the probability of having a rainfall event on day *t* given the condition on the previous day. So, here we need to have two different probability values, one that we will use if yesterday was dry, and another if yesterday was wet. 

- What is the probability of having a wet day if yesterday was dry? (i.e. P(W/D) )

- What is the probability of having a wet day if yesterday was wet? (i.e. P(W/W) )

As you can see if we only use two values and we are generating rainfall for a long period, then may not accurately simulate seasonal rainfall trends. TO solve this some models use Wet-Dry and Wet-Wet on a monthly basis. In this step we generate a yes/no answer. We will deal with the amount in the next step. 

**Step 3**: If as a consequence of the random process we obtain that on day *t* there is a rainfall event, then we have to compute the amount. To do this we will use a gamma distribution, which is suited to heavily skewed distributions, such as those resulting from histograms of daily rainfall. Again, here we probably want to have some parameters that describe this distribution at least on a monthly basis to capture the seasonal effects on rainfall amounts.


We will first run an example using the loaded dataset to learn how to compute the Wet-Dry and Wet-Wet probabilities for the entire year. It's important that we first learn how to compute the simplest step before moving into a more detailed characterization of the rainfall process on a monthly basis.



```python
# Rainfall probability Wet-Dry and Wet-Wet
WW = []
WD = []
month_WW = []
month_WD = []

# Extract variables from DataFrame to sherten variable names
months = df["MONTH"]
rainfall = df["P_DAILY_CALC"]

for i in range(1,df.shape[0]):
    if rainfall[i-1] > 0:
        
        # Get current month
        month_WW.append(months[i])
        
        if rainfall[i] > 0:
            WW.append(1)
        else:
            WW.append(0)
    
    elif rainfall[i-1] == 0:
        
        month_WD.append(months[i])
        if rainfall[i] > 0:
            WD.append(1)
        else:
            WD.append(0)

print(sum(WW)/len(WW))
print(sum(WD)/len(WD))

```

    0.4166666666666667
    0.20590604026845638



```python
# Wet/Wet
df_WW = pd.DataFrame([month_WW,WW]).T
df_WW.columns = ["month","WW"]
df_WW.tail(5)

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
      <th>WW</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1315</th>
      <td>9</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1316</th>
      <td>9</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1317</th>
      <td>9</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1318</th>
      <td>9</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1319</th>
      <td>10</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Wet/Dry
df_WD = pd.DataFrame([month_WD, WD]).T
df_WD.columns = ["month","WD"]
df_WD.tail(5)

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
      <th>WD</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3720</th>
      <td>9</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3721</th>
      <td>9</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3722</th>
      <td>10</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3723</th>
      <td>10</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3724</th>
      <td>10</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



## Daily rainfall probability per month


```python
# Compute monthly probabilities per month
monthly_WW = df_WW.groupby("month").sum()/df_WW.groupby("month").count()
monthly_WD = df_WD.groupby("month").sum()/df_WD.groupby("month").count()

```


```python
# Create table of parameters
coeff = pd.concat([monthly_WW, monthly_WD], axis=1)
coeff

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
      <th>WW</th>
      <th>WD</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.338028</td>
      <td>0.141176</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.320513</td>
      <td>0.161184</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.401709</td>
      <td>0.219048</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.503448</td>
      <td>0.273063</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.480769</td>
      <td>0.290909</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.442953</td>
      <td>0.293680</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.415254</td>
      <td>0.228571</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.412214</td>
      <td>0.245847</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.362745</td>
      <td>0.201258</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.409524</td>
      <td>0.193353</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.388889</td>
      <td>0.129794</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.381579</td>
      <td>0.144092</td>
    </tr>
  </tbody>
</table>
</div>



## Daily rainfall amount probability


```python
# Fit gamma distribution to each month
monthly_shape = []
monthly_scale = []
unique_months = list(range(1,13))
for m in unique_months:
    idx_month = df["MONTH"] == m
    x = df["P_DAILY_CALC"].loc[idx_month]
    x = x[~np.isnan(x)]
    x = x[x>0]
    shape, loc, scale = gamma.fit(x, floc=0)
    monthly_shape.append(shape)
    monthly_scale.append(scale)

```


```python
# Append amount parameters to monthly lookup table
coeff["shape"] = monthly_shape
coeff["scale"] = monthly_scale
coeff.head(12)

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
      <th>WW</th>
      <th>WD</th>
      <th>shape</th>
      <th>scale</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.338028</td>
      <td>0.141176</td>
      <td>0.748257</td>
      <td>4.166760</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.320513</td>
      <td>0.161184</td>
      <td>0.678656</td>
      <td>8.142081</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.401709</td>
      <td>0.219048</td>
      <td>0.632519</td>
      <td>11.092325</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.503448</td>
      <td>0.273063</td>
      <td>0.589697</td>
      <td>15.483557</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.480769</td>
      <td>0.290909</td>
      <td>0.630033</td>
      <td>13.661336</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.442953</td>
      <td>0.293680</td>
      <td>0.599585</td>
      <td>22.291664</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.415254</td>
      <td>0.228571</td>
      <td>0.677182</td>
      <td>16.648971</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.412214</td>
      <td>0.245847</td>
      <td>0.625811</td>
      <td>21.885339</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.362745</td>
      <td>0.201258</td>
      <td>0.585308</td>
      <td>15.593047</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.409524</td>
      <td>0.193353</td>
      <td>0.659315</td>
      <td>11.230858</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.388889</td>
      <td>0.129794</td>
      <td>0.628394</td>
      <td>10.430029</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.381579</td>
      <td>0.144092</td>
      <td>0.668791</td>
      <td>9.900732</td>
    </tr>
  </tbody>
</table>
</div>



Check that our gamma function can generate sound rainfall distributions. Note that the figure below is the pdf function, not the actual rainfall. The y-axis contains density information, while the x-axis contains rainfall data.


```python
# Demonstrate distribution of rainfall amount distribution for last month of fitting
a,b,c = gamma.fit(x, floc=0)
plt.hist(x, density=True)
plt.plot(np.linspace(0.1,50,100), gamma.pdf(np.linspace(0.1,50,100),a,b,c))
plt.show()

```


![png](rainfall_generator_files/rainfall_generator_19_0.png)


## Build simulator


```python
# Define rainfall simulator
def rainfall_gen(dates,coeff):
    
    P = np.ones(dates.shape[0])*np.nan
    P[0] = 0

    for t in range(1,dates.shape[0]):
        month = dates.month[t]
        if P[t-1] == 0:
            if np.random.rand() > coeff["WD"][month]:
                P[t] = 0
            else:
                P[t] = gamma.rvs(coeff["shape"][month],0,coeff["scale"][month])
                P[t] = np.round(P[t]*10)/10

        elif P[t-1] > 0:
            if np.random.rand() > coeff["WW"][month]:
                P[t] = 0
            else:
                P[t] = gamma.rvs(coeff["shape"][month],0,coeff["scale"][month])
                P[t] = np.round(P[t]*10)/10

    P_total = P.sum()
    return P

```

## Create rainfall scenarios


```python
# Create an example set of dates
dates = pd.date_range("1-jan-2018","31-dec-2018",freq="D")

# Call rainfall generator multiple times and plot cumulative rainfall
for i in range(20):
    P = rainfall_gen(dates,coeff)
    #print("Total rainfall",round(P.sum()))
    plt.plot(P.cumsum())
    
plt.title("Cumulative rainfall per year")
plt.xlabel("DOY")
plt.ylabel("Rainfall amount (mm)")
plt.show()

```


![png](rainfall_generator_files/rainfall_generator_23_0.png)



```python
# Plot the last iteration
plt.figure(figsize=(12,4))
plt.bar(dates,P)
plt.title("Daily rainfall") # for last year of the previous simulation
plt.ylabel("Rainfall amount (mm)")
plt.show()

```


![png](rainfall_generator_files/rainfall_generator_24_0.png)


## References

Intergernmental Panel on Climate Change Data Distribution Centre: https://www.ipcc-data.org/guidelines/pages/weather_generators.html

Jones, P., Harpham, C., Kilsby, C., Glenis, V. and Burton, A., 2010. UK Climate Projections science report: Projections of future daily climate for the UK from the Weather Generator.

Kilsby, C.G., Jones, P.D., Burton, A., Ford, A.C., Fowler, H.J., Harpham, C., James, P., Smith, A. and Wilby, R.L., 2007. A daily weather generator for use in climate change studies. Environmental Modelling & Software, 22(12), pp.1705-1719.

Richardson, C.W., 1982. Dependence structure of daily temperature and solar radiation. Transactions of the ASAE, 25(3), pp.735-0739.

Richardson, C.W. and Wright, D.A., 1984. WGEN: A model for generating daily weather variables.
