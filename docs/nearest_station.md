# Find Nearest Monitoring Station

Once in a while when working with weather data we face the need to identify the nearest station to a specific place, landmark, farm, or city.

Calculating the true shortest distance between two points on the Earth's surface requires detailed information about terrain topography and the Earth's ellipticity, which would require accessing additional datasets. In most cases we are only interested in approximate distances to allow us identifying the nearest station. The haversine formula is perhaps one of the most widely used approaches to approximate the great-circle distance between two points on a sphere given their longitudes and latitudes. This calculation is approximate since the  rotating Earth has the shape of an oblate spheroid rather than a sphere, but it will suffice our application.



```python
# Import modules
import pandas as pd
import numpy as np

```


```python
# Import stations from the Soil Climate Analysis Network
scan = pd.read_csv('../datasets/SCAN_stations_geoinfo.csv')
scan.head()

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
      <th>network</th>
      <th>state</th>
      <th>county</th>
      <th>site_name</th>
      <th>start</th>
      <th>lat</th>
      <th>lon</th>
      <th>elev</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SCAN</td>
      <td>AK</td>
      <td>Bethel</td>
      <td>Aniak</td>
      <td>2002</td>
      <td>61.58</td>
      <td>-159.58</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SCAN</td>
      <td>AK</td>
      <td>Bethel</td>
      <td>Canyon Lake</td>
      <td>2014</td>
      <td>59.42</td>
      <td>-161.16</td>
      <td>550</td>
    </tr>
    <tr>
      <th>2</th>
      <td>SCAN</td>
      <td>AK</td>
      <td>Nome</td>
      <td>Checkers Creek</td>
      <td>2014</td>
      <td>65.40</td>
      <td>-164.71</td>
      <td>326</td>
    </tr>
    <tr>
      <th>3</th>
      <td>SCAN</td>
      <td>AK</td>
      <td>Yukon-koyukuk</td>
      <td>Hozatka Lake</td>
      <td>2014</td>
      <td>65.20</td>
      <td>-156.63</td>
      <td>206</td>
    </tr>
    <tr>
      <th>4</th>
      <td>SCAN</td>
      <td>AK</td>
      <td>Yukon-koyukuk</td>
      <td>Innoko Camp</td>
      <td>2014</td>
      <td>63.64</td>
      <td>-158.03</td>
      <td>83</td>
    </tr>
  </tbody>
</table>
</div>




```python
def haversine(lat_point,lon_point,lat_list, lon_list):
    
    """Haversine function: Computes distance between the geogrpahic
    coordinates of two points on a sphere or a point and list of points.
    
    Inputs: geographic coordinates in decimal degrees.
    Output: Approximate distance in kilometers"""
    
    # Convert point coordinates to radians
    lat_point = np.radians(lat_point)
    lon_point = np.radians(lon_point)

    # Convert list coordinates to radians
    lat_list = np.radians(lat_list)
    lon_list = np.radians(lon_list)

    # Compute deltas to simplify equation below
    lat_delta = lat_list - lat_point
    lon_delta = lon_list - lon_point

    # Define average Earth radius in kilometers
    earth_radius = (6356.752 + 6378.137)/2 # (Radius at the poles + Radius at the Ecuator)/2

    # Haversine formula
    a = (np.sin(lat_delta/2))**2 + np.cos(lat_point) * np.cos(lat_list) * (np.sin(lon_delta/2))**2
    d = 2 * earth_radius * np.arcsin(np.sqrt(a))
    return d

```


```python
# Define a location.
# Latitude and Longitude for the geogrpahic center for the US, which is located in Kansas.
lat_center_usa = 39.828344
lon_center_usa = -98.579473

```


```python
# Compute distances using the haversine function defined above
distances = haversine(lat_center_usa, lon_center_usa, scan["lat"], scan["lon"])

```


```python
# Find shortest distance to point (find nearest station)
idx_nearest = np.argmin(distances)

# Summary
print('The nearest SCAN station is', scan.loc[idx_nearest,"site_name"], '(about',
      round(distances[idx_nearest]), 'km from the point of interest)')

# Print station details
print(scan.loc[idx_nearest])
```

    The nearest SCAN station is Phillipsburg (about 64.0 km from the point of interest)
    network              SCAN
    state                  KS
    county           Phillips
    site_name    Phillipsburg
    start                2004
    lat                 39.79
    lon                -99.33
    elev                 1986
    Name: 78, dtype: object


## References

Haversine formula: https://www.wikiwand.com/en/Haversine_formula

Versines: https://www.wikiwand.com/en/Versine

