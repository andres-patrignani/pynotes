# Objects and classes

Everything in Python is an object. The following code snippets are aimed at showing some of the features of objects and its properties and methods. At the end of these lines we learn how to create Python classe, which allow us to create a new type of object.

I want to highlight that the difference between `function` and `method` is merely semantic. Functions are regular functions that can be outside a class (anywhere in your code) or as part of a class. In the latter situation, the function is often called `method`. A method is a function that is only available for a specifc object type. 

## List objects
For instance, if we have the follwoing list `a = [2,1,3]` we can check its `type` and built-in `methods`.

> To be clear, the variable `a` is an object that belongs to the class `list`


```python
# Generate a list
a = [2,1,3]

# Print its type
print(type(a))

# Prove that sort() is a method of lists
print(a.sort) # not using the ()

# The following address 0x11b1f5508 is a pointer in memory
# You will likely get a different pointer.

# Let's use the built-in method
a.sort()  # to invoke 

# and print the sorted list
print(a)

```

    <class 'list'>
    <built-in method sort of list object at 0x11b1f6bc8>
    [1, 2, 3]


## String objects

Let's try a different example this time using a string `b = "soil` and the method `.upper()` to convert the entire string into upper case letters.

Note that if you do this `b.s` and press on the `TAB` key you will not find a method called `.sort()` because that method is only available to `lists`. Now it could make sense to have such a method, let's say to sort letters from A to Z, but it was not developed into the class and as a consequence it is not available (or it might be available under a different method name).



```python
# Generate a string
b = "soil"

# Print its type
print(type(b))

# Prove that upper() is a method of lists
print(b.upper) # not using the ()

# Let's use the built-in method and print the upper case word
print(b.upper())

```

    <class 'str'>
    <built-in method upper of str object at 0x10d66ed50>
    SOIL


## Numpy objects

One last example using the popular Numpy module. I want to show you that some methods can be extremely useful to make basic computations like calculating the mean value of an array.



```python
# Import numpy module
import numpy as np

# Generate array of 3 rows and 4 columns (a 3 by 4 matrix)
array = np.array(np.random.uniform(12, size=(3,4)))

# Print array
print(array)

# Print object type
print(type(array))

# Prove that .mean() is a method of numpy objects
print(array.mean)

# Use the numpy method .mean() for axis 1 (along columns)
# Output is 1 by 4 (average for each column along rows)
print(array.mean(0))

# Use the numpy method .mean() for axis 1
# Output is 1 by 3 (average for each row along columns)
print(array.mean(1))

```

    [[ 5.82762107  6.30116202  6.49394418 10.95759274]
     [ 8.55305002  5.21993403  8.95412887  7.67664029]
     [ 2.02234023 10.01881041  4.58524054  2.74771334]]
    <class 'numpy.ndarray'>
    <built-in method mean of numpy.ndarray object at 0x11b22d2b0>
    [5.46767044 7.17996882 6.6777712  7.12731545]
    [7.39508    7.6009383  4.84352613]


## Object properties


```python
# These are properties of the object

# Type of elements in the array (float 64 bits in this case)
print(type(array.dtype))
print(array.dtype)
      
# Dimensions of the array
print(type(array.shape))
print(array.shape) # returns a tuple

# Total number of elements
print(type(array.size))
print(array.size)

```

    <class 'numpy.dtype'>
    float64
    <class 'tuple'>
    (3, 4)
    <class 'int'>
    12


## Custom classes and objects

Now that we looked at multiple examples I propose we create our own class with our own properties and methods.

Let 's set a class for a specific soil series. The underlying assumption is that soils that belong to the same soil series have similar characteristics. Thus, when we initiate our class, we can readily populate many soil properties based on commonly known features. If a specific soil that belongs to the series has a slightly different value for a property then we can change it.

#### Crete soil series
TAXONOMIC CLASS: Fine, smectitic, mesic Pachic Udertic Argiustolls

TYPICAL PEDON: Crete silt loam on a convex, 0.5 percent slope in crop cover at an elevation of 440 meters (1445 feet). (Colors are for dry soil unless otherwise stated.)

Ap--0 to 15 centimeters; very dark gray (10YR 3/1) silt loam

A--15 to 36 centimeters; very dark gray (10YR 3/1) silty clay loam

BA--36 to 48 centimeters; dark gray (10YR 4/1) silty clay loam

Bt1--48 to 71 centimeters; brown (10YR 4/3) silty clay

Bt2--71 to 89 centimeters; brown (10YR 5/3) silty clay

BC--89 to 107 centimeters; light olive brown (2.5Y 5/4) silty clay

C--107 to 152 centimeters; light yellowish brown (2.5Y 6/4) silty clay loam

TYPE LOCATION: Saline County, Nebraska; latitude 40 degrees 39 minutes 47.8 seconds N and longitude 97 degrees 0 minutes 33.8 seconds W., NAD83.

GEOGRAPHIC SETTING:
Parent material: loess
Landform: interfluves and hillslopes on uplands and stream terraces on river valleys
Slopes: 0 to 11 percent
Elevation: 400 to 600 meters (1310 to 1970 feet)
Mean annual temperature: 10 to 13 degrees C (50 to 55 degrees F)
Mean annual precipitation: 56 to 86 centimeters (22 to 34 inches)
Frost-free period: 149 to 196 days



```python
class creteseries():
    
    # class variable shared by all instances
    Ap = {'top_depth': 0, 
          'bottom_depth': 15, 
          'munsell_color': '10YR 3/1', 
          'textural_class': "silt loam"}
    
    # class variable shared by all instances
    A = {'top_depth': 15, 
          'bottom_depth': 36, 
          'munsell_color': '10YR 3/1', 
          'textural_class': "silt clay loam"}
    
    # instance variable unique to each instance
    def add_location(self,city,lat,lon):
        self.city = city
        self.lat = lat
        self.lon = lon
    
```


```python
soil = creteseries()
print(soil.Ap)
print(soil.Ap['munsell_color'])
print(type(soil))

mysoil.add_location('Manhattan',38.5,-97.5)
mysoil.city

```

    {'top_depth': 0, 'bottom_depth': 15, 'munsell_color': '10YR 3/1', 'textural_class': 'silt loam'}
    10YR 3/1
    <class '__main__.creteseries'>





    'Manhattan'




```python
class creteseries():
        
    # class variable shared by all instances
    Ap = {'top_depth': 0, 
          'bottom_depth': 15, 
          'munsell_color': '10YR 3/1', 
          'textural_class': "silt loam"}
    
    A = {'top_depth': 15, 
          'bottom_depth': 36, 
          'munsell_color': '10YR 3/1', 
          'textural_class': "silt clay loam"}
    
    # instance variables unique to each instance
    def __init__(self,city,lat,lon):
        self.city = city
        self.lat = lat
        self.lon = lon
        
    def add_landcover(self,landcover):
        self.landcover = landcover
        
    def add_tillage(self,tillage):
        self.tillage = tillage
        
```


```python
soil = creteseries('konza prairie',39.094980, -96.575208)
soil.add_landcover = 'natural grassland'
soil.add_tillage = 'none'

import pprint as pprint
pprint.pprint(vars(soil))

```

    {'add_landcover': 'natural grassland',
     'add_tillage': 'none',
     'city': 'konza prairie',
     'lat': 39.09498,
     'lon': -96.575208}


## Create your own class

The Harney soil series is the official state soil for Kansas. Harney soils are deep, well-drained Mollisols formed in loess and that typically developed under prairie vegetation in the area covered by Nebraska and Kansas. Harney soils belong to the family of Fine, smectitic, mesic Typic Argiustolls.

**TAXONOMIC CLASS**: Fine, smectitic, mesic Typic Argiustolls

**TYPICAL PEDON**: Harney silt loam-in a nearly level cultivated field. (Colors are for dry soil unless otherwise stated.)

**Ap** 0 to 9 inches; dark grayish brown (10YR 4/2) silt loam, very dark grayish brown (10YR 3/2) moist; moderate medium granular structure; slightly hard, very friable; many fine roots; slightly acid; clear smooth boundary. (4 to 14 inches thick)

**AB** 9 to 12 inches; dark grayish brown (10YR 4/2) silt loam, very dark grayish brown (10YR 3/2) moist; moderate fine subangular blocky structure; hard, friable; many fine roots; neutral; clear smooth boundary. (0 to 10 inches thick)

**Bt1** 12 to 18 inches; grayish brown (10YR 5/2) silty clay loam, dark grayish brown (10YR 4/2) moist; moderate medium subangular blocky structure; very hard, very firm; few fine roots; moderately alkaline; clear smooth boundary.

**Bt2** 18 to 28 inches; grayish brown (10YR 5/2) silty clay loam, dark grayish brown (10YR 4/2) moist; strong medium subangular blocky structure; very hard, very firm; few fine roots; moderately alkaline; gradual smooth boundary. (Combined thickness of the Bt horizon is 10 to 26 inches)

**BCk** 28 to 35 inches; brown (10YR 5/3) silty clay loam, brown (10YR 4/3) moist; moderate medium subangular blocky structure; hard, firm; few fine roots; many soft accumulations of carbonates; strong effervescence; moderately alkaline; gradual smooth boundary. (0 to 16 inches thick)

**Ck** 35 to 47 inches; pale brown (10YR 6/3) silt loam, brown (10YR 5/3) moist; massive; slightly hard, friable; common soft accumulations of carbonates; strong effervescence; moderately alkaline; gradual smooth boundary. (0 to 20 inches thick)

**C** 47 to 60 inches; pale brown (10YR 6/3) silt loam, brown (10YR 5/3) moist; massive; slightly hard, friable; strong effervescence; moderately alkaline.

**TYPE LOCATION**: Pawnee County, Kansas; about 1 1/2 miles north of Garfield; 1,840 feet east and 100 feet south of the northwest corner of sec. 36, T. 22 S., R. 18 W.

**Source**: United States Department of Agriculture (USDA) Natural Resources COnservation Service (NRCS) soil series description for [Harney series](https://soilseries.sc.egov.usda.gov/OSD_Docs/H/HARNEY.html)

## References

Python official documentation about objects and classes: https://docs.python.org/3/tutorial/classes.html

Soil Survey Staff, Natural Resources Conservation Service, United States Department of Agriculture. Web Soil Survey. Available online. Accessed [20-january-2020].



```python

```
