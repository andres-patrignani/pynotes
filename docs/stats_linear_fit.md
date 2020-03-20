# Linear fit

Perhaps there is no other task more common in curve fitting than optimizing the parameters of a linear model using least squares. The linear model is one simplest (of course after the constant model) models to fit our data and usually represents our first choice when we don't know any better.

## Linear model

The linear model is described by:

$$ y = a + bx $$

where:

$y$ is the dependent variable. The output of our model.

$a$ is the intercept at the ordinate. Position on the x-axis that our model intersects the y-axis. This value can be positive or negative.

$b$ is the slope of our model. It represents the rate of change of the curve. This value can be positive or negative.

$x$ is the independent variable. The input of our model.


In this exercise we will look at simple examples of linear dependency for some weather variables. The weather variables are just an excuse to use real observations and avoid relying on synthetic data. The dataset was obtained from the [Kansas Meosnet station near Gypsum, KS](https://www.google.com/maps/@38.725139,-97.4441767,42m/data=!3m1!1e3). It's important to highlight that the dataset contains atmopsheric and soil variables (soil temperature and soil moisture at multiple depths).

Soil temperature is recorded by a custom made soil temperature sensor and by the Campbel Scientific 655 (CS655) soil water reflectometer sensor, which records both soil water and soil temperature.



```python
# Import modules
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from scipy import stats

```


```python
# Load some weather data
df = pd.read_csv('../datasets/gypsum_ks_daily_2018.csv')

```


```python
# Explore headers
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
    <tr>
      <th>3</th>
      <td>1/4/18 0:00</td>
      <td>Gypsum</td>
      <td>98.22</td>
      <td>98.54</td>
      <td>97.90</td>
      <td>102.99</td>
      <td>-5.83</td>
      <td>-11.79</td>
      <td>0.24</td>
      <td>-5.01</td>
      <td>...</td>
      <td>-0.98</td>
      <td>-2.67</td>
      <td>-1.60</td>
      <td>-1.46</td>
      <td>-0.21</td>
      <td>2.45</td>
      <td>0.1235</td>
      <td>0.0973</td>
      <td>0.2094</td>
      <td>0.2182</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1/5/18 0:00</td>
      <td>Gypsum</td>
      <td>98.10</td>
      <td>98.42</td>
      <td>97.75</td>
      <td>102.88</td>
      <td>-4.73</td>
      <td>-14.22</td>
      <td>5.36</td>
      <td>-4.23</td>
      <td>...</td>
      <td>-0.72</td>
      <td>-2.81</td>
      <td>-1.54</td>
      <td>-1.38</td>
      <td>-0.25</td>
      <td>2.25</td>
      <td>0.1249</td>
      <td>0.0976</td>
      <td>0.2047</td>
      <td>0.2180</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 44 columns</p>
</div>




```python
# Explore last few rows
df.tail(5)

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
      <th>360</th>
      <td>12/27/18 0:00</td>
      <td>Gypsum</td>
      <td>96.71</td>
      <td>97.59</td>
      <td>95.54</td>
      <td>101.17</td>
      <td>9.22</td>
      <td>5.78</td>
      <td>12.36</td>
      <td>9.18</td>
      <td>...</td>
      <td>7.24</td>
      <td>3.58</td>
      <td>5.15</td>
      <td>4.97</td>
      <td>4.60</td>
      <td>5.18</td>
      <td>0.3561</td>
      <td>0.3666</td>
      <td>0.3534</td>
      <td>0.2972</td>
    </tr>
    <tr>
      <th>361</th>
      <td>12/28/18 0:00</td>
      <td>Gypsum</td>
      <td>95.77</td>
      <td>96.97</td>
      <td>95.02</td>
      <td>100.21</td>
      <td>2.60</td>
      <td>-4.05</td>
      <td>12.36</td>
      <td>2.44</td>
      <td>...</td>
      <td>7.45</td>
      <td>3.46</td>
      <td>6.26</td>
      <td>6.26</td>
      <td>6.24</td>
      <td>5.77</td>
      <td>0.3786</td>
      <td>0.3810</td>
      <td>0.3856</td>
      <td>0.3445</td>
    </tr>
    <tr>
      <th>362</th>
      <td>12/29/18 0:00</td>
      <td>Gypsum</td>
      <td>97.72</td>
      <td>98.44</td>
      <td>96.89</td>
      <td>102.44</td>
      <td>-4.92</td>
      <td>-8.76</td>
      <td>-2.09</td>
      <td>-5.07</td>
      <td>...</td>
      <td>3.46</td>
      <td>1.63</td>
      <td>2.35</td>
      <td>2.72</td>
      <td>4.30</td>
      <td>5.93</td>
      <td>0.3588</td>
      <td>0.3645</td>
      <td>0.3699</td>
      <td>0.3204</td>
    </tr>
    <tr>
      <th>363</th>
      <td>12/30/18 0:00</td>
      <td>Gypsum</td>
      <td>98.16</td>
      <td>98.55</td>
      <td>97.54</td>
      <td>102.95</td>
      <td>-6.38</td>
      <td>-9.77</td>
      <td>-0.55</td>
      <td>-6.49</td>
      <td>...</td>
      <td>1.63</td>
      <td>0.97</td>
      <td>1.22</td>
      <td>1.58</td>
      <td>2.96</td>
      <td>5.26</td>
      <td>0.3477</td>
      <td>0.3539</td>
      <td>0.3612</td>
      <td>0.3131</td>
    </tr>
    <tr>
      <th>364</th>
      <td>12/31/18 0:00</td>
      <td>Gypsum</td>
      <td>96.75</td>
      <td>97.64</td>
      <td>96.08</td>
      <td>101.38</td>
      <td>0.28</td>
      <td>-5.52</td>
      <td>8.13</td>
      <td>0.21</td>
      <td>...</td>
      <td>0.98</td>
      <td>0.71</td>
      <td>0.82</td>
      <td>1.15</td>
      <td>2.32</td>
      <td>4.67</td>
      <td>0.3373</td>
      <td>0.3455</td>
      <td>0.3557</td>
      <td>0.3089</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 44 columns</p>
</div>




```python
# Examine data to see whether we have NaN values or not
df.isna().sum()

```




    TIMESTAMP          0
    STATION            0
    PRESSUREAVG        0
    PRESSUREMAX        0
    PRESSUREMIN        0
    SLPAVG             0
    TEMP2MAVG          1
    TEMP2MMIN          1
    TEMP2MMAX          2
    TEMP10MAVG         0
    TEMP10MMIN         1
    TEMP10MMAX         2
    RELHUM2MAVG        0
    RELHUM2MMAX        0
    RELHUM2MMIN        1
    RELHUM10MAVG       0
    RELHUM10MMAX       0
    RELHUM10MMIN       1
    VPDEFAVG           1
    PRECIP             0
    SRAVG              0
    SR                 0
    WSPD2MAVG          0
    WSPD2MMAX          0
    WSPD10MAVG         0
    WSPD10MMAX         0
    WDIR2M             0
    WDIR2MSTD          0
    WDIR10M            0
    WDIR10MSTD         0
    SOILTMP5AVG        0
    SOILTMP5MAX        0
    SOILTMP5MIN        0
    SOILTMP10AVG       0
    SOILTMP10MAX       0
    SOILTMP10MIN       0
    SOILTMP5AVG655     0
    SOILTMP10AVG655    0
    SOILTMP20AVG655    0
    SOILTMP50AVG655    0
    VWC5CM             0
    VWC10CM            0
    VWC20CM            0
    VWC50CM            0
    dtype: int64



When dealing with real data the presence of missing values is typically the rule than the exception. In our case, most variables have no missing values, but of them do. In this case we can take advantage of some built-in methods to fill these values. An alternative is to remove these rows using the `dropna()` method. Note that missing values represented with a different placeholder (e.g. -99, -9999) will not be detected as NaN. We need to manually fill in these with NaNs using the `fillna()` method or using a boolean index.

>Booleans represent zeros (`False`) and ones (`True`), so two bits. We can actually add them up using the Pandas library using the `sum()` method to summarize the total number of missing values represented as NaN in our dataset.



```python
# Fill missing values
df = df.fillna(method='backfill')

```

For a single column we can simply do `data['TEMP10MAVG'].fillna(method='backfill')`

## Air temperature vs Soil temperature

Together with soil moisture, soil temperature is and important variabl dictating the planting date, particularly of summer crops such as corn. On average crops are seeded at about 5 cm (2 inches or 0.05 meter) depth, and while there is an increasing number of networks monitoring soil temperature, air temperature is by far a more common variable even from backyard weather stations.

So a valid question would be to think whether we can estimate soil temperature at the planting depth using readily available air temperature observations. To find the answer we can use data from stations monitoring both variables to give as an idea of the relationship. We will fit a linear model and see whether the model provides adequate predictive power for common practical applications.



```python
# Compare air temperature at 2 and 10 meters.
plt.scatter(df["TEMP2MAVG"], df["SOILTMP5AVG655"])
plt.xlabel('Air temperature at 2 meters (Celsius)')
plt.ylabel('Soil temperature at 0.05 meters depth (Celsius)')
plt.show()

```


![png](stats_linear_fit_files/stats_linear_fit_10_0.png)



```python
# Fit linear model using linregress
fit_info = stats.linregress(df["TEMP2MAVG"], df["SOILTMP5AVG655"])

# Display individual parameters
print('slope:',fit_info.slope)
print('intercept:',fit_info.intercept)
print('r:',fit_info.rvalue)
print('r-squared:',fit_info.rvalue**2)
print('p-value:',fit_info.pvalue)


```

    slope: 0.8667839222050925
    intercept: 3.503863329087279
    r: 0.9480160047071207
    r-squared: 0.8987343451808514
    p-value: 1.3681157460919794e-182



```python
# Predict soil temperature at 0.05 m based on air temperature at 2 m 
y_pred = fit_info.intercept + fit_info.slope*df["TEMP2MAVG"]

```

Alternatively you can call the `linregress()` method with multiple output arguments: `slope, intercept, r_val, p_val, std_err = stats.linregress(df["TEMP2MAVG"], df["SOILTMP5AVG655"])`. In this case all the outputs will be assigned their own variable name. There is no rule about which method you should use. If you are dealing with multiple models and fitted parameters, it might be a good idea to encapsulate the slopes, intercepts, and goodness of fit into a single variable to simplify variable names and reduce code complexity.



```python
# Figure of data and linear model
plt.scatter(df["TEMP2MAVG"], df["SOILTMP5AVG655"], marker='o', color='b', alpha=0.3, label='obs')
plt.plot(df["TEMP2MAVG"], y_pred, '-k', label='pred')
plt.legend()
plt.xlabel('Air temperature at 2 meters (Celsius)')
plt.ylabel('Soil temperature at 0.05 meters depth (Celsius)')
plt.show()

```


![png](stats_linear_fit_files/stats_linear_fit_14_0.png)


## Examine residuals


```python
# Compute residuals
residuals = df["SOILTMP5AVG655"] - y_pred # Observed - predicted

# Do residuals have a mean of zero?
print(stats.tmean(residuals))

# Always plot residuals
plt.figure()
plt.scatter(df["SOILTMP5AVG655"], residuals, marker='o', color='g', alpha=0.3)
plt.xlabel("Observed air temperature at 2 meters (Celsius) ")
plt.ylabel("Residuals (Celsius) ")
plt.show()

```

    1.5184200928572005e-15



![png](stats_linear_fit_files/stats_linear_fit_16_1.png)


## Compute accuracy

We will take advantage of the built-in median absolute errormetric from the `scipy.stats` module. The median absolute error represents the typical error and is more robust agains outliers than the mean absolute error and the root mean squared error.



```python
# Median absolute error. 
MAE = stats.median_absolute_deviation(residuals)
print("MAE=", round(MAE,2), 'Celsius')

```

    MAE= 3.19 Celsius


## Remarks

- There is a positive relationship between air and soil temperature, as expected. While it seems obvious, note that this may not be true for deeper soil temperatures. It takes time for heat to move to deeper layers and there can be strong delays at greater depths and a linear model may no longer be valid.

- The coefficient of linear correlation (R) between air temperature and soil temperature at 5 cm depth was strong (R = 0.95).

- The low p-value (P<0.001) shows that the regression is statistically significant.

- Despite the overall high linear correlation, the relationship is not that strong at temperatures below freezing. From the plot of residuals we can see up to 10 degrees error. To better capture this pattern we need a non-linear model. Since our scope is in summer crops, then this probably not that important.

- Using data for 2018, the linear model can predict soil temperature at 5 cm depth from air temperature obsrvations at 2 m height with an accuracy of about 3 Celsius across the range of -20 to 30 degrees Celsius.


## Additional exercises

- Can you find other linear relationships in the dataset? Hint: Daily average air temperature (TEMP2MAVG) vs average daily atmospheric pressure (PRESSUREAVG).

- What is the sign of the slope? What does it mean?

