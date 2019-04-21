
# **Error handling**

Erro handling may seem out of the scope for non-computer science students beginning to code. While you don't have to handle every exception when you are learning to code, it's a good idea to at least learn the basics. In this short snippet of code I want to introduce you to the `try` and `except` blocks, which allow you to set specific error messages. Again not something that you want to do after learning `hello world` but something that you can add to improve your projects. The idea is to let the user know why the code is not working.

The idea is to *try* some code. If it works properly, then the interpreter will not execute the lines insde `except`. However, if the interpreter finds a valid expression or generates an invalid output, then (instead of immediately crashing) it will execute the code inside `except`. Here the idea is to capture things that went wrong and then provide a meaningful error message. Because many things can go wrong (e.g. input was required to be a string and the user passed a float, etc.) we can use `if statemetns` to check some scenarios.

> *Running properly* means that the python expressions are valid. It does not mean that the result of the computation makes sense for your particular application.


```python
# try-except method

try:
    some statements here
except:
    exception handling
```


```python
input_value = '5'

try:
    input_value + 1
except:
    print('Value is not a number')
```

    Value is not a number



```python
# Custom error message (see last line of the error message)
input_value = '5'

try:
    input_value + 1
except:
    raise Exception('Value is not a number')
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-2-d8208d031f92> in <module>()
          3 try:
    ----> 4     input_value + 1
          5 except:


    TypeError: can only concatenate str (not "int") to str

    
    During handling of the above exception, another exception occurred:


    Exception                                 Traceback (most recent call last)

    <ipython-input-2-d8208d031f92> in <module>()
          4     input_value + 1
          5 except:
    ----> 6     raise Exception('Value is not a number')
    

    Exception: Value is not a number

