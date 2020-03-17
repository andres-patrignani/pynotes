# Photoperiod Using Math Module

Approximate daylength (photoperiod) based on location and day of the year using numpy module.

Alternative to the function code using Numpy.


```python
# Quick shortcuts for prototyping
# phi = 33.4
# doy = 201; # Day of the year. 19-jul-2020 is DOY 201.

# Request latitude from user
# Latitude was named phi for consistency with notation in literature.
phi = input("Enter latitude in decimal degrees (S is negative):") # Decimal degrees
phi = float(phi)

# Request date from user
date_str = input("Enter date (dd-mmm-yyyy):")
date_num = datetime.datetime.strptime(date_str,"%d-%b-%Y")
doy = date_num.timetuple().tm_yday


# Compute angle of the sun below the horizon. 
# For reference civil twilight is -4.76 degrees.
light_intensity = 2.206 * 10**-3
B = -4.76 - 1.03 * math.log(light_intensity) # Eq. [5].

# COmpute aalue at sunrise and sunset.
alpha = 90 + B # Eq. [6]. 

M = 0.9856*doy - 3.251 # Eq. [4].

lmd = M + 1.916*math.sin(math.radians(M)) + 0.020*math.sin(math.radians(2*M)) + 282.565 # Eq. [3]. Lambda variable

# sin of the obliquity of 23.44 degrees.
C = math.sin(math.radians(23.44))

delta = math.degrees( math.asin(C*math.sin(math.radians(lmd))) ) # Eq. [2].

# Compute photoperiod
# Defining sec(x) = 1/cos(x)
P = 2/15 * math.degrees( math.acos( math.cos(math.radians(alpha)) * (1/math.cos(math.radians(phi))) * (1/math.cos(math.radians(delta))) - math.tan(math.radians(phi)) * math.tan(math.radians(delta)) ) ) # Eq. [1].

print('Photoperiod: ' + str(round(P,2)) + ' hours/day')

```
