# Soil Mass-Volume Relationships

**Problem 1**

A cylindrical soil sample with an inner diameter of 5.00 cm and height of 5.00 cm has a wet mass of 180.0 g. After oven-drying the soil sample at 105 degrees Celsius for 48 hours the sample has a dry mass of 147.0 g. Based on the provided information, calculate: porosity, bulk density, gravimetric water content, volumetric water content, and relative saturation, and soil water storage expressed in mm and inches of water. Assume that water has a density of 0.998 g cm$^{-3}$.



```python
# Import modules
import math

```


```python
# Problem information
ring_diameter = 5.00 # cm
ring_height = 5.00 # cm
wet_mass = 180.0 # grams
dry_mass = 147.0 # grams
density_water = 0.998 # g/cm^3 at 20 Celsius

```


```python
# Sample volume
volume = math.pi * (ring_diameter/2)**2 * ring_height
print('Volume of sample is:', round(volume), 'cm^3')

```

    Volume of sample is: 98 cm^3



```python
# Bulk density
bulk_density = dry_mass/volume
print('Bulk density of the sample is:', round(bulk_density,2), 'g/cm^3')

```

    Bulk density of the sample is: 1.5 g/cm^3



```python
# Porosity
particle_density = 2.65 # g/cm^3
f = 1 - bulk_density/particle_density 
print('Porosity of the sample is:', round(f,2))

```

    Porosity of the sample is: 0.43



```python
# Gravimetric soil mositure 
# Mass of water per unit mass of dry soil. Typically in g/g or kg/kg
theta_g = (wet_mass - dry_mass) / dry_mass
print('Gravimetric water content is:', round(theta_g,3), 'g/g')

```

    Gravimetric water content is: 0.224 g/g



```python
# Volumetric soil mositure
# Volume of water per unit volume of dry soil. Typically in cm^3/cm^3 or m^3/m^3
density_water
theta_v = theta_g * bulk_density/density_water
print('Volumetric water content is:', round(theta_v,3), 'cm^3/cm^3')

```

    Volumetric water content is: 0.337 cm^3/cm^3



```python
# Relative saturation
rel_sat = theta_v/f
print('Relative saturation is:', round(rel_sat,2))

```

    Relative saturation is: 0.77



```python
# Storage
storage_mm = theta_v * ring_height*10
storage_in = storage_mm/25.4
print('The soil water storage in mm is:', round(storage_mm,1), 'mm')
print('The soil water storage in inches is:', round(storage_in,3), 'inches')

```

    The soil water storage in mm is: 16.8 mm
    The soil water storage in inches is: 0.663 inches


**Problem 2**

How many liters of water are stored in the top 1 meter of the soil profile of a field that has an area of 64 hectares (about 160 acres)? Assume the soil moisture of the field is the volumetric water content computer in the previous problem.


```python
# Liters of water in a field
field_area = 64*10000 # m^2
profile_length = 1 # meters
equivalent_height = profile_length * theta_v # m of water
volume_of_water = field_area * equivalent_height # m^3 of water
# Use; 1 m^3 = 1,000 liters
liters_of_water = volume_of_water * 1000

print('There are ', round(liters_of_water), 'L of water')
print('This is equivalent to',round(liters_of_water/(50*25*3)), 'olympic swimming pools')

```

    There are  215557669 L of water
    This is equivalent to 57482 olympic swimming pools


**Problem 3**
The composition of the soil in the top 1 meter of the field in the previous problem is 20% sand, 60% silt, and 20% clay by mass. Compute the amount of sand required to change the composition to 21% sand, 59% silt, and 20% clay. Assume that the bulk density of the field is the value computed in problem 1.



```python
field_volume = field_area * 1 # m^3
field_mass = field_volume * bulk_density # in Mg/m^3 since g/cm^3 = Mg/m^3

print('Replacing a 1% soil texture would require', round(field_mass/100), 'Mg (or metric tons)')

```

    Replacing a 1% soil texture would require 9583 Mg (or metric tons)

