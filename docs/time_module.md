# Time module

The time module is widely used when we need simple time information or when we need to test the performance of some code.


```python
import time
```

An easy way to obtain the current and detailed information about the local time is by using the `localtime()` method.


```python
clk = time.localtime()
print(clk)
type(clk)  # Type struct_time
```

    time.struct_time(tm_year=2020, tm_mon=1, tm_mday=30, tm_hour=10, tm_min=51, tm_sec=33, tm_wday=3, tm_yday=30, tm_isdst=0)





    time.struct_time



The returned object contains information about the year, hour, and even seconds. You can see the object attributes in the printed output. You can then use this information to retrieve the piece of information that you need for your application. Here are some examples, but you should explore other attributes by typing `clk.` and then the `Tab key`



```python
print(clk.tm_year)
print(clk.tm_hour)
print(clk.tm_sec)
```

    2020
    10
    33


## Python is synchronous

The Python interpreter executes one line at the time and will only proceed to the following line once the current line has finished. This means that if you have a line that takes time to execute, the entire code will be on hold. The up side of this behavior is that it makes code easy to write following a clear logical sequence. This may seem obvious, but not all programming languages behave this way.

The time module can be used to demonstrate this important propererty of Python. Let's first print few lines in sequential order:


```python
print('Executed step 1')
print('Executed step 2')
print('Executed step 3')
print('See, Python is synchronous!')
```

    Executed step 1
    Executed step 2
    Executed step 3
    See, Python is synchronous!


Using the `sleep()` method we can introduce a pause (defined in seconds) between lines of code to mimic a time consuming computation.


```python
# Applying a time delay. In this case, the time delay mimics a long computation
print('Executing step 1')
time.sleep(5)  # in seconds
print('Executing step 2')
time.sleep(3)  # in seconds
print('Executed step 3')
print('See, Python is synchronous!')
```

    Executing step 1
    Executing step 2
    Executed step 3
    See, Python is synchronous!


As you can see the "step 2" and "step 3" commands did not execute right away. 

>In Python a single line can block the execution of the entire code. Note that when the Jupyter Lab is busy there is a light gray bracket next to the code cell that shows `[*]:`, When the interpreter finished the execution of the cell code you will see a number such as `[5]:`.

## Evaluate code performance

Use the time module to check and document the performance of your script. While we can easily use the `localtime()` method, the module offers a more accurate method called `perf_counter()` that brings up the clock with the highest resolution.

Note that unlike `localtime()`, where we stored time information into a variable, the reference point of the returned value by the `perf_counter()` method is undefined, so that only the difference between the results of consecutive calls is meaningful. Source: https://docs.python.org/3/library/time.html

To mimic a clock (and the Matlab functions for timing code) I will assign the variable `tic` to store the initial time and the variable `toc` to store the final time.


```python
tic = time.perf_counter(); # Get initial time

# Start code
F = 82 # Temperature in [F]
C = (F - 32) * 5/9
print('Temperature outside is',round(C,1),'Celsius')
# end code

toc = time.perf_counter(); # Get final time

print('Elapsed time:', toc - tic)
```

    Temperature outside is 27.8 Celsius
    Elapsed time: 0.0004749069998979394


## Format time strings

Dates and time can be represented in a wide variety of formats across the world. There are also conventions to convert local time in one place to local time in a different location. To handle all these possibilities, the Python time module has a `strftime()` method to print the same information in multiple formats. The format largely depends on the application. For instance, if we are interested in a process that changes on daily time steps, then we can probably omit hours, minutes, seconds, and milliseconds from the output, so that it is legible and clear to the user or reader of that date. Below I present few examples to show you the basic syntax. Feel free to try any of the following formats.


**%a**    Locale’s abbreviated weekday name

**%d**    Day of the month as a decimal number [01,31]

**%b**    Locale’s abbreviated month name

**%Y**    Year with century as a decimal number

**%H**    Hour (24-hour clock) as a decimal number [00,23]

**%M**    Minute as a decimal number [00,59]

**%S**    Second as a decimal number [00,61]




```python
time.strftime("%a, %d %b %Y", time.localtime())
```




    'Thu, 30 Jan 2020'




```python
# UTC (Coordinated Universal Time) or GMT (Greenwich Meridian Time)
time.strftime("%a, %d %b %Y %H:%M:%S +0000", time.gmtime())
```




    'Thu, 30 Jan 2020 16:58:06 +0000'




```python
# Formatted local time
time.strftime("%a, %d %b %Y %H:%M:%S +0000", time.localtime())
```




    'Thu, 30 Jan 2020 10:58:07 +0000'



## References

Official Python docs for the time module: <https://docs.python.org/3/library/time.html>

