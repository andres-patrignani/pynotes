## Sedimentation time

The first example is about computing the time that it takes for a particle of a given diameter to settle down in a lake of a given depth using Stoke's law. Stoke's law assumes:

- Laminar flow (non-turbulent)
- Particles do not interact with each other
- Particles are round
- Particles settle down at constant velocity (no acceleration, this the terminal velocity). This is the point where drag force and buoyant force are equal to the gravitational force.

The terminal velocity $u$ according to Stoke's law is given by:

$$ u = \frac{d^2 \; g \; (\rho_s - \rho_f)}{18 \; \eta}$$


```python
import datetime

# Display some tentative values
print('Typical particle diameters: sand=0.5 mm, silt=0.05 mm, and clay=0.0005 mm')

# Request particle size
diameter = input('Enter particle diameter (mm):')
diameter = float(diameter) / 1000 # Convert to meters

# Request depth of lake
print('Large lake=500 m, small lake=15 to 20 m')
lake_depth = input('Lake depth (m):')
lake_depth = float(lake_depth)

# Compute terminal velocity
g = 9.81                   # m/s^2
density_solid = 2650       # kg/m^3
density_fluid = 1000       # kg/m^3
viscosity_fluid = 0.001    # kg/(m s)

terminal_velocity = diameter**2 * g * (density_solid-density_fluid)/(18*viscosity_fluid) # m/s
sedimentation_seconds = round(lake_depth/terminal_velocity) # seconds

# Convert to datetime.timedelta object
sedimentation_time = datetime.timedelta(seconds=sedimentation_seconds)

# Display result
print('The sedimentation time is {time}'.format(time=sedimentation_time))

```

    Typical particle diameters: sand=0.5 mm, silt=0.05 mm, and clay=0.0005 mm


    Enter particle diameter (mm): 0.01


    Large lake=500 m, small lake=15 to 20 m


    Lake depth (m): 20


    The sedimentation time is 2 days, 13:46:48

