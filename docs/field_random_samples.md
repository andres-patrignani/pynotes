# Random Sampling Points

When collecting field observations we are often confronted with large regions, watershed, or agricultural fields and sampling strategies are required to ensure that we accurately assess the variable of study.

We will use a real watershed boundary from the Konza Prairie, which is located in the northern part of the Flint Hills region of Kansas. The Konza Prairie is primarily owned by The Nature Conservancy and operated as a field research station by Kansas State University. The Konza is a long-term ecological research field and is one the largest protected tallgrass prairie ecosystem in the world.

During the summer period it is common to see experienced and young researchers conducting field research using a variaety of sampling techniques. This exercise is aimed at researchers that need to come up with random points and grid cells to collect point and quadrat observations.


Learn more about the Konza Prairie:

- [Konza Prairie Kansas State University page](https://kpbs.konza.k-state.edu/)

- [Konza Prairie Nature Conservancy page](https://www.nature.org/en-us/get-involved/how-to-help/places-we-protect/konza-prairie/?gclid=EAIaIQobChMInoCtktfA6AIVRj0MCh2ltAm7EAAYASAAEgKT6PD_BwE&gclsrc=aw.ds)


```python
# Import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from shapely.geometry import Point, MultiPoint, Polygon, MultiPolygon, box

```


```python
# Load Konza Prairie watershed
df = pd.read_csv("../datasets/konza_prairie_K1B_watershed.csv")
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
      <th>lon</th>
      <th>lat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-96.560971</td>
      <td>39.083618</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-96.561303</td>
      <td>39.084147</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-96.561698</td>
      <td>39.084579</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-96.562693</td>
      <td>39.084823</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-96.562929</td>
      <td>39.085223</td>
    </tr>
  </tbody>
</table>
</div>



## Plot watershed boundary


```python
# Visualize watershed
plt.title('Konza Prairie - Watershed K1B')
plt.plot(df["lon"],df["lat"], '-k')
plt.axis('equal')
plt.show()

```


![png](field_random_samples_files/field_random_samples_4_0.png)


## Test if point is inside watershed 


```python
# Create a point using Shapely
p = Point(-96.560,39.092) # Intensionally inside the watershed

# Create a polygon of the watershed using Shapely
# We will make use of the itertuples method to conver a Pandas row into a tuple
coords = list(df.itertuples(index=False, name=None))
watershed = Polygon(coords)

# Check if the point is inside polygon
p.within(watershed)

```




    True




```python
# Confirm visually
lon_watershed,lat_watershed = watershed.boundary.xy
plt.title('Konza Prairie - Watershed K1B')
plt.plot(lon_watershed,lat_watershed, '-k')
plt.scatter(p.x,p.y, marker='x', color='r')
plt.axis('equal')
plt.show()

```


![png](field_random_samples_files/field_random_samples_7_0.png)


## Random sampling points

Let's generate a set of *N* random sampling points within the watershed.



```python
# Let's make use of the watershed bounds for our points
# bounding box is a (minx, miny, maxx, maxy) 
watershed.bounds

```




    (-96.57528919, 39.08361847, -96.55282197, 39.09784962)



In our next step we will try to learn how to generate the random coordinates and convert these coordinates into a Shapely MultiPoint object. We will re-write part of this code in a later section once we know how to do this. We also need to see whether creating the points this way would work. We will need to confirm this visually.



```python
# For reproducibility
np.random.seed(99)

# Generate N random points
N = 30
rand_lon = np.random.uniform(low=watershed.bounds[0],
                               high=watershed.bounds[2],
                              size=N)

rand_lat = np.random.uniform(low=watershed.bounds[1],
                               high=watershed.bounds[3],
                              size=N)

```


```python
# Create tuples with Lat and Lon for each point
rand_points = []
for n in range(len(rand_lon)):
    rand_points.append((rand_lon[n],rand_lat[n]))

# Convert points to Shapely Multipoint object
P = MultiPoint(rand_points)

```


```python
# Visualize random points
lon_watershed,lat_watershed = watershed.exterior.xy
plt.title('Konza Prairie - Watershed K1B')
plt.plot(lon_watershed,lat_watershed, '-k')

# Iterate over each point in MultiPoint object P
for point in P: 
    
    # If point is within watershed, then make it green, oterhwise red.
    if point.within(watershed):
        plt.scatter(point.x,point.y, marker='x', color='g')
    else:
        plt.scatter(point.x,point.y, marker='x', color='r')

plt.axis('equal')
plt.show()

```


![png](field_random_samples_files/field_random_samples_13_0.png)



```python
# Let's write some code to ensure we get N random points 
# within the watershed.

# For reproducibility
np.random.seed(99)

rand_points = []
while len(rand_points) < N:
    rand_lon = np.random.uniform(low=watershed.bounds[0], high=watershed.bounds[2])
    rand_lat = np.random.uniform(low=watershed.bounds[1], high=watershed.bounds[3])
    point = Point(rand_lon, rand_lat)
    if point.within(watershed):
        rand_points.append((rand_lon,rand_lat))
        
P = MultiPoint(rand_points)

```


```python
# Visualize random points
lon_watershed,lat_watershed = watershed.exterior.xy
plt.title('Konza Prairie - Watershed K1B')
plt.plot(lon_watershed,lat_watershed, '-k')
for point in P: 
    if point.within(watershed):
        plt.scatter(point.x,point.y, marker='x', color='g')
    else:
        plt.scatter(point.x,point.y, marker='x', color='r')
plt.axis('equal')
plt.show()

```


![png](field_random_samples_files/field_random_samples_15_0.png)


## Random sampling grid cells

Another option to random sampling points is to discretize the area of the water shed into *N* grid cells and then randomly select some of these cells to conduct the field sampling.

To solve this problem we will need to generate square grid cells and determined whether they are fully within the boundary of the watershed. This approach will make sure that no grid cell overlaps with the watershed boundary.

Another approach is to intersect the grid cell with the boundary, but this could led grid cells with smaller sampling areas.


```python
# Create a test grid cell using the box method
# box(minx, miny, maxx, maxy, ccw=True)
b = box(-96.565, 39.090, -96.560, 39.095)
b.exterior.xy

```




    (array('d', [-96.56, -96.56, -96.565, -96.565, -96.56]),
     array('d', [39.09, 39.095, 39.095, 39.09, 39.09]))




```python
# Extract coordinates from the box object
b_lon, b_lat = list(b.exterior.coords.xy)

```


```python
# Test whehter box is COMPLETELY inside the watershed
# For other options such as: intersect, overlaps, and touches see the docs
b.within(watershed)

```




    True




```python
# Visualize that the arbitrary square is indeed within the watershed
plt.title('Konza Prairie - Watershed K1B')
plt.plot(lon_watershed,lat_watershed, '-k')
plt.plot(b_lon, b_lat, '-r')
plt.axis('equal')
plt.show()

```


![png](field_random_samples_files/field_random_samples_20_0.png)


### Create grid

To create the grid cells within the watershed we have two options: 1) create a known number of cells that fit within the watershed, or 2) create as many grid cells as possible of a given size. Because of its irregular shape, we will create as many cells as possible of a given size. We will cover the entire bounding box of the water shed, and then eliminate those grdi cells that are not fully contained by the watershed boundaries.

Of course, the smaller the size of the grid cells, the more cells we can fit, and the closer they will follow the perimeter of the watershed.

An important observation is that grid cells will share their sides, they will be touching each other, but they will not be overlapping.


```python
# Longitude vector
x_vec = np.linspace(watershed.bounds[0], watershed.bounds[2], 30) 

# Latitude vector
y_vec = np.linspace(watershed.bounds[1], watershed.bounds[3], 30)

```

An alternative that deserves some exploration is using the Numpy `meshgrid()` function and the Shapely `MultiPolygon` feature. In this particualr case I found that a straigth forward `for` loop and the use of an array of Shapely `Polygons` was simpler, at least for this particular problem.

### Generate tuples for each grid cell


```python
grid = []
for i in range(len(x_vec)-1):
    for j in range(len(y_vec)-1):
        cell = box(x_vec[i], y_vec[j], x_vec[i+1], y_vec[j+1])
        grid.append(cell)
        
```

### Overlay grid on watershed map


```python
# Visualize grid
plt.figure(figsize=(8,8))
plt.title('Konza Prairie - Watershed K1B')
plt.plot(lon_watershed,lat_watershed, '-k')

for cell in grid:
    cell_lon = list(cell.exterior.coords.xy[0])
    cell_lat = list(cell.exterior.coords.xy[1])
    plt.plot(cell_lon,cell_lat, '-k', linewidth=0.5)
    
plt.axis('equal')
plt.show()

```


![png](field_random_samples_files/field_random_samples_27_0.png)


### Exclude grid cells that are outside or overlap watershed


```python
grid = []
for i in range(len(x_vec)-1):
    for j in range(len(y_vec)-1):
        cell = box(x_vec[i], y_vec[j], x_vec[i+1], y_vec[j+1])      
        if cell.within(watershed):
            grid.append(cell)


plt.figure(figsize=(8,8))
plt.title('Konza Prairie - Watershed K1B')
plt.plot(lon_watershed,lat_watershed, '-k')


for cell in grid:
    cell_lon, cell_lat = list(cell.exterior.coords.xy)
    plt.plot(cell_lon, cell_lat, '-k', linewidth=0.5)
    
plt.axis('equal')
plt.show()

```


![png](field_random_samples_files/field_random_samples_29_0.png)


### Select random numer of cell within watershed


```python
# For reproducibility
np.random.seed(99)

# Select random cells from the set within the watershed
sampling_cells = np.random.choice(grid, size=20, replace=False)

plt.figure(figsize=(12,12))
plt.title('Konza Prairie - Watershed K1B')
plt.plot(lon_watershed,lat_watershed, '-k')

for cell in grid:
    cell_lon, cell_lat = list(cell.exterior.coords.xy)
    plt.plot(cell_lon,cell_lat, '-k', linewidth=0.5)
    
for count,cell in enumerate(sampling_cells):
    cell_lon, cell_lat = list(cell.exterior.coords.xy)
    plt.plot(cell_lon,cell_lat, '-r',linewidth=2)
    
    # Add count + 1 to start numbering from one. Add a little offset to improve visualization on map.
    plt.annotate(str(count+1), xy=(cell_lon[3]+0.0001, cell_lat[3]+0.0001))
    
plt.axis('equal')
plt.show()

```


![png](field_random_samples_files/field_random_samples_31_0.png)


### Print centroid for each sampling cell

Thinking ahead on field work, it would be nice to have a the coordiantes for each cell. In this case we will print the Lower-Left corner of each cell. An alternative is to compute the cell centroid. You can easily do this using numpy. For instance:

```python

x_centroid = np.mean(cell_lon)
y_centroid = np.mean(cell_lat)

```


```python
print("Coordinates for the lower-left corner of the each cell")
for count,cell in enumerate(sampling_cells):
    cell_lon, cell_lat = list(cell.exterior.coords.xy)
    print("Cell",count+1,"Lat:", cell_lat[3],"Lon:",cell_lon[3])
```

    Coordinates for the lower-left corner of the each cell
    Cell 1 Lat: 39.08509065793103 Lon: -96.55747036034482
    Cell 2 Lat: 39.08656284586207 Lon: -96.5628934824138
    Cell 3 Lat: 39.09196086827586 Lon: -96.55592089689655
    Cell 4 Lat: 39.09196086827586 Lon: -96.55514616517242
    Cell 5 Lat: 39.08999795103448 Lon: -96.56599240931035
    Cell 6 Lat: 39.092451597586205 Lon: -96.55592089689655
    Cell 7 Lat: 39.09147013896551 Lon: -96.55437143344827
    Cell 8 Lat: 39.09196086827586 Lon: -96.5628934824138
    Cell 9 Lat: 39.0880350337931 Lon: -96.55669562862069
    Cell 10 Lat: 39.09294232689655 Lon: -96.56754187275862
    Cell 11 Lat: 39.087544304482755 Lon: -96.55824509206896
    Cell 12 Lat: 39.08999795103448 Lon: -96.56211875068965
    Cell 13 Lat: 39.08656284586207 Lon: -96.55747036034482
    Cell 14 Lat: 39.09147013896551 Lon: -96.56211875068965
    Cell 15 Lat: 39.09196086827586 Lon: -96.55979455551724
    Cell 16 Lat: 39.09294232689655 Lon: -96.56366821413793
    Cell 17 Lat: 39.09294232689655 Lon: -96.56986606793104
    Cell 18 Lat: 39.08705357517241 Lon: -96.56134401896551
    Cell 19 Lat: 39.0934330562069 Lon: -96.57373972655174
    Cell 20 Lat: 39.09392378551724 Lon: -96.56056928724138


## References

[Shapely official documentation]( https://shapely.readthedocs.io/en/latest/manual.html#geometric-objects)
