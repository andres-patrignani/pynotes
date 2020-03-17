# Comparative Anatomy of Functions

Simialr to the notebooks about the comparative anatomy of `for loops` and `if statements`, in this notebook the goal is to observe similarities when declaring and invoking functions in common languages for scientific computing.

## Python

```python
# Define function
def hypotenuse(C1,C2):
    H = (C1**2 + C2**2)**0.5
    return H

# Invoke function
hypothenuse(3,4)
```

**Answer**
```
5.0
```

---

## Matlab

Functions defined within a script must be defined at the end of the script in Matlab

```matlab

% Invoke function
hypotenuse(3,4)

% Define function
function [H] = hypothenuse(C1,C2)
    H = (C1.^2 + C2.^2).^0.5;
end
```

**Answer**
```
5
```

---

## Javascript

```javascript

//Define function
function hypotenuse(C1,C2){
    H = (C1**2 + C2**2)**0.5
    return H
}

//Invoke function
hypothenuse(3,4)
```

**Answer**
```
5
```

---

## Julia

```julia
# Define function
function hypotenuse(C1,C2)
    H = (C1^2 + C2^2)^0.5
    return H
end
```

**Answer**
```
5.0
```

## Synthesis

- All languages contain a keyword defining the declaration of a function

- All languages contain syntax for declaring function name, inputs, and outputs

- Functions are invoked in all languages by calling the function by its name and passing the input arguments between parentheses.


