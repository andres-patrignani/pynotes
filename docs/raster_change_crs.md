# Changing Coordinate Reference System

Converting from EPSG:9001 to EPSG:4326, which represents spatial coordinates in terms of latitude and longitude based on the World Geodetic System 1984 (WGS84).


```python
# Import modules
import rasterio
from rasterio.warp import calculate_default_transform, reproject, Resampling

```


```python
# Load raster map
raster_path = 'nws_precip_1day_20200426_geotiff/nws_precip_1day_20200426_conus.tif'
raster = rasterio.open(raster_path)

```


```python
# Check main attributes (e.g. shape, CRS, bounds, etc.)
raster.meta

```




    {'driver': 'GTiff',
     'dtype': 'float32',
     'nodata': -3.4028234663852886e+38,
     'width': 1121,
     'height': 881,
     'count': 4,
     'crs': CRS.from_wkt('PROJCS["NOAA_HRAP_Grid",GEOGCS["GCS_NOAA_HRAP",DATUM["D_NOAA_HRAP",SPHEROID["Sphere",6371200,0]],PRIMEM["Greenwich",0],UNIT["degree",0.0174532925199433]],PROJECTION["Polar_Stereographic"],PARAMETER["latitude_of_origin",60],PARAMETER["central_meridian",-105],PARAMETER["scale_factor",1],PARAMETER["false_easting",0],PARAMETER["false_northing",0],UNIT["metre",1,AUTHORITY["EPSG","9001"]]]'),
     'transform': Affine(4763.0, 0.0, -1904912.11073866,
            0.0, -4763.0, -3423783.69569394)}




```python
# Set properties to be used in the new reprojected raster map
dst_crs = 'EPSG:4326' # As an alternative the EPSG:3857 represents the web meractor 
transform, width, height = calculate_default_transform(raster.crs,dst_crs,raster.width,raster.height,*raster.bounds)
kwargs = raster.meta.copy()
kwargs.update({'crs':dst_crs, 'transform':transform, 'width':width, 'height':height})

```


```python
# Iterate over each band and reproject
# This step will save a new reprojected .tiff file
with rasterio.open("nws_geocoords.tiff", 'w', **kwargs) as dst:
    for i in range(1, raster.count + 1):
        reproject(
            source = rasterio.band(raster, i),
            destination = rasterio.band(dst, i),
            src_transform = raster.transform,
            src_crs = raster.crs,
            dst_transform = transform,
            dst_crs = dst_crs,
            resampling = Resampling.nearest)
        
```


```python
# Load newly created and reprojected .tiff file and display the new metadata
# to check that we did the right CRS conversion
raster_reprojected = rasterio.open("nws_geocoords.tiff")
print(raster_reprojected.meta)

```

    {'driver': 'GTiff', 'dtype': 'float32', 'nodata': -3.4028234663852886e+38, 'width': 1675, 'height': 860, 'count': 4, 'crs': CRS.from_epsg(4326), 'transform': Affine(0.044273720114906476, 0.0, -134.09052600184998,
           0.0, -0.044273720114906476, 57.86725177915063)}

