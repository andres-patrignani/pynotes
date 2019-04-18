
# Time module
The time module is typically used when we need to time code, otherwise the datetime module might be a better choice.


```python
import time
```


```python
# Get detailed local time and date

clk = time.localtime()
print(clk)
type(clk)
```

    time.struct_time(tm_year=2019, tm_mon=1, tm_mday=30, tm_hour=13, tm_min=44, tm_sec=11, tm_wday=2, tm_yday=30, tm_isdst=0)





    time.struct_time




```python
# The statement above generates a time structure (a container with labeled sub-containers)
# This means that we can access the sub-containers by calling their name.

print(clk.tm_year)
print(clk.tm_hour)
print(clk.tm_sec)
```

## Python is synchronous

The Python interpreter executes one line at the time and will only proceed to the following line once the current line has finished. This means that if you have a line that takes time to execute, the entire code will be on hold. The up side of this behavior is that is easy to write code using logical steps.


```python
# An important concept is that Python is synchronous, which means that the interpreter executes 
# lines from top to bottom and that it will wait until the current line has finished in order to 
# proceed with the following line. This seems obvious, but not all programming languages
# behave this way. We can see this if we print the steps or if we apply a delay. 
# Let's examine both:

# Print steps
print('Executing step 1')
print('Executing step 2')
print('Executing step 3')
print('See, they were printed sequentially!')
```


```python
# Applying a time delay. In this case, the time delay mimics a long computation
print('Executing step 1')
time.sleep(3)  # in seconds
print('Executing step 2')

# As you can see the "step 2" command did not execute right away. 
# An advantage of this behavior is that is easy to write code and organize lines in logical order.
# A disadvantage is that if one line of code requires long processing times, the entire code will 
# be delayed. In order words, a single line could block the entire code.
```

## Evaluate code performance

Use the time module to check and document the performance of your script
The `perf_counter()` function brings up the clock with the highest resolution.
The reference point of the returned value is undefined, so that only the difference between the results of consecutive calls is meaningful. Source: https://docs.python.org/3/library/time.html

`tic` will store the initial time and `toc` will store the final time.


```python

tic = time.perf_counter(); # Get initial time

# Start code
temp_f = 82 # Temperature in [F]
temp_c = (temp - 32) * 5/9
print('Temperature outside is',round(temp_c,1),'Celsius')
# end code

toc = time.perf_counter();

print('Elapsed time:', toc-tic)
```

    Temperature outside is 27.8 Celsius
    Elapsed time: 0.0002739239999982601


## Format of time strings

**%a**    Locale’s abbreviated weekday name

**%d**    Day of the month as a decimal number [01,31]

**%b**    Locale’s abbreviated month name

**%Y**    Year with century as a decimal number

**%H**    Hour (24-hour clock) as a decimal number [00,23]

**%M**    Minute as a decimal number [00,59]

**%S**    Second as a decimal number [00,61]

For more options visit the official Python docs at: <https://docs.python.org/3/library/time.html>


```python
# UTC (Coordinated Universal Time) or GMT (Greenwich Meridian Time)
time.strftime("%a, %d %b %Y %H:%M:%S +0000", time.gmtime())
```




    'Fri, 02 Nov 2018 15:14:38 +0000'




```python
# Formatted local time
time.strftime("%a, %d %b %Y %H:%M:%S +0000", time.localtime())
```




    'Fri, 02 Nov 2018 10:17:13 +0000'


