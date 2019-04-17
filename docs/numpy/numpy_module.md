
# **Numpy Module**

## Table of contents
1. [Introduction](#introduction)
2. [Create Numpy arrays](#create_arrays)


<a name="introduction"></a>
## Introduction <a name="introduction"></a>

Module describing basic commands of the numpy module
Documentation: <https://docs.scipy.org/doc/numpy/reference/index.html>


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


<a name="create_arrays"></a>
## Create arrays



```python
import numpy as np

# Re-define the previous arrays as numpy arrays
A = np.array([1,2,3,4])
B = np.array([5,6,7,8])

# Other alternatives
#np.array([1,2,3,4], dtype="int64")
#np.array([1,2,3,4], dtype="float64")


# Wrong approach
# array = np.array(1,2,3,4)
```


```python
# Check type
print(type(A))
```

    <class 'numpy.ndarray'>



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
# Challenge: Compute sum of integers from 1 to 100 using numpy
print(np.arange(101).sum())
```

    5050



```python
# Most important attributes of Numpy arrays
array2D = np.array([[1,2,3,4],[5,6,7,8]]) # 2D array

print(array2D.ndim)  # Number of dimensions
print(array2D.shape) # Size of the array
print(array2D.size)  # Total number of elements
print(array2D.dtype) # Type of elements in array

```

    2
    (2, 4)
    8
    int64



```python
# Get specific dimensions of an array
print(array2D.shape[0]) # number of rows
print(array2D.shape[1]) # Number of columns
```

    2
    4



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
# Calculate the main attributes of the array
print(R3.ndim)  # Number of dimensions
print(R3.shape) # Size of the array
print(R3.size)  # Total number of elements
print(R3.dtype) # Type of elements in array
```

    2
    (5, 5)
    25
    int64



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


```python
# Indexing

x = np.arange(10)
print(x)

#                +---+---+---+---+---+---+---+---+---+---+
#                | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
#                +---+---+---+---+---+---+---+---+---+---+
#Slice position: 0   1   2   3   4   5   6   7   8   9   10
#Index position:   0   1   2   3   4   5   6   7   8   9


# array[start:stop:step]

print(x[1])      # Second element (indexing)
print(x[0:2])    # First and second element (slicing)
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



```python
# Retrive array values starting from the end
print(x[8:0:-1])
print(x[8::-1]) # 8: 'from 8 on' combined with '-1' for negative step and skipping the middle input (the stop)

```

    [8 7 6 5 4 3 2 1]
    [8 7 6 5 4 3 2 1 0]



```python
# Generate a range
print(np.arange(0,5))
print(np.arange(0,5,2))

```

    [0 1 2 3 4]
    [0 2 4]



```python
# Linear range
# numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)[source]¶
print(np.linspace(-10, 10))
print(np.linspace(-10, 10,10))
```

    [-10.          -9.59183673  -9.18367347  -8.7755102   -8.36734694
      -7.95918367  -7.55102041  -7.14285714  -6.73469388  -6.32653061
      -5.91836735  -5.51020408  -5.10204082  -4.69387755  -4.28571429
      -3.87755102  -3.46938776  -3.06122449  -2.65306122  -2.24489796
      -1.83673469  -1.42857143  -1.02040816  -0.6122449   -0.20408163
       0.20408163   0.6122449    1.02040816   1.42857143   1.83673469
       2.24489796   2.65306122   3.06122449   3.46938776   3.87755102
       4.28571429   4.69387755   5.10204082   5.51020408   5.91836735
       6.32653061   6.73469388   7.14285714   7.55102041   7.95918367
       8.36734694   8.7755102    9.18367347   9.59183673  10.        ]
    [-10.          -7.77777778  -5.55555556  -3.33333333  -1.11111111
       1.11111111   3.33333333   5.55555556   7.77777778  10.        ]



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
# # Create mesh
N = 10
x = np.linspace(0, 40, N)
y = np.linspace(0, 100, N)


X,Y = np.meshgrid(x,y)

Z = X**2 + Y**2
Z
```




    array([[    0.        ,    19.75308642,    79.01234568,   177.77777778,
              316.04938272,   493.82716049,   711.11111111,   967.90123457,
             1264.19753086,  1600.        ],
           [  123.45679012,   143.20987654,   202.4691358 ,   301.2345679 ,
              439.50617284,   617.28395062,   834.56790123,  1091.35802469,
             1387.65432099,  1723.45679012],
           [  493.82716049,   513.58024691,   572.83950617,   671.60493827,
              809.87654321,   987.65432099,  1204.9382716 ,  1461.72839506,
             1758.02469136,  2093.82716049],
           [ 1111.11111111,  1130.86419753,  1190.12345679,  1288.88888889,
             1427.16049383,  1604.9382716 ,  1822.22222222,  2079.01234568,
             2375.30864198,  2711.11111111],
           [ 1975.30864198,  1995.0617284 ,  2054.32098765,  2153.08641975,
             2291.35802469,  2469.13580247,  2686.41975309,  2943.20987654,
             3239.50617284,  3575.30864198],
           [ 3086.41975309,  3106.17283951,  3165.43209877,  3264.19753086,
             3402.4691358 ,  3580.24691358,  3797.5308642 ,  4054.32098765,
             4350.61728395,  4686.41975309],
           [ 4444.44444444,  4464.19753086,  4523.45679012,  4622.22222222,
             4760.49382716,  4938.27160494,  5155.55555556,  5412.34567901,
             5708.64197531,  6044.44444444],
           [ 6049.38271605,  6069.13580247,  6128.39506173,  6227.16049383,
             6365.43209877,  6543.20987654,  6760.49382716,  7017.28395062,
             7313.58024691,  7649.38271605],
           [ 7901.2345679 ,  7920.98765432,  7980.24691358,  8079.01234568,
             8217.28395062,  8395.0617284 ,  8612.34567901,  8869.13580247,
             9165.43209877,  9501.2345679 ],
           [10000.        , 10019.75308642, 10079.01234568, 10177.77777778,
            10316.04938272, 10493.82716049, 10711.11111111, 10967.90123457,
            11264.19753086, 11600.        ]])




```python
# Trigonometric functions

np.cos([1,0])
np.ceil([0.9, 0.4, 2.99])
one_radian = 360 / (2*np.pi)
np.radians(57.299) # Degrees to radians (a revolution has 2*pi radians, so 360/(2*pi) = 57.29 = 1 radian)

```

    [ 0.00000000e+00  1.27877162e-01  2.53654584e-01  3.75267005e-01
      4.90717552e-01  5.98110530e-01  6.95682551e-01  7.81831482e-01
      8.55142763e-01  9.14412623e-01  9.58667853e-01  9.87181783e-01
      9.99486216e-01  9.95379113e-01  9.74927912e-01  9.38468422e-01
      8.86599306e-01  8.20172255e-01  7.40277997e-01  6.48228395e-01
      5.45534901e-01  4.33883739e-01  3.15108218e-01  1.91158629e-01
      6.40702200e-02 -6.40702200e-02 -1.91158629e-01 -3.15108218e-01
     -4.33883739e-01 -5.45534901e-01 -6.48228395e-01 -7.40277997e-01
     -8.20172255e-01 -8.86599306e-01 -9.38468422e-01 -9.74927912e-01
     -9.95379113e-01 -9.99486216e-01 -9.87181783e-01 -9.58667853e-01
     -9.14412623e-01 -8.55142763e-01 -7.81831482e-01 -6.95682551e-01
     -5.98110530e-01 -4.90717552e-01 -3.75267005e-01 -2.53654584e-01
     -1.27877162e-01 -2.44929360e-16]



```python
# COmbine NUmpy with Matplotlib

import matplotlib.pyplot as plt
%matplotlib inline

x = np.linspace(0, 2*np.pi) # Recall that 50 values is the default for linspace
y = np.sin(x)

plt.plot(x, y, '-r')
plt.xlabel('Days')
plt.ylabel('Air Temperature')
plt.show()
```


```python
# Combine sine and random functions
noise = np.random.normal(0, 0.2, x.size)
y2 = np.sin(x) + noise

plt.plot(x, y2, '-r')
plt.show()

# Press ctrl + enter several times to show how different sets of random numbers change the chart

```


```python
# Problem: Write five python commands to extract:
# top row
# bottom row
# right-most column
# left-most column
# upper-right 3x3 matrix

import numpy as np
np.random.seed(0)
M = np.random.randint(0,10,[5,5])
print(M)

```

    [[5 0 3 3 7]
     [9 3 5 2 4]
     [7 6 8 8 1]
     [6 7 7 8 1]
     [5 9 8 9 4]]



```python
# Answer
# Top row
print(M[0,:]) # or simply M[0]

# Bottom row
print(M[-1,:])

# Right-most column
print(M[:,-1])

# Left-most column
print(M[:,0])

# upper-right 3x3 matrix
print(M[0:3,2:]) # common mistake is to do M[0:2, 2:-1]

# Read '2:' as all columns (or rows) from index 2 to the end (all the remaining, to the end)
```

    [5 0 3 3 7]
    [5 9 8 9 4]
    [7 4 1 1 4]
    [5 9 7 6 5]
    [[3 3 7]
     [5 2 4]
     [8 8 1]]

