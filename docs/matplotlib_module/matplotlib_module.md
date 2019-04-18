
# **Matplotlib Module**

## Introduction

[Matplotlib](https://matplotlib.org/users/index.html) is one of the most popular plotting libraries in Python. The [pyplot](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.html) sub-module is a high-level library of plotting commands very similar to Matlab syntax. Overt the years the community of Python users have developed a substantial amount of plots and an extensive documentation for Matplotlib. While more interactive libraries such as Bokeh and Plotly are strongly emerging in the plotting world, Matplotlib is still the most popular in the scientific enviroment and one of the easiest to get started.

It is common to tuse Matplotlib in combination with Numpy and Pandas modules. So, in this example we will use all three of them to generate some basic plots.


```python
# Line required when plotting in Jupyter Lab
%matplotlib inline

import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
```

## Load data

For this example we will use long-term data of atmospheric carbon dioxide concentration in Mauna Load, Hawaii. The monthly data timeseries spans from March, 1958 to November of 2018. Data is reported monthly.

Source: https://www.esrl.noaa.gov/gmd/ccgg/trends/full.html


```python
# Define file path
dirname = '/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/'
filename = 'mauna_loa_co2.csv'

# Load data
df = pd.read_csv(dirname + filename)

# Print top entries in DataFrame
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
      <th>year</th>
      <th>month</th>
      <th>co2_ppm</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1958</td>
      <td>3</td>
      <td>315.71</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1958</td>
      <td>4</td>
      <td>317.45</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1958</td>
      <td>5</td>
      <td>317.50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1958</td>
      <td>6</td>
      <td>317.10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1958</td>
      <td>7</td>
      <td>315.86</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate a decimal year so that we can plot the monthly data
df['decimal_date'] = df.year + df.month/12

# Check that our computation
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
      <th>year</th>
      <th>month</th>
      <th>co2_ppm</th>
      <th>decimal_date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1958</td>
      <td>3</td>
      <td>315.71</td>
      <td>1958.250000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1958</td>
      <td>4</td>
      <td>317.45</td>
      <td>1958.333333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1958</td>
      <td>5</td>
      <td>317.50</td>
      <td>1958.416667</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1958</td>
      <td>6</td>
      <td>317.10</td>
      <td>1958.500000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1958</td>
      <td>7</td>
      <td>315.86</td>
      <td>1958.583333</td>
    </tr>
  </tbody>
</table>
</div>



<a name="matplotlib_parameters"></a>
## Parameters

Before we create a plot I want to show how to access all the parameters in a plot, so that can search and learn about all the possibilities.


```python
print(plt.rcParams.items)
```

Sometimes after changing many parameters we may want to go back to the defaults. When working in the Jupyter Lab and Jupyter Notebook we need to be aware that most times we use the `inline` backend, which ensures that Matplotlig figures render in Jupyter Lab and Jupyter Notebooks. The `inline` has its own default parameters, so in order to reset figure preoperties we need to run the following sequence of commands:

```python

%matplotlib inline
import matplotlib as mpl
import matplotlib.pyplot as plt

inline_rc = dict(mpl.rcParams)
mpl.rcParams.update(inline_rc)
```

>If the following sets of commands don't work, then you can always restart the Python kernel and load the module again. Unless you have lots of as a result of lengthy computations sometimes the oldest trick in the book is the fastest option.

<a name="matplotlib_line"></a>
## Line plot

A more generic method of creating line and scatter plots. You can use Matlab-like syntax.


```python
# Barebones of line plot
plt.plot(df.decimal_date, df.co2_ppm, '-r')
plt.show()
```


![png](output_9_0.png)


This is the simplest line plot we can generate. It's still missing axis labels and probably we need to plot a shorter period since we can hardly see any data. In the following steps we will write few more commands to improve the figure, but before that let's inspect the components of the first line:

`plt.plot()`: Most basic plotting function. It generates a line plot

`decimal_date`: This the `x` data

`df.co2_ppm`: This is the `y` data

`'-r'`: The `-` indicates a solid line and `r` indicates that the color is red.

Other possible colors are: 

`k`: black,
`b`: blue,
`g`: green,
`y`: yellow

Other possible line styles are:
`--`: Dashed line,
`-.`: Dashed dot line,
`:`:  Dotted line

Now it's time to tweak few more parameters in addition to line style and line color to improve our figure. From now on I will only plot the first 50 values of the dataset, so that we can better see the details of the timeseries.


```python
# Line plot
plt.figure(figsize=(12,8)) # sets figure size in inches
plt.plot(df.decimal_date[0:50], df.co2_ppm[0:50],'-oy')
plt.ylabel('Carbon Dioxide Concentration (ppm)', fontsize=16)

# Parameters
plt.rcParams['axes.grid'] = True;
plt.rcParams['grid.color'] = 'k'
plt.rcParams['ytick.labelsize'] = 14.0
plt.rcParams['xtick.labelsize'] = 14.0
plt.show()

```


![png](output_11_0.png)


<a name="matplotlib_scatter"></a>
## Scatter plot



```python
# Scatter plot
plt.figure(figsize=(12,8)) 
plt.scatter(df.decimal_date[0:50], 
            df.co2_ppm[0:50], 
            s=55, 
            marker='s', 
            facecolors='g', 
            edgecolors='g')
plt.ylabel('Carbon Dioxide Concentration (ppm)', fontsize=16)
plt.show()

```


![png](output_13_0.png)


<a name="matplotlib_line_scatter"></a>
## Combined scatter and line plot

In Matplotlib you can easily overlay different plots. In this case we will overlay a line and scatter plots containing the same data, but the same applies if the dataset in the line plot is different than that of the scatter plot. The only detail to remember if we want the plots to overlap is that data must have a similar range.


```python
# Line and scatter plot

# Figure size
plt.figure(figsize=(12,8))

# Add line plot
plt.plot(df.decimal_date[0:50], 
         df.co2_ppm[0:50], 
         '-r')

# Add scatter plot
plt.scatter(df.decimal_date[0:50], 
            df.co2_ppm[0:50], 
            s=55, 
            marker='s', 
            facecolors='g', 
            edgecolors='g')

# Calculate average of first 50 values
avg_co2 = df.co2_ppm[0:50].mean()

# Add horizontal line representing mean value
plt.axhline(y=avg_co2, 
            linewidth=1, 
            color='k', 
            linestyle='--')
#plt.plot(df.decimal_date[0:50],np.ones(df.co2_ppm[0:50].size)*avg_co2 ,'--k')

# Add ylabel
plt.ylabel('$CO_2$ (ppm)', fontsize=16)

# Add legend
plt.legend(['Carbon dioxide line','Average','Carbon dioxide marker'])

# Add annotation
annotation_label = 'Mean: ' + str(round(df.co2_ppm[0:50].mean(),1)) + ' ppm'
plt.annotate(annotation_label, xy=(65, 380), xycoords='figure points', fontsize=16)

# Remove extra marging
plt.autoscale(enable=True, axis='x', tight=True)

plt.rcParams['axes.grid'] = False;
plt.rcParams['ytick.labelsize'] = 14.0
plt.rcParams['xtick.labelsize'] = 14.0
plt.rcParams['axes.facecolor'] = 'w'
plt.rcParams['axes.spines.top'] = True
plt.show()
```


![png](output_15_0.png)


<a name="matplotlib_subplots"></a>
## Subplots

A way of condensing figures using different subplot layouts


```python
# Set file path
dirname = '/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/'
filename = 'global_wheat.csv'

```


```python
# Load wheat data
df = pd.read_csv(dirname + filename)
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
      <th>year</th>
      <th>Argentina_area_has</th>
      <th>Argentina_production_ton</th>
      <th>Argentina_yield_hg_ha</th>
      <th>Australia_area_has</th>
      <th>Australia_production_ton</th>
      <th>Australia_yield_hg_ha</th>
      <th>Canada_area_has</th>
      <th>Canada_production_ton</th>
      <th>Canada_yield_hg_ha</th>
      <th>...</th>
      <th>Germany_yield_hg_ha</th>
      <th>Mexico_area_has</th>
      <th>Mexico_production_ton</th>
      <th>Mexico_yield_hg_ha</th>
      <th>UK_area_has</th>
      <th>UK_production_ton</th>
      <th>UK_yield_hg_ha</th>
      <th>US_area_has</th>
      <th>US_production_ton</th>
      <th>US_yield_hg_ha</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1961</td>
      <td>4420900</td>
      <td>5725000</td>
      <td>12950</td>
      <td>5958110</td>
      <td>6727190</td>
      <td>11291</td>
      <td>10245000</td>
      <td>7713000</td>
      <td>7529</td>
      <td>...</td>
      <td>28607</td>
      <td>836538</td>
      <td>1401910</td>
      <td>16758</td>
      <td>739000</td>
      <td>2614000</td>
      <td>35372</td>
      <td>20870000</td>
      <td>33539000</td>
      <td>16070</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1962</td>
      <td>3744700</td>
      <td>5700000</td>
      <td>15222</td>
      <td>6664680</td>
      <td>8352910</td>
      <td>12533</td>
      <td>10853000</td>
      <td>15393000</td>
      <td>14183</td>
      <td>...</td>
      <td>33898</td>
      <td>747728</td>
      <td>1455260</td>
      <td>19462</td>
      <td>913000</td>
      <td>3974000</td>
      <td>43527</td>
      <td>17680000</td>
      <td>29718000</td>
      <td>16809</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1963</td>
      <td>5676000</td>
      <td>8940000</td>
      <td>15751</td>
      <td>6666680</td>
      <td>8924460</td>
      <td>13387</td>
      <td>11156000</td>
      <td>19691000</td>
      <td>17651</td>
      <td>...</td>
      <td>33934</td>
      <td>819210</td>
      <td>1702990</td>
      <td>20788</td>
      <td>780000</td>
      <td>3046000</td>
      <td>39051</td>
      <td>18415000</td>
      <td>31211900</td>
      <td>16949</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1964</td>
      <td>6135400</td>
      <td>11260000</td>
      <td>18353</td>
      <td>7251500</td>
      <td>10037000</td>
      <td>13841</td>
      <td>12018000</td>
      <td>16349000</td>
      <td>13604</td>
      <td>...</td>
      <td>34845</td>
      <td>818325</td>
      <td>2203070</td>
      <td>26922</td>
      <td>893000</td>
      <td>3793000</td>
      <td>42475</td>
      <td>20138000</td>
      <td>34928000</td>
      <td>17344</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1965</td>
      <td>4601200</td>
      <td>6079000</td>
      <td>13212</td>
      <td>7087980</td>
      <td>7067060</td>
      <td>9970</td>
      <td>11453000</td>
      <td>17674000</td>
      <td>15432</td>
      <td>...</td>
      <td>32321</td>
      <td>858259</td>
      <td>2150350</td>
      <td>25055</td>
      <td>1026000</td>
      <td>4171000</td>
      <td>40653</td>
      <td>20056000</td>
      <td>35805000</td>
      <td>17853</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
# Subplots

plt.figure()

plt.subplot(2,1,1)
plt.plot(df.year,df.Argentina_yield_hg_ha)
plt.ylabel('Yield kg/ha')

plt.subplot(2,1,2)
plt.plot(df.year,df.UK_yield_hg_ha)
plt.ylabel('Yield kg/ha')

plt.show()
# The subplot() command specifies numrows, numcols, plot_number
```


![png](output_19_0.png)


<a name="matplotlib_secondary_yaxis"></a>
## Secondary Y axis plots


```python
# Creating plot with secondary Y axis

plt.figure()

ax1 = plt.subplot()
ax1.plot(df.year,df.Argentina_yield_hg_ha,'-g')

ax2 = ax1.twinx()
ax2.plot(df.year,df.UK_yield_hg_ha,'--b')

ax1.set_xlabel('Years')
ax1.set_ylabel('Yield hg $\mathregular{ha^{-1}}$', color='g')
ax2.set_ylabel('Cultivated area (1000 has)', color='b')

plt.show()
```


![png](output_21_0.png)


<a name="matplotlib_styles"></a>
## Styles

In addition to the default style, Matplotlib has a variery of pre-defined styles. TO see some examples visit the following websites:

Gallery 1 at: https://matplotlib.org/gallery/style_sheets/style_sheets_reference.html

Gallery 2 at: https://tonysyu.github.io/raw_content/matplotlib-style-gallery/gallery.html

To see the available styles just type:


```python
print(plt.style.available)
```


```python
# Change plot defaults to ggplot style (similar to ggplot R language library)

plt.style.use('ggplot') # Use plt.style.use('default') to revert.
plt.plot(df.year,df.Argentina_yield_hg_ha,'-g')
plt.show()
```


![png](output_24_0.png)



```python
# Plot using the default style

plt.style.use('default') # Use plt.style.use('default') to revert.
plt.plot(df.year,df.Argentina_yield_hg_ha,'-g')
plt.show()
```


![png](output_25_0.png)


<a name="matplotlib_savefig"></a>
## Saving a figure

savefig(fname, dpi=None, facecolor='w', edgecolor='w',
        orientation='portrait', papertype=None, format=None,
        transparent=False, bbox_inches=None, pad_inches=0.1,
        frameon=None, metadata=None)
        
Source: https://matplotlib.org/api/_as_gen/matplotlib.pyplot.savefig.html


```python
# Example plot
plt.plot(df.year,df.Argentina_yield_hg_ha,'-g')

# Save command specifying the number of dots per inch
plt.savefig('examplefig.png', dpi=200) 
```
