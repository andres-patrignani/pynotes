
# Soil temperature model

We will implement an analytical solution of the heat conduction-diffusion equation. The model is simple and accounts for soil temperature in time and soil depth.

## Assumptions

- Constant soil thermal diffusivity

- Uniform soil texture

- Temperature in deep layers approximate the mean annual air temperature

- Soil surface temperature is equal to the air temperature


```python
# Import modules
import numpy as np
import math
from bokeh.plotting import figure, show, output_notebook, ColumnDataSource
from bokeh.layouts import row
from bokeh.models import HoverTool
output_notebook()

```



    <div class="bk-root">
        <a href="https://bokeh.pydata.org" target="_blank" class="bk-logo bk-logo-small bk-logo-notebook"></a>
        <span id="40bae85c-4cc5-4513-9fee-a072d5dced36">Loading BokehJS ...</span>
    </div>





```python
### Temperature as a function of time for a given soil depth ###

# Constants
T_avg = 25 # Average temperature at the soil surface
A0 = 10 # Temperature amplitude at the soil surface
D_T = 0.5 * 10**(-6)# Thermal diffusivity [m^2/s]

par = [T_avg, A0, D_T]

def soil_temperature(t,z,par):
    phi = 50000 # phase constant
    T_avg = par[0]
    A0 = par[1]
    D_T = par[2]
    period = 60*60*24
    omega = 2*math.pi/period
    d = (2* D_T/omega)**(1/2) # Length units
    soil_T = T_avg + A0 * math.exp(-z/d) * math.sin(omega*t + phi - z/d)
    return soil_T, d

npoints = 1000
z  = 0 # soil depth in meters
t = 12*3600 # time in hours
z_max = 1 # soil depth in meters
t_vec = np.linspace(0,2*86400,npoints) # 0 cm = surface; 200 cm = lower boundary
z_vec = np.linspace(0,z_max,npoints) # 0 cm = surface; 200 cm = lower boundary

T_time = []
T_depth = []
for i in range(npoints):
    Ti,di = soil_temperature(t_vec[i],z,par)
    Tj,dj = soil_temperature(t,z_vec[i],par)
    T_time.append(Ti)
    T_depth.append(Tj)

# Set data for p1
source_p1 = ColumnDataSource(data=dict(x=t_vec/3600, y=T_time))

# Define tools for p1
hover_p1 = HoverTool(
        tooltips=[
            ("Time (hour)", "@x{0.00}"),
            ("Temperature (Celsius)","@y{0.00}" )
        ]
    )

# Create plots
p1 = figure(y_range=[0,50],
            width=400,
            height=300,
            title="Soil Temperature as a Function of Time",
            tools=[hover_p1],
            toolbar_location="right")

p1.xaxis.axis_label = 'Time [hours]'
p1.yaxis.axis_label = 'Temperature'
p1.line('x','y',source=source_p1)


# Set data for p2
source_p2 = ColumnDataSource(data=dict(x=T_depth, y=-z_vec))

# Define tools for p1
hover_p2 = HoverTool(
        tooltips=[
            ("Depth (meters)","@y{0.00}"),
            ("Temperature (Celsius)","@x{0.00}")
        ]
    )

# Create plots
p2 = figure(y_range=[0,-z_max],
            width=400,
            height=300,
            title="Soil Temperature as a Function of Soil Depth",
            tools=[hover_p2],
            toolbar_location="right")

p2.xaxis.axis_label = 'Temperature'
p2.yaxis.axis_label = 'Depth (cm)'
p2.min_border_left = 100
p2.line('x','y',source=source_p2)

show(row(p1,p2))

```








  <div class="bk-root" id="a472a0e0-9aae-450e-8827-13aa03164325"></div>




