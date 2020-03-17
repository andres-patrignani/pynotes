# Handy functions for data science

To use the functions below you need to know how to:

- Define a custom function
- Create Lambda functions: An short anonymous function in python
- Know the basic structure of iterables. An iterable is simply a list, tuple, or dictionary over which the functions below will iterate over.

## Map

`map(function, iterable)`

Function that maps individual values in one list to another list following a specific function. If you are alrady using the Numpy module in your code, then you might be able to perform element-wise operations without the need for the `map()` functions.



```python
celsius = [0,10,20,100]
fahrenheit = list(map(lambda x: (x*9/5)+32, celsius))
print(fahrenheit)

```

    [32.0, 50.0, 68.0, 212.0]



```python
# Convert a DNA sequence into RNA
# RNA doesn't contain thymine bases, but it contains uracil
dna = 'ATTCGGGCAAATATGC'
lookup = dict({"A":"U", "T":"A", "C":"G", "G":"C"})
rna = list(map(lambda x: lookup[x], dna))
print(''.join(rna))

```

    UAAGCCCGUUUAUACG


## Filter

`filter(function, iterable)`


```python
# Get the occurrence of all adenine nucleotides
dna = 'ATTCGGGCAAATATGC'
list(filter(lambda x: x == "A", dna))

```




    ['A', 'A', 'A', 'A', 'A']




```python
def hydrophobic(contact_angle):

    if contact_angle < 90:
        repel = False
    elif contact_angle >= 90 and contact_angle <= 180:
        repel = True
    
    return repel

contact_angles = [5,10,20,50,90,150]
list(filter(hydrophobic, contact_angles))

```




    [90, 150]



## Reduce

`reduce(function, iterable)`

This is basically a rolling computation


```python
from functools import reduce
```


```python
# Compute the cumulative sum
cumsum = reduce(lambda x, y: x+y, range(0,101)) 
print(cumsum)

```

    5050


>Again, if you are handling data with Pandas or Numpy these functions may not look that powerful. I particularly like the `map()` function combined with dictionaries.



```python

```
