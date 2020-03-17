# For Loops

When we think about computers we often think as machines that help us automating tasks. For loops are central to this concept and allow us to iterate, to repeate, an operation as many times as we want.

For loops are practical to iterate over the time steps of a given process, iterate over files in a directory, and iterate over elements in an object (e.g. list, dictionary, dataframe, etc.). Together with `if` staments and boolean operations, a `for loop` is an essential operation for anyone interested in automating tasks. 


Loops require a specific syntax. The barebones of the for loop is as follows:

```python
for item in listOfItems:
    do something
```

This is basic and potentially confusing if this is the first your attempting to write a `for loop`. Let's look at an example and then let's dissect the code into parts. A trivial example is:


```python
for i in range(0, 5):
    print(i)
```

    0
    1
    2
    3
    4


`for` = reserved command that indicates we are implementing a "for loop".

`i` = temporary dummy variable that will carry the iteration value.

`in range(0,5)` = set of values that will be incrementally adopted by *i*.

`:` = punctuation mark that defines the end of the `for loop` syntax.

`print(i)` = code inside the `for loop` has to be indented by 4 spaces.


>The indentation is equivalent to 4 spaces with the spacebar or `Ctrl + ]` in Windows machines or `Cmd + ]` in Mac. **Indentation matters in Python.**



```python
soils = ["mollisols", "hapludols", "alfisols"]
for x in soils:
    print(x)

```

    mollisols
    hapludols
    alfisols



```python
# The variable name assigned to the iterator is irrelevant
for taxonomic_order in soils:
    print(taxonomic_order)
    
```

    mollisols
    hapludols
    alfisols



```python
for count,item in enumerate(soils):
    print(count)
    print(item)
```

    0
    mollisols
    1
    hapludols
    2
    alfisols



```python
# Data from OpenWeatherMap.org
# https://openweathermap.org/current

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


# Print all fields of dictionary
for i in data:
    print(i)

```

    coord
    weather
    base
    main
    visibility
    wind
    clouds
    dt
    sys
    id
    name
    cod



```python
# Print all fields of dictionary
for j in data['coord']:
    print(j)
```

    lon
    lat



```python
# Nested for loops
# Useful when we need to iterate over two variables. Common in nested data arrays or 
# 2D arrays such as images.

plots = range(1,4)
treatments = ['a','b','c']
for i in plots:
    for j in treatments:
        print('Plot:', str(i) + j) 
        
# Try swapping the order of the treatment letters and see what happens.
```

    Plot: 1a
    Plot: 1b
    Plot: 1c
    Plot: 2a
    Plot: 2b
    Plot: 2c
    Plot: 3a
    Plot: 3b
    Plot: 3c


We can easily list files in the current directory by combining a `for loop` with the `glob module`.
Even better, we can filter the file type using the file extension.


```python
# Store and print the list of text files (denoted with extension ''.txt') only
import glob
glob.os.chdir('../datasets')

for file in glob.glob("*.txt"):
    print(file)

```

    dna_sequence.txt
    IN_Bedford_5_WNW.txt
    KS_Manhattan_6_SSW.txt
    morse_lookup_table.txt
    nobel_physics.txt



```python
# An alternative way in case you want to store the file names in a list
txtfiles = []
for file in glob.glob("*.txt"):
    txtfiles.append(file)

print(txtfiles)  # This is not part of the loop
```

    ['dna_sequence.txt', 'IN_Bedford_5_WNW.txt', 'KS_Manhattan_6_SSW.txt', 'morse_lookup_table.txt', 'nobel_physics.txt']


## Break and continue

These two reserved words are extrmely useful to control the flow of loops. A break will automatically interrupt the ongoing loop. Note that in nested loops the break will only interrupt the loop in which the break was inserted, meaning that to fully break of nested loop we need to add two or more break points.


```python
# Break out of a for (or while) loop
for i in range(10):
    print(i)
    if i == 5:
        break
```

    0
    1
    2
    3
    4
    5



```python
# Combined used of the break and continue key words in a for (or while) loop

biomes = ['Tundra','Rainforest','Savanna','Freshwater','Wetlands','Marine','Coral reef','Estuaries']

for biome in biomes:

    if biome != 'Marine':
        print(biome)
        continue
    else:
        break
```

    Tundra
    Rainforest
    Savanna
    Freshwater
    Wetlands


## Note on application

`for` Loops are great for iterating over lists, but they are also great for modeling time-dependant processes. In the case of the antecedent precipitation index, today's soil water content depends on yesterday's soil water content, today's rainfall, and a loss parameter that encapsulates the effect of soil drainage and atmopsheric demand. 

You can use for loops to model population dynamics, heat transfer, chemical reactions, radioactive decay, biomass accumulation of different organisms, and the list goes on.
