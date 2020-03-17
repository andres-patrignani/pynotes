# Datetime Module

The `datetime` module is useful to handle arithmetic operations that involve timestamps. With this module we can extract the day, month, year, and even milliseconds from a datetime object and then do some sort of computation with it. The `datetime` module also allows for changing the string format without affecting the timestamp infomration.


```python
import datetime
```


```python
# Get current date
currentDate = datetime.datetime.now()
print(currentDate)
print(type(currentDate))
```

    2020-02-03 13:58:41.266103
    <class 'datetime.datetime'>



```python
# Specific component of the date (e.g. day, hour, day of the week, etc.)
print(currentDate.date())
print(currentDate.time())
print(currentDate.day)
print(currentDate.month)
print(currentDate.minute)
```

    2020-02-03
    13:53:49.937538
    3
    2
    53


## Output formats


```python
# Example month format
print(currentDate.strftime("%b"))
print(currentDate.strftime("%B"))

# Example year format
print(currentDate.strftime("%y"))
print(currentDate.strftime("%Y"))

# Example day format
print(currentDate.strftime("%d"))

# Example full date format
print(currentDate.strftime("%c"))
print(currentDate.strftime("%x"))

```

    Jan
    January
    20
    2020
    26
    Sun Jan 26 20:57:59 2020
    01/26/20


## Parse a string into a date


```python
date_str = "2020-02-03 14:30:00"
print(type(date_str))

```

    <class 'str'>



```python
date_str = datetime.datetime.strptime(date_str, "%Y-%d-%m %H:%M:%S") 
print(date_str)
print(type(date_str))
print(date_str.weekday())  # Monday is 0 and Sunday is 6
```

    2020-03-02 14:30:00
    <class 'datetime.datetime'>
    0


## Build a date

To create a datetime we use the `datetime.datetime(year, month, day, hour, minute, second, microseconds)` method. It is also possible to generate a new datetime object by only passing a limited number of inputs, such as `datetime.datetime(year, month, day)`.


```python
print(datetime.datetime(2018, 6, 1, 8, 0, 0, 5))
print(datetime.datetime(2018, 6, 1))
```

    2018-06-01 08:00:00.000005
    2018-06-01 00:00:00


## Arthmetic operations with dates

Operations with dates are easy using this module. In fact, since handling dates can sometimes be tricky, it is highly recommended to use a module to handle dates to avoid introducing errors.


```python
date_start = datetime.datetime(2018, 6, 1)
date_end = datetime.datetime.now()
date_diff = date_end - date_start;
print(date_diff)
```

    612 days, 14:06:52.878049



```python
print(type(date_start))  # Class datetime
print(type(date_diff))   # Class timedelta
```

    <class 'datetime.datetime'>
    <class 'datetime.timedelta'>


# Working with seconds

Converting any date and time into seconds also enables the use seconds as a sort of digital number that we can employ to carry operations in a single unit of time. For instance, once we know the difference between two dates in terms of seconds we can integrate this information with the `math` module.


```python
print(date_diff.total_seconds()) # Only for timedelta datatype
```

    52927612.878049



```python
secs_per_day = 60 * 60 * 24 # 86400 seconds in one day
print(date_diff.total_seconds()/secs_per_day)
```

    612.588112014456



```python
import math
print(math.floor(date_diff.total_seconds())) # Only for timedelta datatype
```

    52927612


## References

Official docs: https://docs.python.org/3/library/datetime.html

Data formats: https://docs.python.org/3/library/time.html
