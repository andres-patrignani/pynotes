
# Random module

Module tutorial about generating single random numbers, multiple random numbers from a specified range, items from lists, or numbers from specific distributions.

Official documentation: https://docs.python.org/3/library/random.html


```python
import random
```


```python
# A random float between 0 and 1 from a uniform distribution (each number has the same probability). 
random.random()
```




    0.38004768437208425




```python
# Random float between 0 and 100
random.random()*100
```




    32.95480829051052




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
random.randrange(120)  # from 0 to 119
```




    82




```python
# Random integers
random.randint(0,5) # A number from 0 to 5. Run multiple times to see it in action
```




    5




```python
# Random numbers from user-specified range using the randrange function
random.randrange(120,150)
```




    148




```python
# Random numbers from user-specified range and STEP using the randrange function
step = 10
random.randrange(120,250,step)
```




    150




```python
# Random number from normal distribution
mu, sigma = 5, 3
random.gauss(mu, sigma)

```




    8.366747126985722




```python
# Random number from gamma distribution
alpha = 0.1
beta = 3
random.gammavariate(alpha, beta)
```




    0.17920433762068266




```python
# Get random items from a list

# First define a list
#array = [1,2,3,4,5]
array = ['ATTA','AGTT','AAAC','CCTG','GGAT']

# Single random element from list
rnd_single_val = random.choice(array)
print(rnd_single_val)

# Multiple elements chosen from the population WITH replacement
rnd_replacement = random.choices(array, k=4)
print(rnd_replacement)

# Multiple elements chosen from the population WITHOUT replacement
rnd_no_replacement = random.sample(array, k=4)
print(rnd_no_replacement)

# Run several times. First block of code will result in repeated values. 
# Second block of code will result in randomly organized, but unique values from the array
```

    CCTG
    ['GGAT', 'GGAT', 'ATTA', 'AGTT']
    ['GGAT', 'CCTG', 'AGTT', 'ATTA']



```python
random.seed(a=None) # Same as not defining the seed at all
random.randint(0, 5)
```




    3




```python
random.seed(a=244)
random.randint(a=0, b=5)
```




    5


