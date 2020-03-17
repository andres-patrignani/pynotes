# Data Structures

Information lives in data structures. You can view thse data structures as containers in which we store information for later use or processing. The selection of the right data structure depends on the structure, format, and level of complexity of the information that we want to store. Many times the same information can be stored in more than one data structure. The choice of the right data structure will depend on the application and, to a great extent, on your experience and coding preferences.


## 1. Lists

Lists are great for storing short or long sequences of elements. One of the most distinct features is that we access information stored in lists by using either a positional index (retrieve single item) or slicing (retrieve one or more items). Lists are the typical arrays in other languages. I think it is worth mentioning that the standard Python lists cannot handle element-wise operations. For this we need use Numpy arrays, which we will cover later.

List can have mixed data types and are `mutable`, which means that we can overwrite the information. This `mutable` capability can be good and bad. If we want to prevent accidentally changing the information of the list, then it could lead to some serious issues. On the other hand, if we want to re-define or update a variable then this feature is convenient.


```python
# Simple list
list_primes = [2, 3, 5, 7, 11, 13, 17]  # A row vector
print(list_primes)
type(list_primes)
```

    [2, 3, 5, 7, 11, 13, 17]





    list




```python
# Re-define list (they are mutable)
list_primes = [31, 37, 41, 43]
print(list_primes)
```

    [31, 37, 41, 43]



```python
# Determine the number of elements in a list
len(list_primes)
```




    4




```python
mixed_list = [1, 2, 3, 4, 'five', 'six']
print(mixed_list)
```

    [1, 2, 3, 4, 'five', 'six']



```python
# Lists can be nested
nested_list = [1980, 1990, 2000, 2010,['sand','silt','clay']]

print(nested_list[0])         # Access first element of list in the first position
print(nested_list[4])         # Access list in second position
print(nested_list[4][1])      # Access 'silt'
print(nested_list[4][2][0])   # Access the 'c' in 'clay'
```

    1980
    ['sand', 'silt', 'clay']
    silt
    c



```python
# Extract and save a nested list in a different variable
particles = nested_list[4]
print(particles)    # Copy information and store in a variable with a different name
print(nested_list)  # This list remains untouched
```

    ['sand', 'silt', 'clay']
    [1980, 1990, 2000, 2010, ['sand', 'silt', 'clay']]



```python
# Count method
nested_list.count(2000) # Count specific element. Takes only one argument

```




    1




```python
# Index of specific entry
nested_list.index(2010) 
```




    3



**Will the following command work?**

```python
nested_list.index('sand')
```

**Solution**
```python
nested_list[4].index('sand')
```



```python
nested_list.append([2020,2030])
print(nested_list)
```

    [1980, 1990, 2000, 2010, ['sand', 'silt', 'clay'], [2020, 2030]]



```python
# Clear list and check that list was cleared (it should print empty brackets)
nested_list.clear()
print(nested_list)
```

    []



```python
# Here is a more challenging example:
new_list = [ [1,2,3] , [4,5,6, ['a','b','c']] ]
new_list[1][3][0]
```




    'a'



**What letter will the following Python command return?**
```python
new_list[1][3][0]
```

**Solution**
```python
'a'
```

### Creating a 2D matrix



```python
M = [[1, 4, 5],
    [-5, 8, 9]]
print(M)
```

    [[1, 4, 5], [-5, 8, 9]]


### Append single element to a list


```python
# Append a single element
soil_orders = ['Gelisols','Histosols','Spodosols']
soil_orders.append('Andisols') # Add new item to the existing list
print(soil_orders)

```

    ['Gelisols', 'Histosols', 'Spodosols', 'Andisols']


### Append multiple elements to a list


```python
# Append multiple elements
soil_orders = ['Gelisols','Histosols','Spodosols']
extra_soil_orders = ['Oxisols','Aridisols','Vertisols','Ultisols']
soil_orders.append(extra_soil_orders)
print(soil_orders)

```

    ['Gelisols', 'Histosols', 'Spodosols', ['Oxisols', 'Aridisols', 'Vertisols', 'Ultisols']]


>Note the difference in the output list between `append()` in the previous code cell and `extend()` in the following code cell. In the previous cell the appended list appears nested, while the `extend()` method truly merges both lists to make a single, larger list.

### Merge lists


```python
# Extend list
soil_orders = ['Gelisols','Histosols','Spodosols']
extra_soil_orders = ['Oxisols','Aridisols','Vertisols']

soil_orders.extend(extra_soil_orders)
print(soil_orders)

```

    ['Gelisols', 'Histosols', 'Spodosols', 'Oxisols', 'Aridisols', 'Vertisols']


### Delete element from list


```python
# Eliminate last element
print(soil_orders) # Print original list
soil_orders.pop(1) # Eliminate the second element of the list and print remaining elements
print(soil_orders)

```

    ['Gelisols', 'Histosols', 'Spodosols', 'Oxisols', 'Aridisols', 'Vertisols']
    ['Gelisols', 'Spodosols', 'Oxisols', 'Aridisols', 'Vertisols']



```python
# An alternative method to delete one or more elements of the list.
del soil_orders[1:3]
print(soil_orders)
```

    ['Gelisols', 'Aridisols', 'Vertisols']


## 2. Tuples

Tuples are perhaps not as popular as lists, but they have an important property: they are `immutable`. Let's define a tuple and try to change its content to see what happens.



```python
# Tuples (immutable)
mytuple = (1,2,3)
type(mytuple)

```




    tuple




```python
mytuple[0] = 11; # This will throw an error. We can't change the value of a tuple (immutable).
```

## 3. Dictionaries

Dictionaries are an extremely versatile data structure. One of the most powerful features is the ability to store and retrieve information using names. When we use lists we need to know the content in each postion of the list to retrieve the right piece of information. If we don't then we need to do some sort of matching process. With `dictionaries` this type of operations are easier and much more intuitive. Often times our datasets contain names of cities, laboratories, treatments, etc. that we can easily remember and use to access data.

Dictionaries are defined using `{}` and data inside dictionaries is associated to names using `name:value` pairs.


```python
# A dictionary (key:value pairs, similar to JSON)
myDict = {'country': ['Argentina','Brazil','USA'], 
          'capital': ['Buenos Aires','Brasilia','Washington D.C.'],
          'airTemp': [28, 34, -5,]}

print(myDict['country'])
print(myDict['airTemp'][2])
```

    ['Argentina', 'Brazil', 'USA']
    -5


Much of the data in the internet is stored and transferred using structures similar to dictionaries called JSON (Javascript Object Notation). These JSON structures are very much like Python dictionaries. Below I retrieved a some weather data from [OpenWeatherMap.org](https://openweathermap.org/) weather API documentation for the city of London. You can find more recent weather information at [this link.](api.openweathermap.org/data/2.5/weather?q=London/)



```python
# Here is a more concrete example using actual weather data obtained from OpenWeatherMap:

data = {"coord":{"lon":-0.13,"lat":51.51},
        "weather":[{"description":"light intensity drizzle"}],
        "base":"stations",
        "main":{"temp":280.32,"pressure":1012,"humidity":81},
        "visibility":10000,
        "wind":{"speed":4.1,"deg":80},
        "clouds":{"all":90},
        "dt":1485789600,
        "sys":{"country":"GB","sunrise":1485762037},
        "id":2643743,
        "name":"London",
        "cod":200}

print(data['coord']['lon'])
print(data['wind']['speed']) # Using apostrophes

```

    -0.13
    4.1



```python
# Get all the keys in a list
print([*data])
```

    ['coord', 'weather', 'base', 'main', 'visibility', 'wind', 'clouds', 'dt', 'sys', 'id', 'name', 'cod']


## 4. Sets


```python
# Sets (set of unique values, test membership)

states = ['Kansas', 'Texas', 'California', 'Texas', 'Alaska', 'Kansas']
uniqueStates = set(states) # 
print(uniqueStates)

print('Kansas' in uniqueStates) # Testing membership (True)
print('Iowa' in uniqueStates)   # Testing membership (False)
```

    {'California', 'Alaska', 'Kansas', 'Texas'}
    True
    False



```python
dna1 = set('ATTTGAATTA') # DNA sequence 1
dna2 = set('GGATTCGCGT') # DNA sequence 2

# Print unique bases in each DNA sequence
print(dna1)
print(dna2)
```

    {'A', 'G', 'T'}
    {'A', 'G', 'C', 'T'}

