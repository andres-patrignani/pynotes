```python
# Plotting using Bokeh library
# Andres Patrignani
# Last updated: 12-Oct-2018

# References:  https://bokeh.pydata.org/en/latest/index.html


# Highlights
# Multiple plotting libraries, each follows a different philosophy.
# Try few but master one plotting library at the beginning.
# As with any library you need to learn its syntax.
# Bokeh has great documentation, clean syntax, plot interactivity, and research quality visuals

# I encourage you to read the Bokeh documentation, it has great examples and you may end up learning something else
# in addition to what you are looking for.

```


```python
# Import Bokeh modules
from bokeh.plotting import figure, show
from bokeh.io import output_notebook        # Required to embed figures in the notebook
output_notebook()                           # Initialize the bokeh for the notebook

# If everything went correct you should see the following message: " BokehJS 0.13.0 successfully loaded."
```



<div class="bk-root">
    <a href="https://bokeh.pydata.org" target="_blank" class="bk-logo bk-logo-small bk-logo-notebook"></a>
    <span id="e95a266a-ccfb-4c5b-9c83-1822d181cbf2">Loading BokehJS ...</span>
</div>





```python
# A minimalistic plot to understand the building blocks

# Generate some data stored in arrays
x = [1,2,3]
y = [4,5,6]

# Create an empty figure. What constitutes a figure is now stored in the variable "p"
p = figure()

# Add a line to the figure. Because we set "p EQUAL TO figure" in the previous step we can use p to refer to the figure. 
p.line(x,y)

# The fact that we created a figure and added a line does not mean that we can see it. 
# These are two different process for the computer, which means that we need to explicitly show the figure
show(p)

# Explore the figure below. This is a high level chart, which means that many features (font style, font size, space of 
# tickmarks, range of the x and y axis, etc.) have been plotted using some defaults. This is great because there are many 
# features that constitute a plot. The idea is to fin-tune the features that we consider relevant.
```








<div class="bk-root" id="f0fb8dc3-b1bf-408f-bfdc-0e50ae3cb181"></div>






```python
# Let's create the samefigure again, but this time adding few more features.
```


```python
p = figure()  # If there exists a variable p already, python will overwrite the variable with new information.
p.title.text = "My First Plot"
p.plot_width = 300
p.plot_height = 300

# Alternatively you can write: p = figure(title="My First Plot", plot_width=300, plot_height=300)
# To me having one property per line is cleaner and easier to read, but it's really a matter of personal preference.

# Let's add some red color
p.line(x,y, line_color='red')

# To learn how to set more line plot attributes go to:
# https://bokeh.pydata.org/en/latest/docs/reference/plotting.html#bokeh.plotting.figure.Figure.line
# line(x, y, **kwargs)

# and display the new figure
show(p)

```








<div class="bk-root" id="a3d7a83a-d2dd-4db9-9c16-ce0b960e25c2"></div>






```python
# Lastly, I want to show you another example with more features. Take the time to implement the code and play with the variables.

# Docs of plot attributes: https://bokeh.pydata.org/en/latest/docs/reference/plotting.html

# Styling examples: https://bokeh.pydata.org/en/latest/docs/user_guide/styling.html

p = figure()

# Plot size
p.plot_width = 400
p.plot_height = 300

# Title
p.title.text = 'A more elaborate plot'
p.title.text_color = 'olive'
p.title.text_font = 'times'
p.title.text_font_style = 'italic'

# Axis labels
p.xaxis.axis_label = 'Variable X'
p.xaxis.axis_label_text_font_size = '16pt' # A common mistake here is to write a number rather a string.
p.yaxis.axis_label = 'Variable Y'
p.yaxis.axis_label_text_font_size = '16pt'

# Line
p.line(x,y, line_color='red', line_dash='dashed', line_width=2.0, legend='red line')

# Legend
p.legend.location = 'top_left'  # By default is on the top right

# Grid
p.xgrid.visible = False
p.ygrid.visible = False

# Tick marks
p.xaxis.major_tick_line_color = "firebrick"
p.xaxis.major_tick_line_width = 3
p.xaxis.minor_tick_line_color = "orange"

# Plot outline box 
p.outline_line_color = "black"

show(p)
```








<div class="bk-root" id="0eb91c0b-7e3e-42a6-ad15-39b0d691877d"></div>






```python
# Simple plot to review steps

# Load modules
from bokeh.plotting import figure, show, output_file
from bokeh.models import ColumnDataSource, ColorBar
from bokeh.palettes import Spectral6
from bokeh.transform import linear_cmap

# Set name of output file if needed
#output_file("styling_linear_mappers.html", title="styling_linear_mappers.py example")

# Data to plot
x = [1,2,3,4,5,7,8,9,10]
y = [1,2,3,4,5,7,8,9,10]

# Generate colormap according to one of the variable values
mapper = linear_cmap(field_name='y', palette=Spectral6 ,low=min(y) ,high=max(y))

# Prepare data by constructing a dictionary
source = ColumnDataSource(dict(x=x,y=y))

# Create figure
p = figure(plot_width=300, plot_height=300, title="Linear Color Map Based on Y")

# Add as many glyphs as you want
p.circle(x='x', y='y', line_color=mapper,color=mapper, fill_alpha=1, size=12, source=source)

# Create and add additional elements to the plot
color_bar = ColorBar(color_mapper=mapper['transform'], width=8,  location=(0,0))
p.add_layout(color_bar, 'right')

# Display the plot
show(p)
```








<div class="bk-root" id="bc458f25-b4ef-41c2-a204-d37eea7106e8"></div>




