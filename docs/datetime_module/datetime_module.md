
# Datetime module

Official docs: https://docs.python.org/3/library/datetime.html 

Three ways to express time:
- With an int ("timestamp")
- With a string ("formatted date/time")
- With a specific structure, either in C or in Python


```python
import datetime
```


```python
# Get current date
currentDate = datetime.datetime.now()
print(currentDate)
```

    2019-02-07 11:00:58.866927



```python
# Specific component of the date (e.g. day, hour, day of the week, etc.)
print(currentDate.day)
print(currentDate.date())
print(currentDate.month)
print(currentDate.minute)
```

    7
    2019-02-07
    2
    28



```python
# Format dates
print(currentDate.strftime("%b"))
print(currentDate.strftime("%B"))
print(currentDate.strftime("%y"))
print(currentDate.strftime("%Y"))
print(currentDate.strftime("%d"))
print(currentDate.strftime("%c"))
print(currentDate.strftime("%x"))

# For more formats go to: https://www.w3schools.com/python/python_datetime.asp

```

    Feb
    February
    19
    2019
    07
    Thu Feb  7 10:28:58 2019
    02/07/19



```python
# Build a date
# datetime.datetime(year, month, day, hour, minute, second, microseconds)
print(datetime.datetime(2018, 6, 1, 8, 0, 0, 5))
```


```python
# Arthmetic operations with dates

date_start =datetime.datetime(2018, 6, 1)
date_end = datetime.datetime.now()

date_diff = date_end - date_start;
 
print(type(date_start))  # Class datetime
print(type(date_diff))   # Class timedelta

# It's usually nice to work in seconds 
print(date_diff.total_seconds()) # Only for timedelta datatype 
print(math.floor(date_diff.total_seconds())) # Only for timedelta datatype 
```

    <class 'datetime.datetime'>
    <class 'datetime.timedelta'>
    21725908.340705
    21725908

