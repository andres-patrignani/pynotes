```python
import pandas as pd
import numpy as np

```


```python
df = pd.read_csv('../datasets/ok_mesonet_8_apr_2019.csv', sep=',')

```


```python
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
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>ADAX</td>
      <td>Ada</td>
      <td>OK</td>
      <td>34.80</td>
      <td>-96.67</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>40</td>
      <td></td>
      <td></td>
      <td>S</td>
      <td>12</td>
      <td>20</td>
      <td>1011.13</td>
      <td>78</td>
      <td>48</td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>ALTU</td>
      <td>Altus</td>
      <td>OK</td>
      <td>34.59</td>
      <td>-99.34</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>39</td>
      <td></td>
      <td>82</td>
      <td>SSW</td>
      <td>19</td>
      <td>26</td>
      <td>1007.86</td>
      <td>82</td>
      <td>45</td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>ALV2</td>
      <td>Alva</td>
      <td>OK</td>
      <td>36.71</td>
      <td>-98.71</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>32</td>
      <td></td>
      <td>82</td>
      <td>S</td>
      <td>20</td>
      <td>26</td>
      <td>1004.65</td>
      <td>84</td>
      <td>40</td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>ANT2</td>
      <td>Antlers</td>
      <td>OK</td>
      <td>34.25</td>
      <td>-95.67</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>35</td>
      <td></td>
      <td></td>
      <td>S</td>
      <td>11</td>
      <td>20</td>
      <td>1013.64</td>
      <td>78</td>
      <td>38</td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



Some columns with empty cells. Ideally we would like to represent missing values with `NaN`. Be filling this cell we need to identify whether they are truly empty or something else is there (e.g. empty string).


```python
# Print one the cells to see what's in there
df.loc[0,'RAIN']
```




    ' '



There is a string with a single space. Now we can use the `replace()` method to substitute these strings for `NaN` from the Numpy module.

The `inplace=True` replaces the string with `NaN` without generating a copy of the Pandas DataFrame


```python
df.replace(' ', np.nan, inplace=True)
```


```python
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
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ADAX</td>
      <td>Ada</td>
      <td>OK</td>
      <td>34.80</td>
      <td>-96.67</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>40</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>12</td>
      <td>20</td>
      <td>1011.13</td>
      <td>78</td>
      <td>48</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ALTU</td>
      <td>Altus</td>
      <td>OK</td>
      <td>34.59</td>
      <td>-99.34</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>39</td>
      <td>NaN</td>
      <td>82</td>
      <td>SSW</td>
      <td>19</td>
      <td>26</td>
      <td>1007.86</td>
      <td>82</td>
      <td>45</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ALV2</td>
      <td>Alva</td>
      <td>OK</td>
      <td>36.71</td>
      <td>-98.71</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>32</td>
      <td>NaN</td>
      <td>82</td>
      <td>S</td>
      <td>20</td>
      <td>26</td>
      <td>1004.65</td>
      <td>84</td>
      <td>40</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ANT2</td>
      <td>Antlers</td>
      <td>OK</td>
      <td>34.25</td>
      <td>-95.67</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>35</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>11</td>
      <td>20</td>
      <td>1013.64</td>
      <td>78</td>
      <td>38</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



## Match specific stations


```python
idx_acme = df['STID'].str.match('ACME')
df[idx_acme]
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
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 22 columns</p>
</div>




```python
idx_starts_with_A = df['STID'].str.match('A')
df[idx_starts_with_A]
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
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ADAX</td>
      <td>Ada</td>
      <td>OK</td>
      <td>34.80</td>
      <td>-96.67</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>40</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>12</td>
      <td>20</td>
      <td>1011.13</td>
      <td>78</td>
      <td>48</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ALTU</td>
      <td>Altus</td>
      <td>OK</td>
      <td>34.59</td>
      <td>-99.34</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>39</td>
      <td>NaN</td>
      <td>82</td>
      <td>SSW</td>
      <td>19</td>
      <td>26</td>
      <td>1007.86</td>
      <td>82</td>
      <td>45</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ALV2</td>
      <td>Alva</td>
      <td>OK</td>
      <td>36.71</td>
      <td>-98.71</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>32</td>
      <td>NaN</td>
      <td>82</td>
      <td>S</td>
      <td>20</td>
      <td>26</td>
      <td>1004.65</td>
      <td>84</td>
      <td>40</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ANT2</td>
      <td>Antlers</td>
      <td>OK</td>
      <td>34.25</td>
      <td>-95.67</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>35</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>11</td>
      <td>20</td>
      <td>1013.64</td>
      <td>78</td>
      <td>38</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>APAC</td>
      <td>Apache</td>
      <td>OK</td>
      <td>34.91</td>
      <td>-98.29</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>41</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>23</td>
      <td>29</td>
      <td>1008.9</td>
      <td>80</td>
      <td>49</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>ARD2</td>
      <td>Ardmore</td>
      <td>OK</td>
      <td>34.19</td>
      <td>-97.09</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>41</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>18</td>
      <td>26</td>
      <td>1011.43</td>
      <td>77</td>
      <td>50</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ARNE</td>
      <td>Arnett</td>
      <td>OK</td>
      <td>36.07</td>
      <td>-99.90</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>10</td>
      <td>NaN</td>
      <td>85</td>
      <td>SW</td>
      <td>22</td>
      <td>32</td>
      <td>1005.13</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 22 columns</p>
</div>




```python
idx_has_A = df['STID'].str.contains('A')
df[idx_has_A].head(15)
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
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ADAX</td>
      <td>Ada</td>
      <td>OK</td>
      <td>34.80</td>
      <td>-96.67</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>40</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>12</td>
      <td>20</td>
      <td>1011.13</td>
      <td>78</td>
      <td>48</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ALTU</td>
      <td>Altus</td>
      <td>OK</td>
      <td>34.59</td>
      <td>-99.34</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>39</td>
      <td>NaN</td>
      <td>82</td>
      <td>SSW</td>
      <td>19</td>
      <td>26</td>
      <td>1007.86</td>
      <td>82</td>
      <td>45</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ALV2</td>
      <td>Alva</td>
      <td>OK</td>
      <td>36.71</td>
      <td>-98.71</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>32</td>
      <td>NaN</td>
      <td>82</td>
      <td>S</td>
      <td>20</td>
      <td>26</td>
      <td>1004.65</td>
      <td>84</td>
      <td>40</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ANT2</td>
      <td>Antlers</td>
      <td>OK</td>
      <td>34.25</td>
      <td>-95.67</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>35</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>11</td>
      <td>20</td>
      <td>1013.64</td>
      <td>78</td>
      <td>38</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>APAC</td>
      <td>Apache</td>
      <td>OK</td>
      <td>34.91</td>
      <td>-98.29</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>41</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>23</td>
      <td>29</td>
      <td>1008.9</td>
      <td>80</td>
      <td>49</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>ARD2</td>
      <td>Ardmore</td>
      <td>OK</td>
      <td>34.19</td>
      <td>-97.09</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>41</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>18</td>
      <td>26</td>
      <td>1011.43</td>
      <td>77</td>
      <td>50</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ARNE</td>
      <td>Arnett</td>
      <td>OK</td>
      <td>36.07</td>
      <td>-99.90</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>10</td>
      <td>NaN</td>
      <td>85</td>
      <td>SW</td>
      <td>22</td>
      <td>32</td>
      <td>1005.13</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>BEAV</td>
      <td>Beaver</td>
      <td>OK</td>
      <td>36.80</td>
      <td>-100.53</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>9</td>
      <td>NaN</td>
      <td>84</td>
      <td>SW</td>
      <td>17</td>
      <td>26</td>
      <td>1003.9</td>
      <td>91</td>
      <td>34</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>BLAC</td>
      <td>Blackwell</td>
      <td>OK</td>
      <td>36.75</td>
      <td>-97.25</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>38</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SSW</td>
      <td>15</td>
      <td>23</td>
      <td>1007.02</td>
      <td>80</td>
      <td>44</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>BYAR</td>
      <td>Byars</td>
      <td>OK</td>
      <td>34.85</td>
      <td>-97.00</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>43</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>22</td>
      <td>32</td>
      <td>1010.64</td>
      <td>77</td>
      <td>49</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>CAMA</td>
      <td>Camargo</td>
      <td>OK</td>
      <td>36.03</td>
      <td>-99.35</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>32</td>
      <td>NaN</td>
      <td>82</td>
      <td>S</td>
      <td>23</td>
      <td>29</td>
      <td>1005.56</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>CARL</td>
      <td>Lake Carl Blackwell</td>
      <td>OK</td>
      <td>36.15</td>
      <td>-97.29</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>36</td>
      <td>NaN</td>
      <td>80</td>
      <td>S</td>
      <td>17</td>
      <td>25</td>
      <td>1007.56</td>
      <td>80</td>
      <td>50</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>CHAN</td>
      <td>Chandler</td>
      <td>OK</td>
      <td>35.65</td>
      <td>-96.80</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>37</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SSW</td>
      <td>16</td>
      <td>27</td>
      <td>1009.35</td>
      <td>80</td>
      <td>48</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>CLAY</td>
      <td>Clayton</td>
      <td>OK</td>
      <td>34.66</td>
      <td>-95.33</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>S</td>
      <td>9</td>
      <td>24</td>
      <td>1012.9</td>
      <td>78</td>
      <td>40</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 22 columns</p>
</div>




```python
idx = df['NAME'].str.contains('Blackwell') & df['NAME'].str.contains('Lake')
df[idx]

# The following line won't work
# idx = df['NAME'].str.contains('LAKE')
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
      <th>22</th>
      <td>CARL</td>
      <td>Lake Carl Blackwell</td>
      <td>OK</td>
      <td>36.15</td>
      <td>-97.29</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>36</td>
      <td>NaN</td>
      <td>80</td>
      <td>S</td>
      <td>17</td>
      <td>25</td>
      <td>1007.56</td>
      <td>80</td>
      <td>50</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 22 columns</p>
</div>




```python
idx = df['NAME'].str.contains('Blackwell') | df['NAME'].str.contains('Lake')
df[idx]
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
      <th>11</th>
      <td>BLAC</td>
      <td>Blackwell</td>
      <td>OK</td>
      <td>36.75</td>
      <td>-97.25</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>38</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SSW</td>
      <td>15</td>
      <td>23</td>
      <td>1007.02</td>
      <td>80</td>
      <td>44</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>CARL</td>
      <td>Lake Carl Blackwell</td>
      <td>OK</td>
      <td>36.15</td>
      <td>-97.29</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>36</td>
      <td>NaN</td>
      <td>80</td>
      <td>S</td>
      <td>17</td>
      <td>25</td>
      <td>1007.56</td>
      <td>80</td>
      <td>50</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 22 columns</p>
</div>




```python
idx = df['STID'].isin(['ACME','ALTU'])
df[idx]
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
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ALTU</td>
      <td>Altus</td>
      <td>OK</td>
      <td>34.59</td>
      <td>-99.34</td>
      <td>2019</td>
      <td>4</td>
      <td>15</td>
      <td>15</td>
      <td>20</td>
      <td>...</td>
      <td>39</td>
      <td>NaN</td>
      <td>82</td>
      <td>SSW</td>
      <td>19</td>
      <td>26</td>
      <td>1007.86</td>
      <td>82</td>
      <td>45</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 22 columns</p>
</div>


