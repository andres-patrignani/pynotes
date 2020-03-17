# Day of the Year

The day of the year (commonly abbreviate as DOY) is a useful variable to predict phenomena tightly related to the recurrent Earth orbital cycles around the sun. Many meteorological and physiological process are related to solar radiation and/or temperature patterns. The day of the year provides a convenient date-agnostic framework to predict these patterns without much additional data.

The key idea here is to realize that the day of the year is defined relative to January 1. So this will be our baseline for any year.



```python
import datetime
import math

```


```python
# Define a specific date
input_date = datetime.datetime(2020,2,27) # Unmute this line to compute DOY for any date

```

>When working with dates and time, using total seconds sometimes can make operations way easier. Remember that there are 3600 seconds in one hour and 86400 seconds in one day.


```python
#current_date = datetime.datetime.now()
date_one = datetime.datetime(input_date.year,1,1)
date_diff = current_date - date_one; # Operations between datetime objects result in datetime objects

doy_float = date_diff.total_seconds()/86400 + 1 # gives the doy with decimal places
doy_int = math.floor(doy_float) # found that total_seconds is a method from the error of dateDiff.total_seconds

print('Day of the year: ' + str(doy_int))

```

    Day of the year: 58


Note that adding a unit in doy_float makes sense if you just want to know the DOY in integer format. In its current state, the code will return DOY=1 for 1-Jan, which is what we expect. If you also want to return the fraction of DOY, then you need to realize that 0.5 would 1-Jan at noon. However, most applications demanding the DOY will assume that 1-Jan is day 1 and not day 0. If this is what you need, then directly working in seconds as a proxy for a serial dates number might be the way to go.



```python
# Alternative using built-in methods
day_of_year = input_date.timetuple().tm_yday
print(day_of_year)

```

    58

