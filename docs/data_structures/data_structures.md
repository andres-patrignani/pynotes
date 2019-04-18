
# Data Structures

This notebook introduces differen data structures (containers) in which we can store information in Python. Many times the same information can be stored in more than one data structure. The choice of the right data structure will depend on the application and, to a great extent, on your experience and coding preferences.

## 1. Lists


```python
# Lists (also known as arrays, they are mutable)

mylist = [1,2,3]  # A row vector
print(mylist)
type(mylist)

# What happens if you replace one of the list elements above by another element?
# Can you mix words with numbers?
# Can you nest multiple lists?
```

    [1, 2, 3]





    list




```python
# Lists are mutable, we can re-define them
mylist = [2000,2001,2002]
```


```python
# Determine the number of elements in a list
print(len(mylist))
```

    3



```python
# Lists can be nested
mylist = [2000,2001,2002,2003,['apple','orange']]
print(mylist[0])
print(mylist[4])
print(mylist[4][0])
```

    2000
    ['apple', 'orange']
    apple



```python
# Copy and save part of the list in a different variable
fruits = myList[4]
print(fruits[0])
```


```python
# List methods
print(mylist.count(2000)) # Count specific element. Takes only one argument
mylist.index(2001) 
mylist.append([2003,2004])
print(mylist)
```

    1





    [2000, 2001, 2002, 2003, ['apple', 'orange'], [2003, 2004]]




```python
# Will this work?
mylist.count('apple')

# Solution: mylist[4].count('apple')
```




    1




```python
# Clear list and check that list was cleared (it should print empty brackets)
mylist.clear()
print(mylist)
```

    []



```python
# Creating a matrix or 2D array
M = [[1, 4, 5],
    [-5, 8, 9]]
print(M)
```

    [[1, 4, 5], [-5, 8, 9]]



```python
# Here is a more challenging example:
B = [ [1,2,3], [4,5,6, ['a','b','c']]]

B[1][3][0]  # returns the letter 'a'
```

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

# Appended list was nested

```

    ['Gelisols', 'Histosols', 'Spodosols', ['Oxisols', 'Aridisols', 'Vertisols', 'Ultisols']]


### Extend (merge) lists


```python
# Extend list
soil_orders = ['Gelisols','Histosols','Spodosols']
extra_soil_orders = ['Oxisols','Aridisols','Vertisols','Ultisols']

soil_orders.extend(extra_soil_orders)
print(soil_orders)

```

    ['Gelisols', 'Histosols', 'Spodosols', 'Oxisols', 'Aridisols', 'Vertisols', 'Ultisols']


### Delete element from list


```python
# Eliminate last element
print(soil_orders) # Print original list
soil_orders.pop(0) # Eliminate the first element of the list and print remaining elements
print(soil_orders)
```

    ['Gelisols', 'Histosols', 'Spodosols', 'Oxisols', 'Aridisols', 'Vertisols', 'Ultisols']
    ['Histosols', 'Spodosols', 'Oxisols', 'Aridisols', 'Vertisols', 'Ultisols']


## 2. Tuples


```python
# Tuples (immutable)
mytuple = (1,2,3)
type(mytuple)

mytuple[0] = 11; # This will throw an error. We can't change the value of a tuple (immutable).
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-3-8b995db9af43> in <module>()
          3 type(mytuple)
          4 
    ----> 5 mytuple[0] = 11; # This will throw an error. We can't change the value of a tuple (immutable).
    

    TypeError: 'tuple' object does not support item assignment


## 3. Dictionaries


```python
# A dictionary (key:value pairs, similar to JSON)
myDict = {'country': ['Argentina','Brazil','Uruguay','USA'], 
          'capital': ['Buenos Aires','Brasilia','Montevideo','Washington D.C.'],
          'airTemp': [28, 12, 14, 35,]}

print(myDict)
print(myDict['country'])
print(myDict['airTemp'][3])
type(myDict)
print("My country is",myDict['country'],"and the capital is",myDict['capital'][0])
```

    {'country': ['Argentina', 'Brazil', 'Uruguay', 'USA'], 'capital': ['Buenos Aires', 'Brasilia', 'Montevideo', 'Washington D.C.'], 'airTemp': [28, 12, 14, 35]}
    ['Argentina', 'Brazil', 'Uruguay', 'USA']
    35
    My country is ['Argentina', 'Brazil', 'Uruguay', 'USA'] and the capital is Buenos Aires



```python
# Add key:value pairs to a dictionary
D = {}
D['group1'] = 'Richard Feynman'
D['group2'] = 'Albert Einstein'
D['group3'] = 'Max Planck', 'Niels Bohr'         # Added as a tuple
D['group4'] = ['Enrico Fermi', 'Hideki Yukawa']  # Added as a list

print(D)
print(type(D['group3']))
print(type(D['group4']))
```

    {'group1': 'Richard Feynman', 'group2': 'Albert Einstein', 'group3': ('Max Planck', 'Niels Bohr'), 'group4': ['Enrico Fermi', 'Hideki Yukawa']}
    <class 'tuple'>
    <class 'list'>



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
print(data["wind"]["speed"]) # Using quotation marks
print(data['wind']["speed"]) # Using a combination of both (not recommended, but it works)

```

    -0.13
    4.1
    4.1
    4.1


## 4. Sets


```python
# Sets (set of unique values, test membership)

states = ['Kansas', 'Texas', 'California', 'Texas', 'Alaska', 'Kansas']
uniqueStates = set(states) # 
print(uniqueStates)

print('Kansas' in uniqueStates) # Testing membership (True)
print('Iowa' in uniqueStates)   # Testing membership (False)
```

    {'California', 'Kansas', 'Texas', 'Alaska'}
    True
    False



```python
dna1 = set('ATTTGAATTA') # DNA sequence 1
dna2 = set('GGATTCGCGT') # DNA sequence 2

# Print unique bases in each DNA sequence
print(dna1)
print(dna2)

dna1 - dna2   # bases in dna1 BUT NOT in dna2
dna1 | dna2   # bases in dna1 OR in dna2
dna1 & dna2   # bases in dna1 AND in dna2
dna1 ^ dna2   # bases in dna1 OR dna2, BUT NOT BOTH
```

    {'G', 'T', 'A'}
    {'G', 'C', 'T', 'A'}





    {'C'}


