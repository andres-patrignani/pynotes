
# **Photoperiod**

Approximate daylength (photoperiod) based on location and day of the year using numpy module.

## References: 

- Keisling, T.C., 1982. Calculation of the Length of Day 1. Agronomy Journal, 74(4), pp.758-759.

- Further information can be found in Campbell and Norman, Introduction to environmental biphysics p. 168-171.
    


```python
import numpy as np
```


```python
phi = 33.4;  # Latitude for consistency with notation in literature.
doy = 201; # Day of the year. Julian calendar. Day from January 1.
light_intensity = 2.206 * 10**-3

C = np.sin(np.radians(23.44)) # sin of the obliquity of 23.44 degrees.
B = -4.76 - 1.03 * np.log(light_intensity) # Eq. [5]. Angle of the sun below the horizon. Civil twilight is -4.76 degrees.

# Calculations
alpha = 90 + B # Eq. [6]. Value at sunrise and sunset.
M = 0.9856*doy - 3.251 # Eq. [4].
lmd = M + 1.916*np.sin(np.radians(M)) + 0.020*np.sin(np.radians(2*M)) + 282.565 # Eq. [3]. Lambda
delta = np.degrees( np.arcsin(C*np.sin(np.radians(lmd))) ) # Eq. [2].

# Defining sec(x) = 1/cos(x)
P = 2/15 * np.degrees( np.arccos( np.cos(np.radians(alpha)) * (1/np.cos(np.radians(phi))) * (1/np.cos(np.radians(delta))) - np.tan(np.radians(phi)) * np.tan(np.radians(delta)) ) ) # Eq. [1].

print('Day of the year: ' + str(doy))
print('Lat: ' + str(phi) + ' degrees')
print('Photoperiod: ' + str(round(P,2)) + ' hours/day')


```

    Day of the year: 201
    Lat: 33.4 degrees
    Photoperiod: 14.2 hours/day

