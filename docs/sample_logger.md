## Sample Entry System for Mass-Volume Properties

One of the biggest challenges of laboratories that deal with samples of some sort (e.g. DNA, tissue, soil, seeds, etc.) on a routine basis is metadata entry, storage, and accessibility. Assigning a sample unique identification number and adding sample metadata is essential for tracking samples around the lab and generating customer reports or doing data mining for research. 

In this example we will create a simple data entry and storage for soil samples. We will assign a unique ID to each new sample as well as sample metadata (e.g. responsible, location, soil attributes, etc.).

To allow an unlimited entry of samples by lab personnel we will couple the `input()` function with a `while` loop.

In order to create a more serious data entry system we need to allow users to modify their inputs and to check whether inputs are plausible or not. For instance, if by chance a user enters a negative ring volume, then we need to catch this and let the user correct the mistake. While I would not use the code below for any serious application, I still hope that this simple example will inspire you to do something simple and useful for your own lab or research experiment.


```python
# Universally Unique IDentifier module
import uuid 
import pprint

new_entry_flag = True
D = dict()

while new_entry_flag:
        
    # Generate sample unique ID
    sample_id = str(uuid.uuid1()) # to make the UUID a string

    # Request responsible
    responsible = input("Name of responsible person ('John Smith'):")

    # Request sample location
    latitude = input("Latitude(e.g. '35.6', North is positive):")
    longitude = input("Longitude (e.g. '-97.8', W is negative):")
    location = input("Nearest city ('Manhattan, KS'):")
    city, state = location.split(",")

    # Request ring number
    ring_number = input("Ring number:")
    ring_number = int(ring_number)

    # Request ring volume
    ring_volume = input("Ring volume (in cm^3):")
    ring_volume = float(ring_volume)

    # Request mass of empty ring 
    mass_empty_ring = input("Mass of empty ring with no plastic lids (g):")
    mass_empty_ring = float(mass_empty_ring)

    # Request mass of wet soil
    mass_wet_soil = input("Mass of wet soil including ring (g):")
    mass_wet_soil = float(mass_wet_soil)

    # Request mass of oven-dry soil
    mass_oven_dry_soil = input("Mass of oven-dried soil with ring (g):")
    mass_oven_dry_soil = float(mass_oven_dry_soil)

    # Compute mass-volume relationships
    particle_density = 2.65 # g/cm^3 - Assumed value for quartz.
    bulk_density = (mass_oven_dry_soil - mass_empty_ring)/ring_volume # g/cm^3
    porosity = 1 - bulk_density/particle_density # unitless
    mass_water = mass_wet_soil - mass_oven_dry_soil # g
    gravimetric_water_content = mass_water/(mass_oven_dry_soil - mass_empty_ring) # g/g
    volumetric_water_content = gravimetric_water_content * bulk_density/0.998 # g/cm^3

    # Initialize dictionary for storing samples
    D[sample_id] = {'responsible': responsible,
                    'lat': latitude,
                    'lon': longitude,
                    'city': city,
                    'state': state,
                    'ring_number': ring_number,
                    'bulk_density': bulk_density,
                    'porosity': round(porosity,3),
                    'volumetric_water': round(volumetric_water_content,3)}
    
    # Ask user if they want to exit
    new_entry_msg = input("Do you want to add a new sample? (y/n)").lower() # Force to be lower case
    if new_entry_msg == 'n' or new_entry_msg == 'no':
        new_entry_flag = False
    
```

    Name of responsible person ('John Smith'): John Doe
    Latitude(e.g. '35.6', North is positive): 33.5
    Longitude (e.g. '-97.8', W is negative): -97.8
    Nearest city ('Manhattan, KS'): Wichita Falls, TX
    Ring number: 1
    Ring volume (in cm^3): 100
    Mass of empty ring with no plastic lids (g): 12
    Mass of wet soil including ring (g): 185
    Mass of oven-dried soil with ring (g): 145
    Do you want to add a new sample? (y/n) y
    Name of responsible person ('John Smith'): Andres Patrignani
    Latitude(e.g. '35.6', North is positive): 38.830539
    Longitude (e.g. '-97.8', W is negative): -97.535717
    Nearest city ('Manhattan, KS'): Salina, KS
    Ring number: 2
    Ring volume (in cm^3): 100
    Mass of empty ring with no plastic lids (g): 12
    Mass of wet soil including ring (g): 199
    Mass of oven-dried soil with ring (g): 141
    Do you want to add a new sample? (y/n) n



```python
# Display entries (maybe not a good idea if you have hundreds or thousands of entries)
pprint.pprint(D)

```

    {'309ab81e-4ce4-11ea-b6eb-f45c89ca92fb': {'bulk_density': 1.29,
                                              'city': 'Salina',
                                              'lat': '38.830539',
                                              'lon': '-97.535717',
                                              'porosity': 0.513,
                                              'responsible': 'Andres Patrignani',
                                              'ring_number': 2,
                                              'state': ' KS',
                                              'volumetric_water': 0.581},
     'fce49940-4ce3-11ea-b6eb-f45c89ca92fb': {'bulk_density': 1.33,
                                              'city': 'Wichita Falls',
                                              'lat': '33.5',
                                              'lon': '-97.8',
                                              'porosity': 0.498,
                                              'responsible': 'John Doe',
                                              'ring_number': 1,
                                              'state': ' TX',
                                              'volumetric_water': 0.401}}



```python
# Get all dictionary keys as a list
all_samples = [*D]
print(all_samples)
```

    ['fce49940-4ce3-11ea-b6eb-f45c89ca92fb', '309ab81e-4ce4-11ea-b6eb-f45c89ca92fb']

