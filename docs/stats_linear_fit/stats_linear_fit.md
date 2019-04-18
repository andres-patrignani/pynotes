
# **Linear fitting**

A linear model is one of the simplest ways to model the dependence of two variables and often times constitute the first guess when dealing with variables for which we know little about their relationship. If it is not linear, then we move towards more complex models.

In this exercise we will look at the correlation between different atmospheric variables using data from the Gypsum station of the Kansas Mesonet for 2018. Each station records many variables, so in this exercise we can try multiple relationships.

For instance, how closely related is the air temperature at a height of 10 meters relative to the air temperature at 2 meters? If the degree of correlation is high, then we may not need to install a sensor at 10 meters and we can just focus on the more convenient installation of a sensor at 2 meters. This would also save us some money since research-grade sensors are expensive.

To fit a linear model we will use the `stats` module within `scipy`. This module is easy to use and also returns some basic metrics of the goodness of fit.

>The linear fitting routine cannot handle NaN values, so we will need to replace missing data before fitting a linear model. For simplicity we will replace missing values with the value of the preceding day. This is not the best approach, but it should not introduce much error if the dataset has only few missing values.

## References
[Scipy docs](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.linregress.html#scipy.stats.linregress)



```python
%matplotlib inline
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from scipy import stats
```


```python
data = pd.read_csv('/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/gypsum_ks_daily_2018.csv')

```


```python
# Explore headers
data.head(5)
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
data.tail(5)
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
# Fill missing values
x_obs = data['TEMP2MAVG'].fillna(method='backfill')
y_obs = data['TEMP10MAVG'].fillna(method='backfill')
```


```python
# Compare air temperature at 2 and 10 meters.
plt.scatter(x_obs, y_obs, label='obs')
plt.xlabel('Air temperature at 2 meters (Celsius)')
plt.ylabel('Air temperature at 10 meters (Celsius)')
plt.show()
```


![png](output_6_0.png)



```python
# Fit linear model using linregress
slope, intercept, r_val, p_val, std_err = stats.linregress(x_obs, y_obs)
print('slope:',slope)
print('intercept:',intercept)
print('r:',r_val)
print('r-squared:',r_val**2)

```

    slope: 0.9951679617412289
    intercept: 0.24828997625776061
    r: 0.9987226605452462
    r-squared: 0.997446952686575



```python
# Predict air temperature at 10 m (y_pred) based on temperature at 2 m (x_obs)
y_pred = intercept + slope*x_obs
```


```python
# Figure of data and linear model
plt.plot(x_obs, y_obs, 'o', label='obs')
plt.plot(x_obs, y_pred, 'r', label='pred')
plt.legend()
plt.xlabel('Air temperature at 2 meters (Celsius)')
plt.ylabel('Air temperature at 10 meters (Celsius)')
plt.show()
```


![png](output_9_0.png)


You can try other variables:
- how close is the relationship between between air temperature at 2 meters and air relative humidity at 2 meters? 

- Can we predict soil temperature at 5 cm depth from air temperature?

- Is atmospheric pressure related to air temperature?


```python
# Re-define y_obs with a new variable.

y_obs = data['PRESSUREAVG'].fillna(method='backfill')
```


```python
fit_info = stats.linregress(x_obs, y_obs)
fit_info
```




    LinregressResult(slope=-0.03174985091216577, intercept=97.66035004164551, rvalue=-0.5201324155291819, pvalue=1.090025613211316e-26, stderr=0.0027363766395771005)




```python
y_pred = fit_info.intercept + fit_info.slope*x_obs
```

By calling `linregress` with a single output variable we can group all the parameters into a single object. This can be handy in order to keep our code a bit cleaner.


```python
# Figure of data and linear model
plt.plot(x_obs, y_obs, 'o', label='obs')
plt.plot(x_obs, y_pred, 'r', label='pred')
plt.legend()
plt.xlabel('Air temperature at 2 meters (Celsius)')
plt.ylabel('Air Relative humidity at 2 meters (%)')
plt.show()
```


![png](output_15_0.png)



```python
print('r-value:', round(fit_info.rvalue,3))
```

    r-value: -0.52


There is some evident relationship, but not nearly as strong as the previous one between air temperature at 2 and 10 meters. We can check the p-value to see if this apparently weaker linear correlation is statistically significant or not.


```python
print('p-value:', fit_info.pvalue)
```

    p-value: 1.090025613211316e-26


Indeed, the linear regression model between air temperature and air pressure is highly significant.
