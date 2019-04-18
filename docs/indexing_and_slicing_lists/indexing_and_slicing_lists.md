
# Indexing and slicing

Learning how to access information stored in lists is essential to use the data in our code.
Accesing data in lists probably deserves a separate tutorial. So here we go.


```python
#                 +---+---+---+---+---+---+
#                 | P | y | t | h | o | n |
#                 +---+---+---+---+---+---+
# Slice position: 0   1   2   3   4   5   6
# Index position:   0   1   2   3   4   5

# array[start:stop:step]

# Source: https://stackoverflow.com/questions/509211/understanding-slice-notation

# Definitions
# start: the beginning index of the slice, it will include the element at this index unless 
# it is the same as stop, defaults to 0, i.e. the first index. If it's negative, it means to 
# start n items from the end.

# stop: the ending index of the slice, it does not include the element at this index, 
# defaults to length of the sequence being sliced, that is, up to and including the end.

# step: the amount by which the index increases, defaults to 1. 
# If it's negative, you're slicing over the iterable in reverse.

p = ['P','y','t','h','o','n']

# Indexing
# Remember that indexing results in items, not lists
p[0]
p[5]
print(type(p[5]))

# Slicing
# Remember that slicing results in lists
p[0:1]
p[0:2]
print(type(p[0:2]))

# Get last 3 letters
print(p[-3:]) # This means: "3rd from the end, to the end."

# Technically this is what is going on behind the Python interpreter
# sliceable[start:stop:step]
print(p[-3:len(p):1])

# The colon is what tells Python you're giving it a slice and not a regular index. 

print(p[-3:-1]) # This will not return the last element. You can use -1 for indexing. 
# Since this is a slicing operation we need to use the : operator
```
