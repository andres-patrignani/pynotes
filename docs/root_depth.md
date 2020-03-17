# Root growth

One of the most important and difficult components of crop models is root growth. Compared to the overwhelming amount of observations and models for above ground vegetation, the information about root growth along the soil profile is fairly limited. Generating accurate estimates of root growth (root length and root distribution) is essential for an accurate computation of the soil water and fertility balance.

Simple root growth model generate estimates of the bulk extent of the root zone in one dimension. More sophistaced root growth models can incoporate a more detailed representation of the root system based on branching patterns, root architecture, and properties of the growing medium.

In this exercise we will implement a simple model as a function of temperature in terms of thermal time (a.k.a. growing-degree days). The model is used in the Aquacrop simulation model and is as follows (Eq. 12 in Steduto et al., 2009):

$$ Z_{TT} = Z_o + (Z_x - Z_o) \sqrt[n]{ \frac{TT-\frac{1}{2}TT_o}{TT_x-\frac{1}{2}TT_o} }$$

$Z$ is the root depth at thermal time *TT*

$Z_o$ is the sowing depth

$Z_x$ is the maximum rooting length

$TT$ is the thermal time

$TT_o$ is the thermal time required until emergence

$TT_x$ is the thermal time required until maximum root length

$n$ is a parameter describing the shape of the root growth curve


## Model assumptions

- Root growth only depends on temperature. This is certainly far from reality in the sense that soil moisture, soil nutrients, and soil penetration resistance exhert an important control on root growth.

- Root development starts before crop emergence. More precisely, root growth is assumed to start half way in terms of thermal time between planting and emergence. This is about right since the first seminal roots appear before the coleoptile or cotiledons appear above the soil surface.



```python
# Import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

```


```python
# Import data
data = pd.read_csv('../datasets/gypsum_ks_daily_2018.csv')
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
# Convert timestamps into pandas datetime
data["TIMESTAMP"] = pd.to_datetime(data["TIMESTAMP"])

```


```python
# Compute and add day of year to dataframe
data.insert(1,'DOY',data["TIMESTAMP"].dt.dayofyear)
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
      <th>TIMESTAMP</th>
      <th>DOY</th>
      <th>STATION</th>
      <th>PRESSUREAVG</th>
      <th>PRESSUREMAX</th>
      <th>PRESSUREMIN</th>
      <th>SLPAVG</th>
      <th>TEMP2MAVG</th>
      <th>TEMP2MMIN</th>
      <th>TEMP2MMAX</th>
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
      <td>2018-01-01</td>
      <td>1</td>
      <td>Gypsum</td>
      <td>99.44</td>
      <td>100.03</td>
      <td>98.73</td>
      <td>104.44</td>
      <td>-15.15</td>
      <td>-19.56</td>
      <td>-11.00</td>
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
      <td>2018-01-02</td>
      <td>2</td>
      <td>Gypsum</td>
      <td>99.79</td>
      <td>100.14</td>
      <td>99.40</td>
      <td>104.88</td>
      <td>-16.48</td>
      <td>-22.10</td>
      <td>-10.40</td>
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
      <td>2018-01-03</td>
      <td>3</td>
      <td>Gypsum</td>
      <td>98.87</td>
      <td>99.52</td>
      <td>97.94</td>
      <td>103.81</td>
      <td>-11.03</td>
      <td>-20.64</td>
      <td>-2.71</td>
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
      <td>2018-01-04</td>
      <td>4</td>
      <td>Gypsum</td>
      <td>98.22</td>
      <td>98.54</td>
      <td>97.90</td>
      <td>102.99</td>
      <td>-5.83</td>
      <td>-11.79</td>
      <td>0.24</td>
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
      <td>2018-01-05</td>
      <td>5</td>
      <td>Gypsum</td>
      <td>98.10</td>
      <td>98.42</td>
      <td>97.75</td>
      <td>102.88</td>
      <td>-4.73</td>
      <td>-14.22</td>
      <td>5.36</td>
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
<p>5 rows × 45 columns</p>
</div>




```python
planting_doy = 100 # About April 1
crop_season_length = 110 # days 
harvest_doy = planting_doy + crop_season_length + 1

idx_growing_season = (data["DOY"] > planting_doy) & (data["DOY"] < harvest_doy)
data = data.loc[idx_growing_season,:]
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
      <th>TIMESTAMP</th>
      <th>DOY</th>
      <th>STATION</th>
      <th>PRESSUREAVG</th>
      <th>PRESSUREMAX</th>
      <th>PRESSUREMIN</th>
      <th>SLPAVG</th>
      <th>TEMP2MAVG</th>
      <th>TEMP2MMIN</th>
      <th>TEMP2MMAX</th>
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
      <th>100</th>
      <td>2018-04-11</td>
      <td>101</td>
      <td>Gypsum</td>
      <td>97.72</td>
      <td>98.38</td>
      <td>96.88</td>
      <td>102.27</td>
      <td>8.83</td>
      <td>-6.67</td>
      <td>23.80</td>
      <td>...</td>
      <td>11.59</td>
      <td>4.80</td>
      <td>7.97</td>
      <td>7.87</td>
      <td>7.61</td>
      <td>7.91</td>
      <td>0.1684</td>
      <td>0.1610</td>
      <td>0.2495</td>
      <td>0.2116</td>
    </tr>
    <tr>
      <th>101</th>
      <td>2018-04-12</td>
      <td>102</td>
      <td>Gypsum</td>
      <td>96.43</td>
      <td>97.09</td>
      <td>95.90</td>
      <td>100.74</td>
      <td>18.63</td>
      <td>8.78</td>
      <td>28.30</td>
      <td>...</td>
      <td>13.85</td>
      <td>8.74</td>
      <td>11.19</td>
      <td>10.78</td>
      <td>9.43</td>
      <td>8.34</td>
      <td>0.1705</td>
      <td>0.1614</td>
      <td>0.2519</td>
      <td>0.2116</td>
    </tr>
    <tr>
      <th>102</th>
      <td>2018-04-13</td>
      <td>103</td>
      <td>Gypsum</td>
      <td>95.44</td>
      <td>96.09</td>
      <td>94.74</td>
      <td>99.63</td>
      <td>22.00</td>
      <td>8.04</td>
      <td>30.96</td>
      <td>...</td>
      <td>16.97</td>
      <td>11.38</td>
      <td>14.26</td>
      <td>13.69</td>
      <td>11.59</td>
      <td>9.33</td>
      <td>0.1723</td>
      <td>0.1618</td>
      <td>0.2548</td>
      <td>0.2127</td>
    </tr>
    <tr>
      <th>103</th>
      <td>2018-04-14</td>
      <td>104</td>
      <td>Gypsum</td>
      <td>95.18</td>
      <td>96.07</td>
      <td>94.60</td>
      <td>99.37</td>
      <td>16.31</td>
      <td>2.06</td>
      <td>23.92</td>
      <td>...</td>
      <td>18.60</td>
      <td>14.49</td>
      <td>16.27</td>
      <td>15.78</td>
      <td>13.85</td>
      <td>10.71</td>
      <td>0.1727</td>
      <td>0.1608</td>
      <td>0.2577</td>
      <td>0.2143</td>
    </tr>
    <tr>
      <th>104</th>
      <td>2018-04-15</td>
      <td>105</td>
      <td>Gypsum</td>
      <td>96.54</td>
      <td>97.37</td>
      <td>95.81</td>
      <td>101.09</td>
      <td>-0.41</td>
      <td>-2.50</td>
      <td>2.67</td>
      <td>...</td>
      <td>14.49</td>
      <td>7.38</td>
      <td>10.59</td>
      <td>10.76</td>
      <td>12.29</td>
      <td>11.59</td>
      <td>0.1655</td>
      <td>0.1526</td>
      <td>0.2544</td>
      <td>0.2152</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 45 columns</p>
</div>




```python
# Define function

def rootgrowth(par,T):
    
    # Pre-allocate variables
    TT = np.ones(len(T)) * np.nan
    TT_cum = np.ones(len(T)) * np.nan  
    root_depth = np.ones(len(T)) * np.nan 

    # Initial conditions
    TT[0] = T[0]
    TT_cum[0] = TT[0]
    root_depth[0] = par["Zsowing"]

    for i in range(1,len(T)):

        # Compute thermal time based on temeprature
        TT_step = np.maximum(T[i] - par['Tbase'], 0)
        TT[i] = TT_step
        TT_cum[i] = TT_cum[i-1] + TT_step

        # Compute root growth
        TTradical = np.maximum(TT_cum[i] - par['TTemerge']/2, 0) / (par['TTmax'] - par['TTemerge']/2)
        root_depth[i] = np.minimum(par['Zsowing'] + (par['Zmax'] - par['Zsowing']) * TTradical**par['Zshape'], par['Zmax'])

    return dict({"root_depth":root_depth, "TT":TT, "TT_cum":TT_cum})


```


```python
# Invoke function
par = {'TTemerge':100,
       'TTmax':1000,
       'Zsowing':5,
       'Zmax':200,
       'Tbase':10,
       'Zshape':0.5}

Z = rootgrowth(par,data.TEMP2MAVG.values)

```


```python
plt.figure(figsize=(10,6))
plt.plot(Z["TT_cum"], Z["root_depth"]*-1)
plt.axvline(par["TTemerge"]/2, color='k', linestyle='--')
plt.axvline(par["TTmax"], color='k', linestyle='--')
plt.xlabel("Thermal time (Cd)", size=18)
plt.ylabel("Root length (cm)", size=18)
plt.xticks(fontsize=14)
plt.yticks(fontsize=14)
plt.show()

```


![png](root_depth_files/root_depth_8_0.png)


## References

Steduto, P., Hsiao, T.C., Raes, D. and Fereres, E., 2009. AquaCrop—The FAO crop model to simulate yield response to water: I. Concepts and underlying principles. Agronomy Journal, 101(3), pp.426-437.
