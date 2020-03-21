# Soil Water Storage

Example for calculation of soil water storage using soil moisture data recorded at two different dates and at ten different soil depths using a neutron probe in a wheat field located near Lahoma, OK. The goal is to integrate the soil moisture values along the soil profile for each date to calculate the soil water storage. 

We will then subtract the soil water storage from two consecutive dates to calculate the change in soil water storage.



```python
# Import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

```


```python
# Read file with soil moisture data
df = pd.read_csv('../datasets/profile_soil_moisture_lahoma_ntw.csv')
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
      <th>profile_layer</th>
      <th>7/2/2009</th>
      <th>7/10/2009</th>
      <th>7/16/2009</th>
      <th>7/24/2009</th>
      <th>7/30/2009</th>
      <th>8/7/2009</th>
      <th>8/12/2009</th>
      <th>8/21/2009</th>
      <th>8/28/2009</th>
      <th>...</th>
      <th>5/21/2011</th>
      <th>5/28/2011</th>
      <th>5/31/2011</th>
      <th>6/9/2011</th>
      <th>6/23/2011</th>
      <th>7/14/2011</th>
      <th>8/2/2011</th>
      <th>8/19/2011</th>
      <th>10/13/2011</th>
      <th>10/18/2011</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0-20</td>
      <td>0.135</td>
      <td>0.198</td>
      <td>0.159</td>
      <td>0.208</td>
      <td>0.243</td>
      <td>0.205</td>
      <td>0.251</td>
      <td>0.248</td>
      <td>0.203</td>
      <td>...</td>
      <td>0.306</td>
      <td>0.329</td>
      <td>0.306</td>
      <td>0.260</td>
      <td>0.319</td>
      <td>0.302</td>
      <td>0.206</td>
      <td>0.320</td>
      <td>0.347</td>
      <td>0.327</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20-40</td>
      <td>0.172</td>
      <td>0.240</td>
      <td>0.237</td>
      <td>0.276</td>
      <td>0.274</td>
      <td>0.291</td>
      <td>0.318</td>
      <td>0.321</td>
      <td>0.315</td>
      <td>...</td>
      <td>0.236</td>
      <td>0.274</td>
      <td>0.270</td>
      <td>0.262</td>
      <td>0.288</td>
      <td>0.278</td>
      <td>0.241</td>
      <td>0.280</td>
      <td>0.318</td>
      <td>0.306</td>
    </tr>
    <tr>
      <th>2</th>
      <td>40-60</td>
      <td>0.228</td>
      <td>0.237</td>
      <td>0.237</td>
      <td>0.257</td>
      <td>0.259</td>
      <td>0.277</td>
      <td>0.327</td>
      <td>0.338</td>
      <td>0.329</td>
      <td>...</td>
      <td>0.268</td>
      <td>0.295</td>
      <td>0.294</td>
      <td>0.290</td>
      <td>0.298</td>
      <td>0.289</td>
      <td>0.287</td>
      <td>0.290</td>
      <td>0.330</td>
      <td>0.322</td>
    </tr>
    <tr>
      <th>3</th>
      <td>60-80</td>
      <td>0.260</td>
      <td>0.262</td>
      <td>0.262</td>
      <td>0.268</td>
      <td>0.267</td>
      <td>0.269</td>
      <td>0.301</td>
      <td>0.333</td>
      <td>0.328</td>
      <td>...</td>
      <td>0.252</td>
      <td>0.273</td>
      <td>0.273</td>
      <td>0.271</td>
      <td>0.275</td>
      <td>0.262</td>
      <td>0.267</td>
      <td>0.264</td>
      <td>0.304</td>
      <td>0.299</td>
    </tr>
    <tr>
      <th>4</th>
      <td>80-100</td>
      <td>0.157</td>
      <td>0.175</td>
      <td>0.179</td>
      <td>0.194</td>
      <td>0.192</td>
      <td>0.198</td>
      <td>0.217</td>
      <td>0.320</td>
      <td>0.307</td>
      <td>...</td>
      <td>0.150</td>
      <td>0.172</td>
      <td>0.175</td>
      <td>0.166</td>
      <td>0.196</td>
      <td>0.164</td>
      <td>0.179</td>
      <td>0.169</td>
      <td>0.190</td>
      <td>0.185</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 63 columns</p>
</div>




```python
# Create array with depths
depths = np.arange(0,200,20) # depths in cm
print(depths)

```

    [  0  20  40  60  80 100 120 140 160 180]


## Trapezoidal integration

Before calculating the soil water storage for all dates we will first compute the storage for a single date to ensure our calculations are correct.

The trapezoidal rule is a discrete integration method that basically adds up a collection of trapezoids. The narrower the intervals the more accurate the method, particularly when dealing with sudden non-linear changes.

<img src="https://upload.wikimedia.org/wikipedia/commons/7/7e/Trapezium2.gif?1582508631465" />

**Figure**: An animated gif showing how the progressive reduction in step size increases the accuracy of the approximated area below the function. Khurram Wadee (2014). This file is licensed under the Creative Commons Attribution-Share Alike 3.0 Unported license.



```python
vwc_1 = df["7/2/2009"].values # volumetric water content
storage_1 = np.trapz(vwc_1,depths) # total profile soil water storage in cm

print(storage_1,"cm of water in 2-Jul-2009")
```

    39.77 cm of water in 2-Jul-2009



```python
vwc_2 = df["7/10/2009"].values # volumetric water content
storage_2 = np.trapz(vwc_2,depths) # total profile soil water storage in cm

print(storage_2,"cm of water in 10-Jul-2009")
```

    43.03 cm of water in 10-Jul-2009



```python
# Plot profile
plt.figure(figsize=(8,6))
plt.plot(vwc_1,depths*-1, '-k', label="2-Jul-2009")
plt.plot(vwc_2,depths*-1, '--k', label="10-Jul-2009")
plt.xlabel('Volumetric water content (cm$^3$ cm$^{-3}$)', size=16)
plt.ylabel('Soil depth (cm)', size=16)
plt.xticks(fontsize=16)
plt.yticks(fontsize=16)
plt.legend()
plt.fill_betweenx(depths*-1, vwc_1, vwc_2, facecolor=(0.7,0.7,0.7), alpha=0.25)
plt.xlim(0,0.5)
plt.show()

```


![png](soil_water_storage_files/soil_water_storage_7_0.png)



```python
# Compute total soil water storage for each date
storage = np.array([])
for date in range(1,len(df.columns)):
    storage_date = np.round(np.trapz(df.iloc[:,date], depths), 2)
    storage = np.append(storage,storage_date)
    
storage
```




    array([39.77, 43.03, 42.84, 44.93, 44.9 , 45.57, 48.42, 55.03, 53.09,
           52.92, 51.87, 51.24, 51.2 , 54.2 , 53.82, 54.4 , 53.93, 53.71,
           52.37, 51.67, 51.57, 52.91, 48.94, 48.38, 46.59, 42.89, 47.29,
           47.61, 45.1 , 45.69, 51.08, 50.9 , 49.9 , 53.62, 52.32, 52.53,
           53.1 , 52.12, 55.43, 54.  , 53.05, 51.87, 49.45, 50.56, 48.79,
           49.66, 49.49, 49.36, 45.03, 41.13, 40.79, 41.34, 42.24, 43.95,
           43.79, 43.23, 44.51, 43.31, 42.84, 43.64, 46.85, 45.5 ])




```python
# Get measurement dates and convert them to datetime format
obs_dates = pd.to_datetime(df.columns[1:])
obs_delta = obs_dates - obs_dates[0]
obs_seq = obs_delta.days
print(len(obs_seq))

```

    62



```python
# Plot timeseries of profile soil moisture
plt.figure(figsize=(10,4))
plt.plot(obs_dates, storage)
plt.ylabel('Storage (cm)')
plt.show()

```


![png](soil_water_storage_files/soil_water_storage_10_0.png)



```python
# Y values
#y = np.tile(depths*-1,62)
y = np.repeat(depths*-1,62)
y.shape

```




    (620,)




```python
# X values
x = np.tile(obs_seq,10)
x.shape

```




    (620,)




```python
# Z values
z = df.iloc[:,1:].values.flatten()
z.shape

```




    (620,)



## Contour plot


```python
plt.figure(figsize=(36,8))
plt.tricontour(x, y, z, levels=14, linewidths=0.5, colors='k')
plt.tricontourf(x, y, z, levels=14, cmap="RdBu")
plt.xticks(obs_seq, labels=obs_dates.date, rotation=90)
plt.colorbar(label="Volumetric Water Content")
plt.ylabel('Soil depth (cm)', size=16)
plt.yticks(fontsize=16)
plt.show()
```


![png](soil_water_storage_files/soil_water_storage_15_0.png)


## References

Patrignani, A., Godsey, C.B., Ochsner, T.E. and Edwards, J.T., 2012. Soil water dynamics of conventional and no-till wheat in the Southern Great Plains. Soil Science Society of America Journal, 76(5), pp.1768-1775.

Yimam, Y.T., Ochsner, T.E., Kakani, V.G. and Warren, J.G., 2014. Soil water dynamics and evapotranspiration under annual and perennial bioenergy crops. Soil Science Society of America Journal, 78(5), pp.1584-1593.

