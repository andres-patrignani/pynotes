# Group with the Least Variance

While growing corn plants for an experiment I noted that despite using seeds of the same size, similar vigor, and burying them at the same depth was still causing some differences in plant size soon after germination. I was careful with the amount of water applied, fertilizer application, room temperature, and light distribution. I tried to controlled everything, but yet there was some variability. Of course, despite trying to control many variables to the best of my abilities (and instruments available), some variables were not under my control. The mositure level right around the seed, room ventilation, and the biochemistry of the seed itself despite all having similar sizes.

So, I decided to grow few extra plants. I needed twelve, but I seeded about 20, with the goal that at the start of the experiment I would only select the twelve that present the least variance in terms of plant height (I guess with modern technology we could be talking about volume of above-ground plant).

Below is a short code that returns the set of $n$ plants that exhibit the least variance. We will import a plotting library just to visualize the variance of all the groups, but certainly this is not needed if you are only interested in *the* group with the lowest variance (or perhaps highest variance depending on your application).



```python
# Import modules
import numpy as np
from itertools import combinations  

```


```python
# Load some sample data
sample_data = np.array([1,1,2,1.2,3,2,3,6,7,9])

```


```python
# Define function
def groupleastvar(data,k):
    """Function that finds the k members of a group with the minimum variance

    Inputs
        -data:  must be a column or row vector of the variable of interest.
                This function can handle NaN values.
        -k:     positive integer that denotes the members per group.
    
    Outputs
        -comb_min:  list with the position of the group with the least variance. 
                    It has a length of k elements
    """

    combs = list(combinations(range(len(data)), k))

    V = np.array([])
    for group in combs:
        group = list(group)
        group_var = np.nanvar(data[group])
        V = np.append(V, group_var)

    # Select group with the lowest variance
    idx_min = np.argmin(V)
    comb_min = list(combs[idx_min])
    return comb_min

```


```python
# Call the function with a dataset
k = 5
comb_min = groupleastvar(data,k)
print("The",k,"members with the lowest variance are:",comb_min,"and their values are:",data[comb_min])

```

    The 5 members with the lowest variance are: [0, 1, 2, 3, 5] and their values are: [1.  1.  2.  1.2 2. ]


To obtain the actual values rather the indices, the following line of code should work:
```python
combs = np.array(list(combinations(data, k)))
```

In our case we needed the index, since we want to go back and retreive the group with the lowest variance. From the practical point of view, we need to be able to identify the *k* members that have the lowest variance, and indices are more useful that the actual value for this operation.

An extension of this exercise is to use boolean indexing to return the *q* groups with the lowest variance as determined by the 5th percentile.

