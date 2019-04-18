
# **Numpy Module**

## Introduction

Module describing basic commands of the numpy module
Documentation: <https://docs.scipy.org/doc/numpy/reference/index.html>


```python
import numpy as np
```


```python
# Multiply a scalar by an array
A = [1,2,3,4]
A * 3

# Not exactly what we are looking for. It will repeat the array three times
```




    [1, 2, 3, 4, 1, 2, 3, 4, 1, 2, 3, 4]




```python
# Now let's create another array, B, with the same dimension as A
B = [5,6,7,8]

# and let's try to multiply it together
A*B
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-40-e1a163a6f9e4> in <module>()
          3 
          4 # and let's try to multiply it together
    ----> 5 A*B
    

    TypeError: can't multiply sequence by non-int of type 'list'


## Create Numpy arrays



```python
# Re-define the previous arrays as numpy arrays
A = np.array([1,2,3,4])
B = np.array([5,6,7,8])

```


```python
# An alternative by specifying the data type
np.array([1,2,3,4], dtype="int64")
np.array([1,2,3,4], dtype="float64")

```




    array([1., 2., 3., 4.])



A common pitfall is to create a numpy array using parenthesis only. The following approach won't work:

```python
array = np.array(1,2,3,4)
```


```python
# Check type
print(type(A))

```

    <class 'numpy.ndarray'>


## Operations


```python
# Operations with Numpy arrays
print(A*3)   # Vector times a scalar
print(A + B)
print(A - B)
print(A * B)
print(A / B)
print(A**2 + B**2) # Exponentiation Calculate hypotenuse of multiple rectangle triangle
print(A.sum())

```

    [ 3  6  9 12]
    [ 6  8 10 12]
    [-4 -4 -4 -4]
    [ 5 12 21 32]
    [0.2        0.33333333 0.42857143 0.5       ]
    [26 40 58 80]
    10



```python
# Dot product (useful for linear algebra operations)
# The dot product is useful when dealing with matrix multiplication. 
# It is often used to calculate the weighted sum.

O = [[1, 2, 3], 
      [4, 5, 6]]

P = [[0.3, 0.8], 
     [0.2, 0.1],
     [0.5, 0.1]]

Q = np.dot(O, P)
print(Q)

```

    [[2.2 1.3]
     [5.2 4.3]]


The values of Q are calculated as follow:

```python
Q11 = 1*0.3 + 2*0.2 + 3*0.5
Q21 = 4*0.3 + 5*0.2 + 6*0.5
Q12 = 1*0.8 + 2*0.1 + 3*0.1
Q22 = 4*0.8 + 5*0.1 + 6*0.1
```


## Attributes


```python
# Most important attributes of Numpy arrays
array2D = np.array([[1,2,3,4],[5,6,7,8]]) # 2D array

print(array2D.ndim)  # Number of dimensions
print(array2D.shape) # Size of the array
print(array2D.size)  # Total number of elements
print(array2D.dtype) # Type of elements in array

# Get specific dimensions of an array
print(array2D.shape[0]) # number of rows
print(array2D.shape[1]) # Number of columns
```

    2
    (2, 4)
    8
    int64
    2
    4



```python
# Example with a name
john = np.array([1,2,3])

print(john.dtype)
print(john.mean())
print(john.size)
print(type(john))
```


```python
# Change shape of array
print('Original 2D:')
print(array2D)

print('New 2D:')
print(array2D.reshape(4,2))

print('New 3D:')
print(array2D.reshape(2,2,2))
```

    Original 2D:
    [[1 2 3 4]
     [5 6 7 8]]
    New 2D:
    [[1 2]
     [3 4]
     [5 6]
     [7 8]]
    New 3D:
    [[[1 2]
      [3 4]]
    
     [[5 6]
      [7 8]]]


## Indexing and slicing

                    +---+---+---+---+---+---+---+---+---+---+
                    | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
                    +---+---+---+---+---+---+---+---+---+---+
    Slice position: 0   1   2   3   4   5   6   7   8   9   10
    Index position:   0   1   2   3   4   5   6   7   8   9




```python
# Create example array
x = np.arange(10)
print(x)

# Indexing
print(x[1]) # Second element

# Slicing
# array[start:stop:step]

print(x[0:2])    # First and second element
print(x[0:])     # All elements (from 0 and on)
print(x[0:10:2]) # Every other element
print(x[0:-1:2]) # Every other element, without knowing the total number of elements


```

    [0 1 2 3 4 5 6 7 8 9]
    1
    [0 1]
    [0 1 2 3 4 5 6 7 8 9]
    [0 2 4 6 8]
    [0 2 4 6 8]


## Slicing exercise

Given the following 5 by 5 matrix of integers:


```python
np.random.seed(0)
M = np.random.randint(0,10,[5,5])
print(M)

```

    [[5 0 3 3 7]
     [9 3 5 2 4]
     [7 6 8 8 1]
     [6 7 7 8 1]
     [5 9 8 9 4]]


Write the five python commands to extract:
- top row
- bottom row
- right-most column
- left-most column
- upper-right 3x3 matrix


```python
# Answer
# Top row
print(M[0,:]) # Preferred
print(M[:1][0])
print(M[0])

# Bottom row
print(M[-1,:]) # Preferred
print(M[-1])
print(M[4,:])


# Right-most column
print(M[:,-1])

# Left-most column
print(M[:,0])

# Upper-right 3x3 matrix
print(M[0:3,2:M.shape[1]])

# This will not work:
# M[0:3,2:1]
# Read '2:' as all columns (or rows) from index 2 to the end (all the remaining, to the end)
```

    [5 0 3 3 7]
    [5 0 3 3 7]
    [5 0 3 3 7]
    [5 9 8 9 4]
    [5 9 8 9 4]
    [5 9 8 9 4]
    [7 4 1 1 4]
    [5 9 7 6 5]
    [[3 3 7]
     [5 2 4]
     [8 8 1]]


## Numpy random module


```python
# Random module within Numpy. Random integers from uniform distribution
np.random.seed(0) # For reproducibility

# numpy.random.randint(low, high=None, size=None, dtype='l')
R1 = np.random.randint(0,10) # Scalar
print(R1)

R2 = np.random.randint(0,10,7) # One dimensional array
print(R2)

R3 = np.random.randint(0,10,[5,5]) # Two-dimensional array
print(R3)

```

    5
    [0 3 3 7 9 3 5]
    [[2 4 7 6 8]
     [8 1 6 7 7]
     [8 1 5 9 8]
     [9 4 3 0 3]
     [5 0 2 3 8]]



```python
# Normally distributed random numbers from the “standard normal” distribution.
# Mean = 0 
# Standard deviation = 1 
np.random.randn() # A single float
np.random.randn(5) # A 1-D array
np.random.randn(3,3) # A 2-D array

```




    array([[-0.40602374,  0.26644534, -1.35571372],
           [-0.11410253, -0.84423086,  0.70564081],
           [-0.39878617, -0.82719653, -0.4157447 ]])




```python
# Random numbers between 0 and 1 from a uniform distribution
print(np.random.rand(5))

```

    [0.38648898 0.90259848 0.44994999 0.61306346 0.90234858]



```python
# Random numbers from specific distribution

mu, sigma = 0, 0.1 # mean and standard deviation
s = np.random.normal(mu, sigma, 1000) # random samples from a Gaussian distribution
print(s)

```

## Numpy common functions


```python
# Generate a range
# np.arange(start,stop,step)
print(np.arange(0,5))
print(np.arange(0,5,2))
print(np.arange(0,60,5))

# Challenge: Compute sum of integers from 1 to 100 using numpy
print(np.arange(101).sum())

```

    [0 1 2 3 4]
    [0 2 4]
    [ 0  5 10 15 20 25 30 35 40 45 50 55]
    5050



```python
# Linear range
# numpy.linspace(start, stop, num=50, endpoint=True)
print(np.linspace(-10, 10, 7))

```

    [-10.          -6.66666667  -3.33333333   0.           3.33333333
       6.66666667  10.        ]



```python
# Array of zeros
np.zeros([5,3])
```




    array([[0., 0., 0.],
           [0., 0., 0.],
           [0., 0., 0.],
           [0., 0., 0.],
           [0., 0., 0.]])




```python
# Array of ones
np.ones([4,3])

```




    array([[1., 1., 1.],
           [1., 1., 1.],
           [1., 1., 1.],
           [1., 1., 1.]])




```python
# NaN values
np.ones([4,3])*np.nan

```




    array([[nan, nan, nan],
           [nan, nan, nan],
           [nan, nan, nan],
           [nan, nan, nan]])




```python
# Vector
N = 5
lat = np.linspace(36, 40, N)
lon = np.linspace(-102, -98, N)
print(lat)
print(lon)

# Matrix
LAT,LON = np.meshgrid(lat,lon)
print(LAT)
print(LON)

# Generate a Z matrix using the X and Y matrix
Z = X**2 + Y**2

```

    [36. 37. 38. 39. 40.]
    [-102. -101. -100.  -99.  -98.]
    [[36. 37. 38. 39. 40.]
     [36. 37. 38. 39. 40.]
     [36. 37. 38. 39. 40.]
     [36. 37. 38. 39. 40.]
     [36. 37. 38. 39. 40.]]
    [[-102. -102. -102. -102. -102.]
     [-101. -101. -101. -101. -101.]
     [-100. -100. -100. -100. -100.]
     [ -99.  -99.  -99.  -99.  -99.]
     [ -98.  -98.  -98.  -98.  -98.]]


## Numpy combined with Matplotlib


```python
import matplotlib.pyplot as plt
%matplotlib inline

T_avg = 15              # Average annual air temperature
T_amp = 10              # Annual air temperature amplitude
doy = np.arange(1,366)  # Generate vector with days of the year

x = 2 * np.pi * doy/365     # Convert doy into pi-radians (1 revolution = 2PI radians = 365 days)
y = T_avg + T_amp*np.sin(x) # Main trend using a sine wave that oscillates around the mean temperature

noise = np.random.normal(0, 3, x.size)  # White noise having zero mean and finite variance
ynoisy = y + noise                      # A more realistic curve of air temperature

plt.plot(doy, y, '-r')
plt.plot(doy, ynoisy, '-k')
plt.xlabel('Days')
plt.ylabel('Air Temperature')
plt.show()

```


![png](output_36_0.png)


>Press ctrl + enter several times to show how different sets of random numbers change the chart
