# Random Plot Generator

One of the most common statistical operations in research projects is the randomization of treatments. This statistical procedure spans multiple disciplines, from experimental plots in agriculture to Petri dishes in microbiology and it's an inherent step of the scientific method to ensure that overall responses are due to treatments instead of background noise from the environment. The idea is to ensure that differences are only due to treatment effects and not due to an extra factor.

There multiple experimental designs, but the two simplest and most popular are complete randomized design and randomized complete block design. 

**Complete randomized design**: treatments are assigned completely at random so that each experimental unit has the same chance of receiving any one treatment. This design is only useful for homogeneous conditions (Jayaraman, 1984). 

In the complete randomized design it should be possible for the same treatment to be more than once within the same replication. Remember here we assume homogeneous conditions and we are not blocking, so any treatment can be assigned to any experimental unit.

**Randomized complete block**: The purpose of blocking is to reduce the experimental error by eliminating the contribution of known sources of variation among the experimental units. For example, terrain slope, temperature gradient in a greenhouse, light conditions, etc. (Jayaraman, 1984).

In the randomized complete block design, each treatment label must be present only once within the block.

The goal of this exercise is to create a Python script that generates random plot labels given the list of treatments and number of replications for both a complete randomized design and a randomized complete block design.



```python
# Import modules
import random

```


```python
# Define treatments
tmt = ["N0", "N25", "N50", "N100", "N200"] # Nitrogen levels in kg of N per hectare
reps = 4

```

## Complete Randomized Design

In this case treatments may appear within the same replication more than once. If we are assuming that the background environment (e.g. atmosphere, light conditions, soil) is homogeneous, then there is nothing to worry about having two treatments next to each other in the same replication. We simply want to have several replicates to account for random errors.


```python
# Randomized complete block
random.seed('default')
for i in range(0, reps):
    plot_labels = random.sample(tmt, len(tmt))
    print('Replication', i+1, ':', plot_labels)
    
```

    Replication 1 : ['N50', 'N200', 'N100', 'N25', 'N0']
    Replication 2 : ['N25', 'N200', 'N100', 'N0', 'N50']
    Replication 3 : ['N0', 'N25', 'N100', 'N200', 'N50']
    Replication 4 : ['N100', 'N200', 'N50', 'N0', 'N25']


## Complete Randomized Block

Note that in the previous experimental layout the treatments do not repeat within each replication. This is because all treatments need to be present in each block, so that all the treatments are exposed to the same characteristic conditions of each block.



```python
# Complete randomized
random.seed('default')

step = len(tmt)
plots = tmt*reps
tmts = random.sample(plots, len(plots))
counter = 0

for i in range(0, len(tmts), step):
    counter += 1
    plot_labels = tmts[i:i+step]
    print('Replication', counter, ':', plot_labels)

```

    Replication 1 : ['N100', 'N0', 'N50', 'N200', 'N50']
    Replication 2 : ['N100', 'N25', 'N0', 'N0', 'N0']
    Replication 3 : ['N25', 'N100', 'N100', 'N50', 'N25']
    Replication 4 : ['N200', 'N200', 'N25', 'N50', 'N200']


## References

Jayaraman, K., 1984. FORSPA-FAO Publication A Statistical Manual for forestry Research (No. Fe 25). FAO,. http://www.fao.org/3/X6831E/X6831E07.htm

