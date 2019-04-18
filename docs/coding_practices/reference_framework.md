# **Reference coding framework**

## Define the goal
Briefly describe the purpose of the code. What need does your idea satisfy?
What tangible outcomes do you envision?

## Sketch the process (Prototyping)
I cannot enphasize this step enough. Informally, draw out in a piece of paper how you foresee the code will work. Include the main inputs, local variables, and outputs, and how they connect with each other. This step will help you defining variables, main processes, and the direction of the process in the
code. If your code relies on multiple equations, add them to the sketch.

## Write pseudo-code (Conceptual model)
Literally, write in your own words few sentences on how you expect the code to work. Describe if inputs will be retrieved from a database or from the user, if you see your code as a webpage, as
a simple script, or as a function. Breakdown the problem into smaller steps using your own words.

## Adopt version control
To keep track of changes and have a copy of your code it is an imperative practice to adopt tools like git, especially when you are building code as part of a team. Adopt these tools even if you are working alone. Although maybe not idea, even tools like Dropbox will be a step forward.

## Draft the code
In this step it is important that you adopt good programming habits. Always include your
name, date, purpose of the code, and a brief description of inputs and outputs (units,
string/numeric, mandatory/optional). Make sure you include meaningful comments in
almost each line.

## Error debugging
Finding the errors in your code is critical. At the very end we are building a code on
which we can rely and use as many times as we want. At this time you have to keep in
mind that syntax errors will prevent the code from running from beginning to end. For
instance, syntax errors can be a missing parenthesis, a missing dot, an extra comma,
misspelling a function name, a missing or extra “end” statement, etc. Also, mathematical
operations between vectors and matrices of different size sometimes cannot be
performed. Using the built-in Matlab debugger can be helpful, but also meticulous
examination of the code is required. In Matlab programming terms you can debug your
code using the following reasoning:

```python
    Run code
        if number of errors >0
            Find error
            Fix error
        Run code
    end
```

## Code review
Ensure that others read your code. Don't take criticism personally. If something is not clear it might be due to bad syntax, improper logic, or lack of documentation. If you are the reviewer, be respectful and be honest.

## Refactor code
This step involves improving the time efficiency of the code by re-writing some lines of code in a more time-efficient way, by replacing functions with similar but more efficient (or newer) functions, removing unnecessary code from loops, replacing loops by element-wise or array operations, by pre-allocating memory, etc. 

Also, pay special attention to parts of the code that need changes to meet a specific code style, add meaningful comments, and be sure to have a well-documented help section at the beginning of the code.

Polishing your code is essential to take it to the next step. Test every change to ensure that the code does not change its behavior. Polish your code one or few lines at the time.



