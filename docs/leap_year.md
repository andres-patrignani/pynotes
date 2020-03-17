## Find leap years

Every year that is exactly divisible by four is a leap year, except for years that are exactly divisible by 100, but these centurial years are leap years if they are exactly divisible by 400. For example, the years 1700, 1800, and 1900 are not leap years, but the years 1600 and 2000 are (United States Naval Observatory).



```python
# Leap years
year = 2000

if year%4 == 0:
    if year%100 > 0:
        print(year,'is a leap year.')
    else:
        if year%400 == 0:
            print(year,'is a leap year.')
        else:
            print(year,'is not a leap year.')
else:
    print(year,'is not a leap year.')

```

    2000 is a leap year.


## References

Wikipedia: https://www.wikiwand.com/en/Leap_year
