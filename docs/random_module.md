# Random module

The `random` module from the standard Python library provides useful methods for choosing samples with or without replacement, drawing values from common distributions, generating random numbers within a specified range, and specifying random seeds for reproducibility.



```python
# Import module
import random
```

## Random number from a specific range

By default the `random()` method will return a random float in the range 0 to 1 from a uniform distribution. In the uniform distribution each number has the same probability of being picked.

In the examples below you can run the cell multiple times to generate different random numbers.



```python
random.random()
```




    0.07532936865064432




```python
# Random float between 0 and 100
random.random()*100
```




    32.95480829051052




```python
# Random integers
random.randint(0,5) # Integer from 0 to 5
```




    5




```python
# A random float within a specific range
min_val = 20
max_val = 250
rnd_val = (max_val - min_val)*random.random() + min_val
print(rnd_val)
```

    168.1272141045335



```python
# Single random integer
random.randrange(120)  # Integer from 0 to 119
```




    82




```python
# Random number from user-specified range
random.randrange(120,150)
```




    133




```python
# Random numbers from user-specified range and step
step = 10
random.randrange(120,250,step)
```




    240




```python
# Random number from normal distribution
mu, sigma = 5, 3 # Both variables defined at the same time
random.gauss(mu, sigma)

```




    4.565407149663365




```python
# Random number from gamma distribution
alpha = 0.1
beta = 3
random.gammavariate(alpha, beta)

```




    0.0035038912626613175



## Random elemnts from lists

In many cases we are not interested in generating random numbers *per se*, but in drawing a random element from a list of elements. In this case the methods `choice()`, `choices()`, and `sample()` can result very useful.



```python
 # Define a list of five elements for this section
array = ['ATTA','AGTT','AAAC','CCTG','GGAT']

```


```python
# Single random element
random.choice(array)

```




    'ATTA'




```python
# Multiple elements WITH replacement
random.choices(array, k=4) 

```




    ['AAAC', 'AAAC', 'AGTT', 'GGAT']



>Note: When drawing elemets **WITH replacement** the elements in the resulting list may appear more than one time. Run the previous code cell few times to see this.


```python
# Multiple elements WITHOUT replacement
random.sample(array, k=4)
#random.sample(array, k=len(array)) # To shuffle all elements
```




    ['CCTG', 'AAAC', 'GGAT', 'AGTT']



>Note: When drawing elements **WITHOUT replacement** all the elements in the resulting list will be unique. Set `k=len(array)` to shuffle all the elements in the list. Also note that `k` must be equal or lower than the number of elements in the list because we are drawing elements without replacement.

## Setting up a random seed

The random module actually generates pseudo-random numbers that have properties of random numbers. To ensure that results from random processes are reproducible it is possible to set up a random seed, which is basically a number that initializes the sequence in the random number generator.

The Mersenne-Twister is perhaps one of the most popular pseudo-random generator algorithms (Matsumoto and Nishimura, 1998). For a simpler pseudo-random generator you can also read the manuscript by Payne et al 1969.

If you run the example below you should obtain the same result that I obtained. It will also remain constant no matter how many times you run the cell.

>Random seeds in Python are exclusive of the cell in which the seed was defined. So if you run the code `random.randint(a=0, b=10)` in a different cell you won't obtain the same number. To do that you need to specify the random seed again.


```python
random.seed(10)
random.randint(a=0, b=10)
```




    9




```python
# Seed no longer applies
# You will otain different values each time
random.randint(a=0, b=10)
```




    0



## References

Matsumoto, M. and Nishimura, T., 1998. Mersenne twister: a 623-dimensionally equidistributed uniform pseudo-random number generator. ACM Transactions on Modeling and Computer Simulation (TOMACS), 8(1), pp.3-30.

Random module official documentation: https://docs.python.org/3/library/random.html

Payne, W.H., Rabung, J.R. and Bogyo, T.P., 1969. Coding the Lehmer pseudo-random number generator. Communications of the ACM, 12(2), pp.85-86.
