# Hello world

Here are some of the most basic Jupyter Lab commands so that you can create your very first notebook.

**Few comments about Jupyter notebooks**
- Code lives in cells, which help breaking down the problem, organizing your code, and debugging your code. Cells add modularity and improves readbility of your code. Cells can be executed individually to ensure that the code is working properly before you move forward onto the next cell.

- To run code in a cell press: ctrl + enter key. The result will appear right below the code.

- To run code in a cell and move onto the next cell when done press: ctrl + shift + enter keys


```python
# The most iconic line of code when learning how to program
print("Hello World")
```

    Hello World



```python
# Comments are represented by the pound symbol "#". 
# So these few lines are comments and will be ignored by the Python interpreter.
# Comments are useful to document your code
```


```python
print("This sentence will print") # This one will not
```

    This sentence will print



```python
# Define a variable
a = 1
print(a)
```


```python
# Variables can be accessed from a different cell (variables are not actually attach to any cell)
# As long as variables are in memory you can call and use them
print(a)
```

    134



```python
# Re-define a variable
a = 2
print(a)

# Now a changed its value from 1 to 2
```

    2



```python
# Student question: Can we define variables using numbers?
1 = "a"

# The answer is no. Variable names must start with a letter or an underscore
# This cell will produce an error.
```


```python
# Define multiple variables at the same time
mu, sigma = 0, 0.2
print(mu)
print(sigma)
```


```python
# Arithmetic operations

print(3 + 4)  # Addition
print(3 - 4)  # Subtraction
print(3 * 4)  # Multiplication
print(3 / 4)  # Division
print(3**4)   # Exponentiation
print(10 % 4) # Modulues
print(10 % 3) # Remainder

```

    7
    -1
    12
    0.75
    81
    2

