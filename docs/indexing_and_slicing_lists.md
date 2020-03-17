# Indexing and slicing

Learning how to access information stored in lists allow us to handle and use the data in our code.

To access information stored in Python lists it helps if we understand the concepts of indexing and slicing. Indexing is accessing list items by their position on the list, while slicing is about retrieving a "slice" of one or more values from the list. Here is a basic sketch to illustrate indexing and slicing:

```
                             +---+---+---+---+---+---+
                             | P | y | t | h | o | n |
                             +---+---+---+---+---+---+
             Slice position: 0   1   2   3   4   5   6
             Index position:   0   1   2   3   4   5
```

## Indexing

To retrieve an element from the list, we need to specify the postion of the item in the list. **Indexing results in items, not lists**.

**Indexing syntax**: `list[index]`


```python
python_word = ['P','y','t','h','o','n']

# Indexing
print(p[0])
print(p[5])
print(type(p[5]))

```

    P
    n
    <class 'str'>


## Slicing

Extract one or more elements from a list. **Slicing results in a list** and requires the use of the `:` operator.

**Slicing syntax**: `array[start:stop:step]`

`start`: the beginning index of the slice, it will include the element at this index unless it is the same as the `stop`, defaults to 0, i.e. the first index. If it's negative, it means to 
start n items from the end.

`stop`: the ending index of the slice, it does not include the element at this index, defaults to length of the sequence being sliced, that is, up to, and including, the end.

`step`: the amount by which the index increases, defaults to 1. If it's negative, you're slicing over the iterable in reverse.

>The colon is what tells Python that you are giving it a slice and not a regular index. 
Remember that slicing results in lists.


```python
# Single element
p[0:1]

```




    ['P']




```python
# More than two elements
p[0:2]

```




    ['P', 'y']




```python
print(type(p[0:2]))
```

    <class 'list'>



```python
# Get last 3 elements
print(p[-3:]) # This means: "3rd from the end, to the end."

```

    ['h', 'o', 'n']



```python
# Technically this is what is going on behind the Python interpreter
print(p[-3:len(p):1])

```

    ['h', 'o', 'n']


The following statement will not return the last element.


```python
print(p[-3:-1])

```

    ['h', 'o']


## References

Source: https://stackoverflow.com/questions/509211/understanding-slice-notation

