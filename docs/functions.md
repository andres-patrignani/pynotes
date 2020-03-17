# Functions

A short introduction to Python functions. In this tutorial we will learn the basic Python syntax including function declaration, inputs, outputs, and documentation (called docstring).

Functions allow us to re-use code that does something specific. In simple terms, functions are snippets of code associated to a name, so that we can call them by their name everytime we need them. They are helpful to avoid repeating the same lines of code all over the program we are building. Functions also allow us to reduce error (we don't have to type everything again) and add documentation. 

Functions make our code more modular. This modularity applies to the current project and may extend to other projects, which means that useful functions that emerge from one project can be used in future projects. The power pf functions comes when you start building your own library of functions. They are one of the most valuable assets in scientific programming.

>Functions are like tools in a toolbox, they serve or perform a well defined task and can be re-used multiple times. You grab a tool when you need it and you move on onto the next task.

Using an analogy, think of functions as a wrench or a screwdriver everytime you have to adjust a nut or tight up a screw. However, to use a wrench or screwdriver you first have to go and buy it at the hardware store. You can only use a tool that is at hand.

So, to understand functions we need to split the process into two steps: 

**Step 1**: define/declare the function

**Step 2**: call/invoke the function


## Anatomy of a function


- Import modules outside the function

- Assign meaningful names to functions.

- Typically function names are all in lower case.

- The ``return` statement indicates the end of the function. Lines after the `return` statement will not be executed by the interporeter. This also applies to `if statements`. As soon as the function hits the line with the `return` statement it will terminate execution.

<img src="../docs/_media/anatomy_function.png" />

## Declare function

This is the function definition. When we define the function we simple assign the inputs, outputs, and logic of the code, but at this point we are not running the code. That part will occur when we call or invoke the function in the second step. In simple words, declaring a function is like encapsulating some code under a specific name. 


```python
# Declare function
def hypotenuse(a,b):
    
    """
    Function that calculates the 
    longest side of a right-angled triangle
    given its two legs.
    
    Keyword arguments:
    a,b -- sides of the right-angle triangle
    
    Author: Andres Patrignani
    Date: 28-Feb-2020
    email: andrespatrignani@ksu.edu
    """
    
    # Compute hypotenuse
    c = (a**2 + b**2)**0.5
    
    # Output desired variables
    return c
```

## Call function

Now that we assigned a name and some specific inputs to our code, we can call the function as many times as we want without the need for rewriting the code again.


```python
# Call function
hypotenuse(3,4)
```




    5.0



## Function docstring

The function `docstring` is defined as a multi-line string `'''Like this'''` and consists of a summary line about the function and a brief description of the inputs. Function documentation needs to be succint and clear, but you can also add units, examples, author, creation date and contact information. The personal information can be omitted, but for personal and research projects I like to include this information in case I share the code with students and colleagues. Here are some suggestions to write brief and meaningful docstrings:

- A brief description of the purpose of the function (20 words or less)
- A brief description about the format of the input variable
- Author's full name
- Date of creation

Let's see what happens when we print the help of our function



```python
hypotenuse?
```


    [0;31mSignature:[0m [0mhypotenuse[0m[0;34m([0m[0ma[0m[0;34m,[0m [0mb[0m[0;34m)[0m[0;34m[0m[0;34m[0m[0m
    [0;31mDocstring:[0m
    Function that calculates the 
    longest side of a right-angled triangle
    given its two legs.
    
    Keyword arguments:
    a,b -- Sides of the right-angle triangle
    
    Author: Andres Patrignani
    Date: 28-Feb-2020
    email: andrespatrignani@ksu.edu
    [0;31mFile:[0m      ~/Dropbox/Teaching/Scientific programming/pynotes/notebooks/<ipython-input-29-ed44cb9b871d>
    [0;31mType:[0m      function



## Additional comments

Before we proceed is probably a good idea to point out some of the limitations and assumptions of our recently defined `hypotenuse()` function:

- The function assumes the side of the right-angle triangle had the same units.

- Documentation gives little insight to a user that doesn't know what a right-angle triangle is.

- The function does not have error checks.  What happens if the user adds an invalid input (e.g. a string)?


## Order of inputs and default values

The order of the inputs matter when calling a function. Python has a specific syntax to ensure that input arguments don't get mixed up even if the inputs are passed to the function in a different order than the specified order when the function was defined.



```python
# Define function
import math

def cone(radius=1, height=1):
    """Function that computes the volume and area of a cone
    given its basal radius and height
    """
    
    cone_area = math.pi * radius**2
    cone_volume = cone_area * height/3
    
    #return {"cone_volume":cone_volume, "cone_area":cone_area}
    return cone_volume, cone_area

```

### Call with one input


```python
# Call function with one input
radius = 5 # cm
ans = cone(radius)
print(ans)


```

    (26.179938779914945, 78.53981633974483)


In the previous call we only had to provide the function with one input. For the other input the function uses the predefined default value.


### Call with two inputs

Users can change override default values by explicitly specifying the value of the other parameters in the function call. By being explicit we can pass the value of the input argument in any order in the function call. If we want to modify the contact angle to a value other than 20 degrees, we can specify it like this:


```python
# Call function with two inputs
radius = 7 # cm
height = 10 # cm
ans = cone(radius, height)
print(ans)

```

    (513.1268000863329, 153.93804002589985)


### Wrong call

What happens if we call the function with the order of the inputs changed? Will Python still recognize `radius` and `height` as the radius and `height` of the cone regardless of the order of the input arguments in the function call?



```python
# Call function with two inputs
radius = 7 # cm
height = 10 # cm
ans = cone(height, radius)
print(ans)

```

    (733.0382858376184, 314.1592653589793)


In this case, the function treats the first input argument `height` as the radius, and the radius as `height`. **The previous function call is wrong**. If we don't specify the name of the input during the function call, then must obey the order of the inputs as defined in the function. 


### The right call

The correct call for the previous example would be as a follows:


```python
cone(height=height, radius=radius)
```




    (513.1268000863329, 153.93804002589985)



or alternatively

Now we obtain the correct result despite the input arguments were passed in a different order compared to the order in the function definition.

These type of syntax is common in plotting libraries, which have a large number input arguments that specify the attributes of the elements of a plot. This way, we can easily set the marker type or line width property without worrying about the order.


### Return

By default when returning multiple output arguments separated by commas Python returns a tuple. Tuples might not be the most convenient since we need to know the order and meaning of the tuple values. Sometimes is more explicit to use a dictionary, where we can access information by the name of the property.

I suggest you mute this line: `return cone_volume, cone_area` and unmute this line: `return {"cone_volume":cone_volume, "cone_area":cone_area}` in the previous function. Don't forget to re-run the cell to update the function definition and then call the function again to see the results.

> Muting code means that we turn a line of code into a comment by adding a `#` sign at the beginning of the line of code to make Python believe this line is a comment. This way you can keep alternative lines of code around your code that will be ignored during execution. 


## Variable scope

An important, and perhaps not so obvious, concept is that functions have their own variable workspace. This means that while the function is being executed by the interporeter, the variables defined within the function are in a different container compared to the variables define in the code outside the function.

>`global` variables can be accessed from anywhere in your code, but its use is not recommend (unless you know exactly what you are doing). In short code snippets it is trivial to manage global variabkes, but when building extensive programs the chances of conlficts with other variables and errors grow quickly.

- What happens if we have a variable with the same name inside a function and outside the function?
- Will the variable inside the function adopt the value defined outside the function?

- Will the variable inside the function be independent of its counterpart outside the function?

- How do we write our code if for semantic reasons we need to assign the same name to variables inside and outside the function?



```python
# A dummy variable to allow us see the scope of the variable
max_val = 15 
print('Value of max_val outside the function is:', max_val)

# Declare function
def onehundred():
    """Function calculates the sum of all integers from 1 to 100.
    Created by Andres Patrignani on 20-feb-2019
    """
    
    max_val = 101
    print('Value of max_val inside the function is:', max_val)
    
    # Compute and return the sum from 0 to max_val
    return sum(range(max_val))


# Call function
print(onehundred())
print('Value of max_val outside the function is:', max_val)
```

    Value of max_val outside the function is: 15
    Value of max_val inside the function is: 101
    5050
    Value of max_val outside the function is: 15

