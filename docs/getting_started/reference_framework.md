# **Reference coding framework**

## Define the goal
Briefly describe the purpose of the code. What need does your idea satisfy?
The objective of a code can be as simple as analyzing a set of data or as advanced as a complex simulation. 

## Sketch the process (Prototyping)
Informally, draw out in a piece of paper how you foresee the code will work. Identify the main inputs and outputs of your code, tentative decisional points that will control the flow of your code, and how these components connect with each other. If your code relies on multiple equations, add them to the sketch.

## Write pseudo-code (Conceptual model)
Literally, breakdown the problem into smaller steps using your own words. You can do this below the sketch or in the form of comments or code statements in code editor. Describe whether inputs will be retrieved from a remote server, the file system, or from the the user through an interactive interface. Identify potential functions that may emerge from this work and that can be re-used throughout the code.

## Adopt version control
To keep track of changes and have a copy of your code it is an imperative practice to adopt tools like git, especially when you are building code as part of a team. Adopt these tools even if you are working alone. Although maybe not idea, even tools like Dropbox will be a step forward and may allow inexperienced coders that feel intimidated by more advanced tools to remain engaged.

## Draft the code
In this step it is important that you adopt good programming habits. Always include your
name, date, purpose of the code, and a brief description of inputs and outputs (units,
data type, mandatory/optional inputs). Make sure you include meaningful comments. If a blck of code is 
long and complicated, take few extra lines at the beginning to explain in more detail the purpose of the code.
If you follow the steps or equations detailed in a peer-reviewed manuscript, add a comment next to each 
correpsonding line to make the code more readable.

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
Ensure that others read your code. Don't take criticism personally. If something is not clear it might be due to bad syntax, improper logic, or lack of documentation. If you are the reviewer, be respectful, honest, and provide constructive criticism.

## Refactor code
This step involves improving the time efficiency of the code by re-writing some lines of code in a more time-efficient way, by replacing functions with similar but more efficient (or newer) functions, removing unnecessary code from loops, replacing loops by element-wise or array operations, by pre-allocating memory, etc. 

Also, pay special attention to parts of the code that need changes to meet a specific code style, add meaningful comments, and be sure to have a well-documented help section at the beginning of the code.

Polishing your code is essential to take it to the next step. Test every change to ensure that the code does not change its behavior. Polish your code one or few lines at the time.