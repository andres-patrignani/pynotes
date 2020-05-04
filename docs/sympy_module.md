# Sympy

A module for symbolic math. In this short set of examples we will cover derivatives, numerical integration, and analytical integration.



```python
import sympy as sp
import numpy as np
import matplotlib.pyplot as plt

```

## Derivation


```python
# Define symbolic variables
x = sp.symbols('x')

```


```python
# First derivative (default, last function argument technically not needed)
first_derivative = sp.diff(5*x**2 + 2*x + 115, x, 1)
first_derivative

```




$\displaystyle 10 x + 2$




```python
# Second derivative
second_derivative = sp.diff(5*x**2 + 2*x + 115, x, 2)
second_derivative

```




$\displaystyle 10$



## Integration


```python
fun = lambda x: -0.003*x**2 + x + 100
lower_xlim = 0
upper_xlim = 300
x = np.linspace(lower_xlim, upper_xlim,1000)
y = fun(x)

plt.figure()
plt.plot(x,y)
plt.show()
```


![png](sympy_module_files/sympy_module_7_0.png)


## Numerical integration


```python
# Trapezoidal rule
numerical_int = np.trapz(y, x)
print(numerical_int)

```

    47999.98647295944


## Analytical integration


```python
# Indefinite integral
x, y = sp.symbols('x y')
sp.integrate(fun(x), x)

```




$\displaystyle - 0.001 x^{3} + 0.5 x^{2} + 100.0 x$




```python
# Definite integral
analytical_int = sp.integrate(fun(x), (x, lower_xlim, upper_xlim))
print(analytical_int)

```

    48000.0000000000



```python
# Difference between analytical and numerical
error_int = analytical_int - numerical_int
relative_error = error_int / analytical_int * 100
print(relative_error)

```

    2.81813344978824e-5

