
# Anonymous Functions (Lambda functions)

Anonymous functions are short, one-line functions that are not worth having as a separate function.
Typically use to define polynomials or other simple mathematical models.

**Fahrenheit to Celsius**:
$$C = \frac{5}{9}(F-32)$$


```python
fahrenheitToCelsius = lambda F: 5/9*(F-32)
fahrenheitToCelsius(212)
```




    100.0



**Second order polynomial**: 
$$ y = \beta_1 + \beta_2 \; x + \beta_3 \; x^2$$




```python
poly2 = lambda b1,b2,b3,x : b1 + b2*x + b3*x**2
poly2(1,2,3,20)
```




    1241



**Saturation vapor pressure**: 
$$0.611 \; exp\Bigg(\frac{17.502 T} {T + 240.97}\Bigg)$$


```python
import math
e_sat = lambda T : 0.611 * math.exp((17.502*T)/(T+240.97)) # Result is in kPa. T is in degrees Celsius
e_sat(25) # Example for T = 25 Celsius
```




    3.165946398560656



**Air pressure as a function of altitude**: 
$$101.3 \; exp\Bigg(\frac{-A}{8200}\Bigg)$$


```python
import math
p_alt = lambda A: 101.3*math.exp(-A/8200) # atmospheric pressure in kPa. Altitude is in meters above sea level.
p_alt(1000)
```




    89.66990376427852


