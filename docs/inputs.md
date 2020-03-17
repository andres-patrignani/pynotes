# Inputs

A rather useful feature of Python is its ability to interact with the user to gather information and trigger specific actions. In research this feature can be useful to automate lab routines, reduce data entry errors, and even used for learning and teaching purposes.


## Unit conversion


```python
F = input("Insert temperature in degrees Fahrenheit:")
F = float(F)
print( (F-32)*5/9 )

```

    Insert temperature in degrees Fahrenheit: 212


    100.0


## Volume of rainfall


```python
rainfall = input("Rainfall in mm")
rainfall = float(rainfall)/10 # convert to float and then to cm
area = 100 * 100 # area in cm^2
volume = area*rainfall # volume in cm^3
liters = volume / 1000 # 1 liter is 1000 cm^3

print(round(liters),'liters per m^2')
print(round(liters*10**6),'liters per km^2')
print(round(liters*4046.86/3.785),'gallons per acre')

```

    Rainfall in mm 1


    1 liters per m^2
    1000000 liters per km^2
    1069 gallons per acre


## Hydrometer Readings

The hydrometer method is one of the most popular methods for soil particle size analysis. The method consists of dispersion a known amount of ground (<2 mm sieve) and dry soil in a solution of de-ionized water with sodium hexametaphosphate (dispersing agent). 

Readings with a hydrometer are made at specific times according to the particle diameter and corresponding sedimentation time in the cylinder calculated based on Stoke's law. To build a detailed curve of particle sizes, hydrometer readings need to be made more frequently. 

A blank treatment only containing de-ionized water and sodium hexametaphosphate is required to remove the density effect of the dispersing agent. THe blank reading is substracted from each reading before proceding with the computations.

In this example we will use a fixed input mass of oven-dried soil of 40 grams.

Learn more about the [hydrometer method](https://www.wikiwand.com/en/Soil_texture)



```python
# Sample location
sample_name = input("Sample name:")

# Blank reading
blank_reading = float(input("Blank reading (g/L):"))

# First reading
first_reading = float(input("First reading (g/L):"))

# Second reading
second_reading = float(input("Second reading (g/L):"))

# Calculate percent of sand, silt, and clay
# For each soil reading we need to subtract the blank reading
dry_soil = 40 # grams
sand_content = round((dry_soil - first_reading)/dry_soil*100)
clay_content = round(second_reading/dry_soil*100)
silt_content = 100 - sand_content - clay_content

# Print results to user
print(sample_name)
print('Sand: {sand}%, Silt: {silt}%, Clay: {clay}%'
      .format(sand= sand_content, 
              silt= silt_content, 
              clay= clay_content))
```

    Sample name: Manhattan
    Blank reading (g/L): 5
    First reading (g/L): 34
    Second reading (g/L): 24


    Manhattan
    Sand: 15%, Silt: 25%, Clay: 60%

