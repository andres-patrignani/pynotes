
# **Math module**

A simple core Python module for common mathematical operations. 


```python
import math

# Define a value
value = 90.87
print(value)

# The math module does not have a round function.
# Python has a round function as part of its core functions.
print(round(value,1))      # The second argument specifies the number of decimal places

print(math.pi)             # Pi
print(math.floor(value))   # largest integer smaller than or equal to a given number.
print(math.ceil(value))    # smallest integer greater than or equal to a given number.
print(math.cos(value))     # cosine
print(math.sin(value))     # sine
print(math.isnan(value))   # check if value is NaN
print(math.radians(value)) # pi-radians
print(math.cos(0))
```

    90.87
    90.9
    3.141592653589793
    90
    91
    -0.9722372823920781
    0.23399715110844135
    False
    1.5859806912872474
    1.0



```python
# Calculate area of a circle
diameter = 2.5 # cm
circle_area = math.pi * (diameter/2)**2
print('The area of a circle with a diameter of {diameter} cm has an area of {circle_area}'.format(diameter=diameter,circle_area=circle_area))
```

    The area of a circle with a diameter of 2.5 cm has an area of 4.908738521234052



```python
# Calculate vapor pressure deficit using math module

airTemperature = 35; # Celsius
relativeHumidity = 45; # Percent

saturatedVaporPressure = 0.611 * math.exp((17.502*airTemperature)/(airTemperature + 240.97)); # kPa
actualVaporPressue = saturatedVaporPressure * relativeHumidity/100;

vaporPressureDeficit = saturatedVaporPressure - actualVaporPressue
print(str(vaporPressureDeficit) + ' kPa')
print(str(round(vaporPressureDeficit,2)) + ' kPa')
```

    3.0931886137027216 kPa
    3.09 kPa

