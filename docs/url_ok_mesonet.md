# Oklahoma mesonet
 One of the longest running mesonets in the world.
 
 Follw terms of service
http://www.mesonet.org/index.php/site/about/terms_of_use


```python
import pandas as pd
from bokeh.io import output_notebook
from bokeh.models import LogColorMapper, LogTicker, ColorBar
from bokeh.palettes import Viridis6
from bokeh.plotting import figure, show
from bokeh.sampledata.us_counties import data as counties
output_notebook()

```



<div class="bk-root">
    <a href="https://bokeh.pydata.org" target="_blank" class="bk-logo bk-logo-small bk-logo-notebook"></a>
    <span id="a69a12be-9d49-4715-80d0-8884e19de65a">Loading BokehJS ...</span>
</div>





```python
# Retrieve current weather conditions
url = 'http://www.mesonet.org/data/public/mesonet/current/current.csv.txt'
df = pd.read_csv(url)
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
      <th>STID</th>
      <th>NAME</th>
      <th>ST</th>
      <th>LAT</th>
      <th>LON</th>
      <th>YR</th>
      <th>MO</th>
      <th>DA</th>
      <th>HR</th>
      <th>MI</th>
      <th>...</th>
      <th>RELH</th>
      <th>CHIL</th>
      <th>HEAT</th>
      <th>WDIR</th>
      <th>WSPD</th>
      <th>WMAX</th>
      <th>PRES</th>
      <th>TMAX</th>
      <th>TMIN</th>
      <th>RAIN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ACME</td>
      <td>Acme</td>
      <td>OK</td>
      <td>34.81</td>
      <td>-98.02</td>
      <td>2020</td>
      <td>1</td>
      <td>18</td>
      <td>18</td>
      <td>55</td>
      <td>...</td>
      <td>58</td>
      <td>30</td>
      <td></td>
      <td>N</td>
      <td>7</td>
      <td>8</td>
      <td>1030.24</td>
      <td>49</td>
      <td>36</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>ADAX</td>
      <td>Ada</td>
      <td>OK</td>
      <td>34.80</td>
      <td>-96.67</td>
      <td>2020</td>
      <td>1</td>
      <td>18</td>
      <td>18</td>
      <td>55</td>
      <td>...</td>
      <td>47</td>
      <td>34</td>
      <td></td>
      <td>N</td>
      <td>8</td>
      <td>11</td>
      <td>1029.38</td>
      <td>56</td>
      <td>38</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ALTU</td>
      <td>Altus</td>
      <td>OK</td>
      <td>34.59</td>
      <td>-99.34</td>
      <td>2020</td>
      <td>1</td>
      <td>18</td>
      <td>18</td>
      <td>55</td>
      <td>...</td>
      <td>67</td>
      <td>33</td>
      <td></td>
      <td>NE</td>
      <td>8</td>
      <td>9</td>
      <td>1030.40</td>
      <td>49</td>
      <td>36</td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>ALV2</td>
      <td>Alva</td>
      <td>OK</td>
      <td>36.71</td>
      <td>-98.71</td>
      <td>2020</td>
      <td>1</td>
      <td>18</td>
      <td>18</td>
      <td>55</td>
      <td>...</td>
      <td>59</td>
      <td>31</td>
      <td></td>
      <td>NNE</td>
      <td>5</td>
      <td>6</td>
      <td>1030.59</td>
      <td>46</td>
      <td>32</td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>ANT2</td>
      <td>Antlers</td>
      <td>OK</td>
      <td>34.25</td>
      <td>-95.67</td>
      <td>2020</td>
      <td>1</td>
      <td>18</td>
      <td>18</td>
      <td>55</td>
      <td>...</td>
      <td>47</td>
      <td>38</td>
      <td></td>
      <td>NNW</td>
      <td>5</td>
      <td>9</td>
      <td>1028.93</td>
      <td>61</td>
      <td>41</td>
      <td>0.02</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>




```python
# Get counties from sample dataset
counties = {
    code: county for code, county in counties.items() if county["state"] == "ok"
}

county_xs = [county["lons"] for county in counties.values()]
county_ys = [county["lats"] for county in counties.values()]
county_names = [county['name'] for county in counties.values()]


color_mapper = LogColorMapper(palette=Viridis6)

TOOLS = "pan,wheel_zoom,reset,hover,save"

p = figure(
    title="Current weather conditions", tools=TOOLS,
    x_axis_location=None, y_axis_location=None,
    match_aspect=True)

p.grid.grid_line_color = None
#p.hover.point_policy = "follow_mouse"

p.patches(county_xs, county_ys,
          fill_alpha=0.7, 
          line_color="white", 
          line_width=0.5)

p.circle(x='LON', y='LAT', 
         size=15, 
         fill_color={'field': 'WSPD', 'transform': color_mapper}, 
         fill_alpha=0.8, 
         source=df)

color_bar = ColorBar(color_mapper=color_mapper, ticker=LogTicker(),
                     label_standoff=12, border_line_color=None, location=(0,0))

p.add_layout(color_bar, 'right')

show(p)
```








<div class="bk-root" id="c082e184-dc1b-44c3-9a19-b8e32c366baf"></div>




