# Python help

Python is one of the most popular languages for data science and as a consequence there is a large community generating content and helping new programmers. The Python Standard Library is the official and latest documentation for Python core modules: <https://docs.python.org/3/>. 

External modules common in scientific computing such as Numpy, Scipy, and Matplotlib have separate documentation website exclusively dedicated to the module. Most online documentation resources contain plenty of detail about input and output arguments, datatype of the required inputs, examples, and notes on special behavior of the code. 

For instance, here is the official documentation for the `round()` function : <https://docs.python.org/3/library/functions.html?highlight=round#round>

If you need quick access to learn more about the input arguments of the function below are some additional alternatives that you can use using the Python interpreter. Note that for the case of the `round()` function the options below do not provide the full documentation such as the special behavior of the function due to float representation.


```python
# The help command
help(round)
```

    Help on built-in function round in module builtins:
    
    round(number, ndigits=None)
        Round a number to a given precision in decimal digits.
        
        The return value is an integer if ndigits is omitted or None.  Otherwise
        the return value has the same type as the number.  ndigits may be negative.
    



```python
# An alternative help shortcut command
round?
```


    [0;31mSignature:[0m [0mround[0m[0;34m([0m[0mnumber[0m[0;34m,[0m [0mndigits[0m[0;34m=[0m[0;32mNone[0m[0;34m)[0m[0;34m[0m[0m
    [0;31mDocstring:[0m
    Round a number to a given precision in decimal digits.
    
    The return value is an integer if ndigits is omitted or None.  Otherwise
    the return value has the same type as the number.  ndigits may be negative.
    [0;31mType:[0m      builtin_function_or_method



## References

Van Rossum, G. and Drake, F.L., 2011. The python language reference manual. Network Theory Ltd..
