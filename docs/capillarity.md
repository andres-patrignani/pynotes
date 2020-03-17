# Capillarity

The phenomenom of capillarity describes the ability of a fluid to increase or decrease its height within a narrow capillary tube. Capillarity is the result of cohesive and adhesive forces at the molecular level. The resulting capillary rise or depression is described by the Young-Laplace equation. 

$$ h = \frac{2 \ \gamma \ cos(\alpha)}{\rho \ g \ r}$$

where,

$h$ is the capillary rise (positive values) or depression (negative values)

$\gamma$ is the surface tension of the liquid-air interface in $mN/m$

$\alpha$ is the contact angle of the solid-liquid interface in degrees

$\rho$ is the desnity of the fluid in $kg/m^3$

$g$ is the acceleration due to gravity in $m/s^2$

$r$ is the radius of the capillary tube in meters

>To simplify the input of the radius into the function we will pass the radius in micrometers and we will convert it into meters within the function.

This equation is often used in soil science to approximate the height of capillary rise (e.g. from a water table) or to find the average pore radius (when solve for *r* instead of *h*.

In this example we will assign the properties of water as the default values since this is probably the most common context for the application of the Young-Laplace equation.



```python
import math

```


```python
def capillary(radius,contact_angle=0,surface_tension=0.073,density=1000):
    """
    Function that approximates the height of capillary rise/depression
    given the radius of the capillary tube (or average soil prore radius)
    based on the Young-Laplace equation.
    
    Input variables:
    radius: radius of the capillary tube (or mean pore radius) in micrometers
    contact_angle: contact angle of the liquid-gas interface on glass. Default 0 degrees
    surface_tension: surface tension. Default 0.073 N/m for water at 20 Celsius 
    density: Density of the fluid in kg/m^3. Default 1000 kg/m^3 for water
    
    Output variables
    height: height of the resulting water column in meters
    """
    
    # Define constants
    g = 9.81 # Acceleration due to gravity in m/s^2

    # Change units of input radius to match all other units in meters
    radius = radius/10**6  # convert from micrometers to meters
    
    # Convert contact angle into radians (required by the math module)
    contact_angle = math.radians(contact_angle) 
    
    # Compute capillary rise using Young-Laplace equation
    numerator = 2*surface_tension*math.cos(contact_angle)
    denominator = density*g*radius
    height = numerator/denominator # height in meters
    
    # Convert height into cm for more intuitive output
    height = round(height*100,2) # height in centimeters

    return height

```


```python
# Define capillary radius for both water and mercury
radius = 28 # micrometers

```


```python
# Properties for water
contact_angle = 20 # degrees for water on soil

# Compute resulting height for water
h_water = capillary(radius,contact_angle)

print(round(h_water,2), 'cm')

```

    49.95 cm



```python
# Properties for mercury
contact_angle = 138
surface_tension = 486.5 / 1000  # mN/m to N/m at 20 Celsius
density = 13593 # kg/m^3

# Compute resulting height for mercury
h_mercury = capillary(radius,contact_angle,surface_tension,density)

print(round(h_mercury,2), 'cm')

```

    -19.37 cm


## References

Table of surface tension values for different fluids: https://www.wikiwand.com/en/Surface-tension_values

