# Lambda Functions

Lambda functions (also known as anonymous functions) are not defined or stored in a file or cell. Anonymous functions are typically short, one-line, and/or code-specific functions that may not warrant the effort to store them as a separate function. 

In Python, anonymous functions are called `lambda` functions, which to some extent it makes total sense because they are not really "anonymous". We do actually give them names, so that we can use (call, invoke) these functions at later steps in our code.

You will typically find or use `lambda` functions when passing formulas into optimization routines, but they can be used in lots of different scenarios.


## Fahrenheit to Celsius
Imagine a code in which the user passes data in degrees Fahrenheit and we need to convert these data into degrees Celsius. Instead of typing the equation every time you need to conver temperatures, we can write the expression for converting temperature units into a `lambda` function that we can call whenever we need it. Functions are just that, code encapsulated behind a name that we can invoke by simply calling its name. This way we avoid repeating code and we minimize the risk of making mistakes since we only write the main equation one time.

$$C = \frac{5}{9}(F-32)$$

The function can then be implemetned and used as follows:


```python
fahrenheitToCelsius = lambda F: 5/9*(F-32)
fahrenheitToCelsius(212)
```




    100.0



It makes sense, 212 degrees Fahrenheit are indeed equivalent to 100 degrees Celsius, which is the boiling temperature for water. Now you can call `fahrenheitToCelsius()` anywhere you want, even in other cells of the Jupyter Notebook.

## Polynomials

Polynomials are simple mathematical expressions that become handy in countless scenarios. Below is an example of a second order (i.e. quadratic) polynomial.

$$ y = \beta_1 + \beta_2 \; x + \beta_3 \; x^2$$




```python
poly2 = lambda b1,b2,b3,x : b1 + b2*x + b3*x**2
poly2(1,2,3,20)
```




    1241



## Saturation vapor pressure

Sometimes the computation of a specific variable (e.g. vapor pressure) can be done for all the temperature and relative humidity data at one time using Numpy arrays. However, if we are writing a simple Python script that requires computation of vepor pressure in multiple places, let's say within a for loop, then using a lambda function can become practical. Again, one of the best advantages is that we code the formula only once, minimizing the chances of introducing typos, particularly, when typing coefficients.

$$e = 0.611 \; exp\Bigg(\frac{17.502 T} {T + 240.97}\Bigg)$$


```python
import math
e_sat = lambda T : 0.611 * math.exp((17.502*T)/(T+240.97)) # Result is in kPa. T is in degrees Celsius
e_sat(25) # Example for T = 25 Celsius
```




    3.165946398560656



## Volume of tree trunk

Trunk volume is used in forestry and ecology to document and track the size of trees. While the actual shape of a trunk can be complex, its cylindricity allow us to make fairly good estimations using simple measurements such as trunk diameter and height.

Some of the Giant Sequoias can have trunk volumes of up to 1,486.9 m$^3$, which is about half the volume of an olympic swimming pool (50 m x 25 m x 2 m).

$$ V = \frac{\pi \; h \; (r_1^2 + r_1^2 + r_1 r_2)}{3} $$

You can learn more about measuring tree volume at <https://www.wikiwand.com/en/Tree_volume_measurement>


```python
import math
trunk_vol = lambda r1,r2,h: (math.pi * h * (r1**2 + r2**2 + r1*r2))/3

r_base = 4  # meters
r_top = 1   # meters
height = 60 # meters
trunk_vol(r_base,r_top,height) # meters^3

```




    1319.468914507713


