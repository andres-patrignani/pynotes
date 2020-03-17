# Geospatial data

- Install new module
- Import raster layer
- Import vector layer
- Plot maps
- Get location-specific values

[Source](https://rasterio.readthedocs.io/en/stable/intro.html)


```python
!pip install rasterio
```

    Collecting rasterio
    [?25l  Downloading https://files.pythonhosted.org/packages/92/7a/244ea4a29ac5e66dd762beb5e5553dcc1de21fe8a7d7d378b8d0f12fef5b/rasterio-1.0.22-cp37-cp37m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (26.6MB)
    [K    100% |████████████████████████████████| 26.6MB 677kB/s 
    [?25hCollecting snuggs>=1.4.1 (from rasterio)
      Downloading https://files.pythonhosted.org/packages/a5/8a/abb0803f05feb4401ab9e4c776ce11f0048b6cb0f2d84ace97cd93d7da04/snuggs-1.4.3-py3-none-any.whl
    Requirement already satisfied: attrs in /Users/andrespatrignani/anaconda3/lib/python3.7/site-packages (from rasterio) (18.2.0)
    Requirement already satisfied: numpy in /Users/andrespatrignani/anaconda3/lib/python3.7/site-packages (from rasterio) (1.15.1)
    Collecting click-plugins (from rasterio)
      Downloading https://files.pythonhosted.org/packages/e9/da/824b92d9942f4e472702488857914bdd50f73021efea15b4cad9aca8ecef/click_plugins-1.1.1-py2.py3-none-any.whl
    Collecting cligj>=0.5 (from rasterio)
      Downloading https://files.pythonhosted.org/packages/e4/be/30a58b4b0733850280d01f8bd132591b4668ed5c7046761098d665ac2174/cligj-0.5.0-py3-none-any.whl
    Collecting affine (from rasterio)
      Downloading https://files.pythonhosted.org/packages/56/5d/6877929932d17850fa4903d0db8233ec8ed35aab7ceae96fa44ea6d479bd/affine-2.2.2-py2.py3-none-any.whl
    Requirement already satisfied: click<8,>=4.0 in /Users/andrespatrignani/anaconda3/lib/python3.7/site-packages (from rasterio) (6.7)
    Requirement already satisfied: pyparsing in /Users/andrespatrignani/anaconda3/lib/python3.7/site-packages (from snuggs>=1.4.1->rasterio) (2.2.0)
    Installing collected packages: snuggs, click-plugins, cligj, affine, rasterio
    Successfully installed affine-2.2.2 click-plugins-1.1.1 cligj-0.5.0 rasterio-1.0.22 snuggs-1.4.3
    [33mYou are using pip version 10.0.1, however version 19.0.3 is available.
    You should consider upgrading via the 'pip install --upgrade pip' command.[0m



```python
import rasterio
import numpy as np

```


```python
dataset = rasterio.open('../datasets/paw_20190501_1km_v1.tif')
```

A coordinate reference system (CRS) is a coordinate-based local, regional or global system used to locate geographical entities. Spatial reference systems can be referred including EPSG codes defined by the International Association of Oil and Gas Producers.

[https://epsg.io/4326](https://epsg.io/4326)

EPSG:4326 is equal to WGS84 - World Geodetic System 1984


```python
print(dataset.crs)
```

    EPSG:4326



```python
print(dataset.dtypes)
```

    ('uint8',)



```python
print(dataset.width)
print(dataset.height)
print(dataset.indexes)
```

    897
    362
    (1,)



```python
print(dataset.bounds)
```

    BoundingBox(left=-102.058329251, bottom=36.991665186999995, right=-94.58332955, top=40.008331733)



```python
print(dataset.lnglat())
```

    (-98.3208294005, 38.49999846)



```python
dataset.xy(0,0,offset='ul') # center by default. ul, ur, ll, lr.
```




    (-102.058329251, 40.008331733)




```python
dataset_masked = np.ma.masked_equal(dataset.read(),255).astype('uint8')
print(dataset_masked.dtype)
print(dataset_masked.shape)
```

    uint8
    (1, 362, 897)



```python
plt.figure(figsize=(12,8))
plt.imshow(dataset_masked[0,:,:], cmap='RdYlBu')
cb = plt.colorbar(ticks=range(0,260,20), 
                  label='Rootzone soil moisture (mm)',
                  orientation="horizontal",
                  pad=0.1,
                  aspect=50)

plt.clim(-0.5, 100)
plt.axis('off')
plt.title('PAW 1-May-2019 in top 50 cm')
plt.show()

```


![png](soil_moisture_map_files/soil_moisture_map_12_0.png)

