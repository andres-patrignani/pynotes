
# Day of the Year

Script that calculates the day of the calendar year for the current date. The script outputs the DOY as an integer.


```python
import datetime
import math


current_date = datetime.datetime.now()
# current_date = datetime.datetime(2019,1,1,12) # Unmute this line to compute DOY for any date
date_one = datetime.datetime(current_date.year,1,1)
date_diff = current_date - date_one; # Operations between datetime objects result in datetime objects

doy_float = date_diff.total_seconds()/86400 # gives the doy with decimal places
doy_int = math.floor(doy_float) # found that total_seconds is a method from the error of dateDiff.total_seconds

print(doy_float)
print('Day of the year: ' + str(doy_int))

```

    107.50141303420138
    Day of the year: 107


## Important points

- The key idea here was to realize that Day of the Year is defined using 1-jan as the baseline.
- Avoid hard coding variables.
- When working with date and time, using total seconds can make some operations way easier

> **Note** that adding a unit in `doy_float` makes sense if you just want to know the DOY in integer format. In its current state, the code will return `DOY=1` for `1-jan`, which is what we expect. If you also want to return the fraction of DOY, then you need to realize that 0.5 would 1-jan at noon. However, most applications demanding the DOY will assume that 1-jan is day 1 and not day 0. If this is waht you need, then directly working in seconds as a proxy for a serial data number might be the way to go.
