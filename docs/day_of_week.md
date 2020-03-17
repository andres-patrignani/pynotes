# Day of the week

Determination of the day of the week for any day based on Lewis Carroll approach. This code was generated more as a personal challenge or puzzle while learning Python. The code is partitioned into a rather long series of steps to follow along the example in the textbook describing the method with an example.



```python
import datetime
import math


def dozens(value):
    dozen = math.floor(value/12)
    fraction = value - dozen*12
    return dozen,fraction


def digitsum(number):
    counter_digits = 0
    counter_fours = 0
    for digit in str(number):
        counter_digits += int(digit) 
        if digit == 4:
            counter_fours += 1
    return counter_digits, counter_fours


def roundseven(number):
    if number >= 7:
        number = number % 7
    else:
        number
    return number


def monthitem(month):
    D = {"months": ["January","February","March","April","May","June","July","August","September","October","November","December"],
        "item": [0,3,3,6,1,4,6,2,5,0,3,5]}
    for i in range(0,len(D["months"])):
        if month == D["months"][i]:
            return D["item"][i]
        

def adjustleap(year,month,partial_sum):
    if year % 4 == 0 and (month == 'January' or month == "February"):
        if partial_sum == 0:
            partial_sum += 7
        partial_sum -= 1
    return partial_sum


def dayofweek(partial_sum):
    week_days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]
    for i in range(0,len(week_days)):
        if partial_sum == i:
            return week_days[i]
        
        
```


```python
# Target date (yyyy,mm,dd)
#date = datetime.datetime(1676,2,23) # Original date in the book
date = datetime.datetime(2020,2,11) # Martin Luther King Jr. day
print(date)
```

    2020-02-11 00:00:00



```python
# Century item
century = math.floor(date.year/100)
century_item = (3 - century % 4)*2
century_item = roundseven(century_item)
century_item

```




    6




```python
(century % 4)
```




    0




```python
# Year item
year = math.floor((date.year-century*100))

dozen,fraction = dozens(year)
sum_fraction, fours = digitsum(fraction)

year_item = dozen + fraction + sum_fraction + fours
year_item = roundseven(year_item)
year_item

```




    3




```python
# Partial sum
partial_sum = century_item + year_item
partial_sum = roundseven(partial_sum)
partial_sum
```




    2




```python
# Month item
month = date.strftime("%B")
month_item = monthitem(month)
month_item

```




    3




```python
# Partial sum
partial_sum = roundseven(partial_sum + month_item)
partial_sum

```




    5




```python
# Day item
day_item = roundseven(date.day)
day_item
    
```




    4




```python
# Partial sum
partial_sum = roundseven(partial_sum + day_item)
partial_sum

```




    2




```python
# Adjust for leap years
adjustleap(date.year,month,partial_sum)

```




    1




```python
# Print the day of the week
dayofweek(partial_sum)

```




    'Tuesday'



## References
- Lewis Carroll in Numberland, p. 167
- <https://www.reddit.com/r/learnmath/comments/2jjl4c/lewis_carrolls_day_of_the_week_algorithm/>
