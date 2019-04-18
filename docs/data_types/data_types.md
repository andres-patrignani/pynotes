
# Data types

At first glance it may seem that commands such as `a = 4` and `a = 4.0` are the same, however, for the computer these statements represent two different data types. Let's explore the different ways in which we can store infromation in Python. We will visit different data type, but we will cover data structures in a different tutorial. Briefly, a data structure is the container of one or more data types. For instance, lists can store different datatypes such as strings, integers, and floats. See data type refers to each single element or item, while data structures refer to the container.

## Integers


```python
a = 8
print(a)
print(type(a)) # This is class int

```

    8
    <class 'int'>


## Floating point


```python
b = 8.71
print(b)
print(type(b)) # This is class float (numbers with decimal places)

```

    2.71
    <class 'float'>


## Strings


```python
# Strings are defined by apostrophes or quotation marks
c = 'This is a string'
d = "This is another string"
print(c)
print(type(c)) # This is class string (text)
print(type(d))

```

    This is a string
    <class 'str'>
    <class 'str'>



```python
# What happens when we want to add an integer to a string?
var1 = 25
var2 = '3'
print(var1 + var2)

# We get an error: unsupported operand type(s) for +: 'int' and 'str'
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-5-258441fbe6da> in <module>()
          2 var1 = 25
          3 var2 = '3'
    ----> 4 print(var1 + var2)
          5 
          6 # We get an error: unsupported operand type(s) for +: 'int' and 'str'


    TypeError: unsupported operand type(s) for +: 'int' and 'str'



```python
# If we convert the string into an integer then the command will generate the right result.
print(var1 + int(var2))
```

    28


## Booleans


```python
# Boolean (True or False)
myBool = [True,True,False,True]
print(type(myBool))
print(type(myBool[0]))

print('orange' == 'apple')

```

    <class 'list'>
    <class 'bool'>
    False



```python
value = 7                           # Value is lower than 10, but not lower than 5. 
print( (value < 5) & (value < 10) ) # False. Both must be true to return True
print( (value < 5) | (value < 10) ) # True. One condition is suffient to return True
```

    False
    True



The following command will print `false`: `print( value < 5 & value < 10 )`. Make sure you group the individual statements between `AND` (`&`) and `OR` (`|`)

## Ranges


```python
# range(start,end,step) 
# Ranges will exclude the last number, so you need to go one extra value to include the last number
# Ranges will also not display the sequence of numbers unless we force the range to be a list

values = range(2,8,2) # Numbers from 2 to 6.
print(values)
print(type(values))
print(list(values))

```

    range(2, 8, 2)
    <class 'range'>


## Integers to strings


```python
### Integers to string
print(type(a))         # Print data type before conversion
my_string = str(a)     # Store converted string into a separate variable to avoid overwriting 'a'
print(my_string)
print(type(my_string)) 

# A one-line command:
print(type(str(a))) 

```

    8
    <class 'str'>


## Float to string


```python
### Floats to string
print(type(b))       # Print data type before conversion
print(type(str(b)))  # Print resulting data type 
```

    <class 'float'>
    <class 'str'>


## String to integers and floats


```python
# Strings to integers/floats
my_float = float('3')
print(my_float)
print(type(my_float))

# Check if string is numeric
str.isnumeric('3')  # Returns True

```

    3.0
    <class 'float'>





    True



## Floats to integers


```python
### Floats to integers
my_int = 4.0
print(type(my_int))
print(type(int(my_int)))
```

    <class 'float'>
    <class 'int'>



```python
# Sometime Python will convert from one data type to another based on a given operation
a = 5 / 2  # Two integers
print(a)
print(type(a))   # Result is a float

```

    2.5
    <class 'float'>

