
# Fun Functions

A short introduction to Python functions. In this tutorial we will learn the basic Python syntax including function declaration, inputs, outputs, and documentation (called docstring).

Functions allow us to re-use code that does something specific. In simple terms, functions are snippets of code associated to a name, so that we can call them by their name everytime we need them. They are helpful to avoid repeating the same lines of code all over the program we are building. Functions also allow to reduce error and add documentation.

Using an analogy, think of functions as a wrench or a screwdriver everytime you have to adjust a nut or tight up a screw. However, to use a wrench or screwdriver you first have to go and buy it at the hardware store. You can only use a tool that is at hand.

So, to understand functions we need to split the process into two steps:

**Step 1**: Create/Define the function

**Step 2**: Use/Call the function



```python
def squarednum(x):
    
    return x**2

squarednum(4)
```




    16




```python
# STEP 1: DEFINING A FUNCTION

import math

def cone (radius, height):
    """Calculates the volume of a cone.

    Keyword arguments:
    radius -- radius of the base of the cone
    height -- height of the cone
    
    Author: Andres Patrignani
    Date: 28-Oct-2018
    email: andrespatrignani@ksu.edu"""
    
    coneArea = math.pi * radius**2
    coneVolume = coneArea * height/3
    
    return coneVolume, coneArea
```

**Line 3**: Import required modules

**Line 5**:

    def  --> indicates that we are defining a function

    cone --> function name, typically all lower case

    (radius, height) -->  function inputs

    : --> colon stating that the code that follows will get executed when the function is called.
                      
**Lines 6 to 14**: Documentation (also called docstring). It consists of a summary line and description of inputs. The documentation could be expanded in case the function is not trivial. You can also units, examples, author, creation date and contact information. The personal information can be omitted, but for personal and research projects I like to include this information in case I share the code with students and colleagues.

**Line 16 to 17**: Function code

**Line 19**: Return statement indicating the end of the function and output variables


```python
# STEP 2: CALLING A FUNCTION
# Output variables are in a tuple
cone(2,5)
```




    (20.943951023931955, 12.566370614359172)




```python
# Observations
# The function assumes the radius and height had the same units.
# The function could also output the area of the cone base.
# Documentation gives little insight to a user that doesn't know what a cone is.
# The function does not have error checks. 

# What happens if the user only adds a single input?
# What happens if the user adds an invalid input (e.g. a string)?
# What if the user adds a list of radii and a list of height values to calculate the volume of multiple cones?

```


```python
# Simple version without for loops
def onehundred():
    """Function calculates the sum of all integers from 1 to 100.
    Created by Andres Patrignani on 20-feb-2019
    """
    return sum(range(101))


print(onehundred())
```

    5050



```python
# Version using a for loop
def onehundred():
    """Function calculates the sum of all integers from 1 to 100.
    Created by Andres Patrignani on 20-feb-2019
    """
    cumulative_sum = 0;
    for i in range(1,101):
        cumulative_sum = cumulative_sum + i
    return cumulative_sum


print(onehundred())
    
    
```

    5050

