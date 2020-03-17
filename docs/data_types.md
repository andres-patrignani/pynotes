# Data Types

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
euler = 2.718
print(euler)
print(type(euler)) # This is class float (numbers with decimal places)

```

    2.718
    <class 'float'>


## Strings


```python
# Strings are defined by apostrophes or quotation marks
quote = 'Simplicity is the ultimate sophistication'
author = "Leonardo DaVinci"
print(quote)
print(type(quote))
print(type(author))

```

    Simplicity is the ultimate sophistication
    <class 'str'>
    <class 'str'>



```python
# What happens when we want to add an integer to a string?
var1 = 25
var2 = '3'
print(var1 + var2)

# We get an error: unsupported operand type(s) for +: 'int' and 'str'
```


```python
# If we convert the string into an integer then the command will generate the right result.
print(var1 + int(var2))
```

    28


## Booleans

A boolean operation tests whether an expression is `True` or `False`. Booleans play a decisive role in controlling the flow of your code using logic. Here is where human knowledge is transferred to the machine in terms of logical statements. The idea is that depending on the result of a statement, then different code will be triggered. For this reason it makes sense that booleans are usually coupled with `if` statements to control de flow of the code.

> Python evaluates conditional arguments from left to right. The evaluation stops as soon as the outcome is determined and the resulting value is returned. Python does not evaluate subsequent operands unless it is necessary to resolve the result.

### Boolean logical operators

`or`: Will evaluate to `True` **if at least one (but not necessarily both)** statements is `True`

`and`: Will evaluate to `True` **only if both** statements are `True`

`not`: Reverses the result of the statement

### Boolean comparison operators include:

`==`: equal

`!=`: not equal

`>=`: greater or equal than

`<=`: less or equal than

`>`: greater than

`<`: less than



```python
# Test for equality
'TAA' == 'ATG' # Stop codon vs methionine codon
```




    False




```python
# Show data type
type('TAA' == 'ATG')
```




    bool




```python
# Test for inequality
'TAA' != 'ATG'
```




    True




```python
# Define an example aminoacid codon
serine = 'AGU'
```


```python
# AND conditional
serine == 'CGC' and serine == 'AGU'
```




    False




```python
# OR conditional
serine == 'CGC' or serine == 'AGU'
```




    True




```python
# Reverse result
not serine == 'AGU' or serine == 'CGC'
```




    False




```python
# Combined AND-OR conditional (evaluated from left to right)
serine == 'AGU' or serine == 'CGC' and serine == 'CCA'
```




    True




```python
# Grouping statements is essential to ensure desired logic
(serine == 'AGU' or serine == 'CGC') and serine == 'CCA'
```




    False




```python
# Checks the entire expression
[1,2,3] == [3]
```




    False




```python
# Use & and | to make element-wise comparisons
# For this we need to use numpy arrays

import numpy as np
np.array([1,2,3]) == [3]

np.fromiter(serine, (np.unicode,1)) == 'G'
```




    array([False,  True, False])



### Leap year example

Leap years refer to the years in which we add an extra day to the calendar to keep it synchronized with the time it takes for the Earth to complete one revolution around the Sun. It takes Earth approximately 365.242189 days, so during leap years we add an extra day to the month of February, meaning that in leap years February has a total of 29 days. 

>Every year that is exactly divisible by four is a leap year, except for years that are exactly divisible by 100, but these centurial years are leap years if they are exactly divisible by 400. For example, the years 1700, 1800, and 1900 are not leap years, but the years 1600 and 2000 are (United States Naval Observatory).

- `Divisible by 4`. So, remainder of year by 4 must be equal to zero.

- `Not divisible by 100`. So, remainder of year by 100 must be greater than zero.

- `Divisible by 400`. So, remainder of year by 400 must be equal to zero.

Example leap years = 2016, 2020, 2024, 2000, 2400

Example non leap years = 2100, 2200


```python
year = 2000
leap = year%4 == 0 and year%100 > 0 or year%400 == 0
print(leap)
```

    True


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
    [2, 4, 6]


## Integers to strings


```python
### Integers to string
int_num = 8
print(type(int_num))   # Print data type before conversion
print(int_num + 3)     # This will work

int_str = str(int_num) # Store converted string into a separate variable to avoid overwriting 'a'
print(type(int_str)) 

#print(int_str + 3)     # This will not work
```

    <class 'int'>
    11
    <class 'str'>


## Float to string


```python
### Floats to string
float_num = 3.1415
print(type(float_num))       # Print data type before conversion
print(type(str(float_num)))  # Print resulting data type 
```

    <class 'float'>
    <class 'str'>


## String to integers and floats


```python
# Strings to integers/floats
float_str = '3'
float_num = float(float_str)
print(type(float_num))

# Check if string is numeric
float_str.isnumeric()
```

    <class 'float'>





    True



## Floats to integers


```python
# Floats to integers
float_num = 4.9
int_num = int(float_num)
print(type(int_num))
print(int_num)
```

    <class 'int'>
    4


In some cases Python will change the class according to the operation. For instance, the following code starts from two integers and results in a floating point.


```python
numerator = 5
denominator = 2
print(type(numerator))
print(type(denominator))

answer = numerator / denominator  # Two integers
print(answer)
print(type(answer))   # Result is a float

```

    <class 'int'>
    <class 'int'>
    2.5
    <class 'float'>

