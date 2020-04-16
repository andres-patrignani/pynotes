# Calculator Examples 

Simple examples of using Python as a regular calculator.

## Convert from degrees Fahrenheit to Celsius


```python
fahrenheit = 45
celsius = (fahrenheit - 32) * 5/9
print("The temperature is:", round(celsius,2), "degrees Celsius")

```

    The temperature is: 7.22 degrees Celsius


**Question**: Why do we need to separate inputs by commas?

**Answer**: The print function can take multiple arguments as inputs. In this context, a comma act as a delimiter between the inputs.

## Convert from inches to meters


```python
inches = 72 # inches
centimeters = inches * 2.54
print(round(inches,1),"inches are equivalent to",round(centimeters,1),"centimeters")

```

    72 inches are equivalent to 182.9 centimeters


## Compute the hypotenuse

Given that `a` and `b` are the sides of a right triangle, compute the hypotenuse. For instance, if `a = 3` and `b = 4`, then the command must return a value of `5`.


```python
a = 3.0 # [cm]
b = 5.5 # [cm]
hypotenuse = (a**2 + b**2)**0.5
print('Hypotenuse is:', round(hypotenuse,2),'cm')

```

    Hypotenuse is: 6.26 cm

