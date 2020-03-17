# Math Module

One of the most useful core Python modules for common constants, arithmetic, and trigonometric operations.


```python
import math
```

## Rounding

Before we move too far I need to let you know that the math module does not have a round function. The `round()` function is readily available when we load Python without the need for importing the `math` module. However, the `math` module has some handy methods for rounding up and down.


```python
value = 90.8752
print(value)

# The round function
print(round(value,2))      # The second argument specifies the number of decimal places

# Floor and ceil and 
print(math.floor(value))   # largest integer smaller than or equal to a given number.
print(math.ceil(value))    # smallest integer greater than or equal to a given number.

```

    90.8752
    90.88
    90
    91


## Assign and check for NaNs

`NaN` stands for "Not a Number" and although it may appear to be a string, it is actually interpreted as a `float`. `NaN` is typically used as a placeholder for missing values. 

>Do not confuse the built-in `NaN` with the Numpy `NaN`.


```python
nan_value = float("Nan")
print(math.isnan(value))   # check if value is NaN
print(math.isnan(nan_value))
print(type(nan_value))
```

    False
    True
    <class 'float'>


## Trigonometric operations

A really useful feature is that it also provides with common constants such as Pi. It also has a function to compute radians, making this core module an excellent choice for trigonometric operations involving the computation of Euclide distance, perimeter, area, and volume of many geometric objects.



```python
# Pi constant
print(math.pi)
```

    3.141592653589793



```python
# Radians
angle = 180 # degrees
print(math.radians(angle))
```

    3.141592653589793



```python
# Trigonometric functions (input is in radians)
math.sin?

```


    [0;31mSignature:[0m [0mmath[0m[0;34m.[0m[0msin[0m[0;34m([0m[0mx[0m[0;34m,[0m [0;34m/[0m[0;34m)[0m[0;34m[0m[0m
    [0;31mDocstring:[0m Return the sine of x (measured in radians).
    [0;31mType:[0m      builtin_function_or_method




```python
math.radians(90)
```




    1.5707963267948966



The docstring of the `sin()` method clearly states that the input argument must be in radians. Note that the computer does not know whether the value is alrady in radians or not. For instance, if want to compute the sine of 90 degrees and we type `math.sin(90)`, the computer assumes that we want the sine of `90 radians`. So, it is our job to ensure that the outputs are meaningful within the context of our particular application.

>Always try naive examples fro which you know the answer. This way you can quickly detect if something is not right.


```python
# cosine
print(math.cos( math.radians(0) ))

# sine
print(math.sin( math.radians(90) ))
```

## Calculate area of a circle

A simple example to use the math module


```python
diameter = 2.5 # cm
circle_area = round(math.pi * (diameter/2)**2, 1)
print('A circle of {diameter} cm in diameter has an area of {area} cm^2'
      .format(diameter=diameter,area=circle_area)) 
```

    A circle of 2.5 cm in diameter has an area of 4.9 cm^2


## Calculate vapor pressure deficit using math module

You can also use the `math` module for many other calculations. The example below for computing the vapor pressure deficit (VPD) based on air temperature and air relative humidity amkes use of the exponential function `math.exp()`. Computing the VPD takes three steps:

1. Compute the hypothetical saturated vapor pressure of air

2. Compute actual vapor pressure of the air

3. The difference between the two is know as the vapor pressure deficit

### Formula for saturated vapor pressure

This is know as the Tetens equation for temperatures above 0 Celsius and has units of kPa:

$$ e_s = 0.611 \; exp\Bigg( \frac{17.3 \; T_{air}}{T_{air} + 237}\Bigg)$$

$T_{air}$ is air temperature in degrees Celsius

### Formula for actual vapor pressure

$$ e_a = e_s * RH/100$$

$RH$ is relative humidity expressed as a percentage

### Formula for vapor pressure deficit

$$ VPD = e_s - e_a $$


```python
T_air = 35; # Celsius
RH = 45; # Percent

e_sat = 0.611 * math.exp( (17.3*T_air)/(T_air + 237) ) # kPa
e_act = e_sat * RH/100;

vpd = e_sat - e_act
print(str( round(vpd,2) ) + ' kPa')
```

    3.11 kPa


## References

Official Python Math module documentation: https://docs.python.org/3/library/math.html
