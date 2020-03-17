# Birthday Paradox

Imagine that you are in a room with 22 other people (23 total including you), what are the chances that two or more people share the same birthday out of 365 days in a year? The surprising answer is 50%. This somewhat unexpected value is due to the counterintuitive nature of combinations, reason why this problem is called a 'paradox', it seems almost impossible that such a high probability value is true. Solving this type of problems is within the realms of combinatorial analysis, and in this exercise we will prove that this paradox is true using Python.

At first it might seem an intimidating problem beyond your area of expertise, but the solution is rather simple once you internalize the problem. 

As you can imagine, in some groups of 23 randomly selected people from a population there will be no people sharing a birthday. So, the idea is to account for different subgroups by repeating the analysis many times (hundreds or thousands). Somehow we will need to keep track of whether there was a match or not per group of 23 people that we test, so that we can compute the probability over all trials. The probability of having matching birthdays in a group of 23 people is formally defined as:

$$P(x) \approx \frac{N_x}{N_T}$$

$N_x$: Trials with people matching birthdays (number of favorable outcomes)

$N_T$: Total number of trials (total number of outcomes)

In some subsets of 23 birthdays there might be more than two people sharing a birthday. These will be rare and we will treat all trials that have a matching birthday as a favorable case.

Before we start solving the problem we need to make clear some rules of the problem:

- Birthdays must have equal probability along the year. We describe this relationship using even distributions.

- Work with days of the year. Assume days from 1 to 365 (for simplicity we will ignore leap years).

- No need for tracking and/or reporting when the birthdays occurred. We should focus on whether there is a match or not among 23 random birthdays.

- Our results will likely oscillate around a probability value of 0.5. Due to the random nature of the process, the answer will not be exactly a probability of 0.5.


>Run the code below multiple times to observe whether the resulting probability value oscillates around 0.5.


```python
# Import module
import numpy as np

# Define inputs
n_groups = 1000  # Increase to improve approximation (i.e. more test groups)
k_members = 23

# Create random days of the year (DOY) from a uniform distribution
birthdays = np.random.randint(1, 365, [n_groups,k_members])

# Find the number of unique days of the year in each group
count_match = 0
for i in range(n_groups):
    unique_birthdays = np.unique(birthdays[i]) # Array with unique DOYs
    
    # If unique_birthdays < k_memebers, there was a match and we should record it.
    if unique_birthdays.size != k_members:     
        count_match += 1                       # Count birthday matches

prob = count_match/n_groups # Favorable/total scenarios
print(prob)

```

    0.52


## References

Flajolet, P., Gardy, D. and Thimonier, L., 1992. Birthday paradox, coupon collectors, caching algorithms and self-organizing search. Discrete Applied Mathematics, 39(3), pp.207-229.

Scientific American: <https://www.scientificamerican.com/article/bring-science-home-probability-birthday-paradox/>

Better explained: <https://betterexplained.com/articles/understanding-the-birthday-paradox/>
