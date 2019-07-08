
# For Loops

When we think about computers we often think as machines that help us automating tasks. For loops are central to this concept. For loops allows us to iterate, to repeate, an operation as many times as we want or within the range we want.


Loops require a specific syntax. The barebones of the for loop is as follows:

```python
for item in listOfItems:
    do something
```

This is basic and potentially confusing if this is the first your attempting to write a `for loop`. Let's look at an example and then let's dissect the code into parts. A trivial example is:


```python
for i in range(0, 3):
    print(i)
```

    0
    1
    2


`for` command that indicates we are implementing a "for loop". This is a reserved word.

`i` this is a (dummy) variable that will change its value in each iteration.

`in range(0,3)` we let the *i* variable know in which range it has to change

`:` it means that whatever comes next is going to be the code to execute within the for loop


> Note the indentation of the `print` command. The indentation is equivalent to 4 spaces with the spacebar or `Ctrl + ]` in Windows machines or `Cmd + ]` in Mac. **Indentation matters in Python.**



```python
soils = ["mollisols", "hapludols", "alfisols"]
for x in soils:
    print(x)

```

    mollisols
    hapludols
    alfisols



```python
for taxonomic_order in soils:
    print(taxonomic_order)
    
```

    mollisols
    hapludols
    alfisols



```python
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
        print(str(i) + j) 
        
# Try swapping the order of the treatment letters and see what happens.
```

    1a
    1b
    1c
    2a
    2b
    2c
    3a
    3b
    3c


We can easily list files in the current directory by combining a `for loop` with the `glob module`.
Even better, we can filter the file type using the file extension.


```python
# Store and print the list of text files (denoted with extension ''.txt') only
import glob
glob.os.chdir('/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets')

for file in glob.glob("*.txt"):
    print(file)

```

    ch1_down_the_rabbit_hole.txt
    dna_sequence.txt
    jabberwocky_lewis_carroll.txt
    morse_lookup_table.txt
    nobel_physics.txt



```python
# An alternative way in case you want to store the file names in a list
txtfiles = []
for file in glob.glob("*.txt"):
    txtfiles.append(file)

print(txtfiles)  # This is not part of the loop
```

    ['ch1_down_the_rabbit_hole.txt', 'dna_sequence.txt', 'jabberwocky_lewis_carroll.txt', 'morse_lookup_table.txt', 'nobel_physics.txt']


## Break and continue

These two reserved words are extrmely useful to control the flow of loops. A break will automatically interrupt the ongoing loop. Note that in nested loops the break will only interrupt the loop in which the break was inserted, meaning that to fully break of nested loop we need to add two or more break points.


```python
# Break out of a for (or while) loop
for i in range(10):
    print(i)
    if i == 5:
        break
```


```python
# Combined used of the break and continue key words in a for (or while) loop
word = 'hello'
for i in word:
    print(i)
    if i != 'l':
        continue
    else:
        break
```

    h
    e
    l


## Note on application of `for loops`
Loops are great for iterating over lists, but they are also great for modeling time-dependant processes. In the case of the antecedent precipitation index, today's soil water content depends on yesterday's soil water content, today's rainfall, and a loss parameter that encapsulates the effect of soil drainage and atmopsheric demand. 

You can use for loops to model population dynamics, heat transfer, chemical reactions, radioactive decay, biomass accumulation of different organisms, and the list goes on.
