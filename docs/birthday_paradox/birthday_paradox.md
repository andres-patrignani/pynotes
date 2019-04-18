
# Birthday Paradox

Imagine that you are in a room with 22 other people (23 total including you), what are the chances that two or more people share the same birthday out of 365 days in a year? The surprising answer is 50%. This somewhat unexpected value is due to the counter intuitive nature of combinations, reason why this problem is called a 'paradox', it seems almost impossible that such a high probability value is true. Solving this type of problems is within the realms of combinatorial analysis, and in this challenge I'm asking you to prove this paradox is true using Python.

At first it might seem an intimidating problem beyond your area of expertise, but the solution is rather simple once you internalize the problem. I suggest sketching and playing with trivial scenarios.

Here I'm looking for a script that returns the probability of two people sharing the same birthday within a group of 23 people. Note that in this problem we only care about the particular case in which we have 23 people. As you can imagine, in some groups of 23 randomly selected people from a population there will be no people sharing a birthday. So, the idea is to account for different and possible subgroups by repeating the analysis many times (hundreds or thousands). Somehow you will need to keep track of whether there was a match or not per group of 23 people, so that you can compute the probability over all trials. So, the probability of having matching birthdays in a group of 23 people is defined as:

$$P(x) \approx \frac{N_x}{N_T}$$

$N_x$: Trials with people matching birthdays (number of favorable outcomes)

$N_T$: Total number of trials (total number of outcomes)

In some subsets of 23 birthdays there might be more than two people sharing a birthday. These will be rare and you can treat all trials that have a matching birthday as a favorable case.

## Rules

- Birthdays must be equally distributed along the year (equal probability of any birthday along the year). We describe this relationship using even distributions.
- Work with days of the year. Assume days from 1 to 365 (no leap years)
- I suggest the use the Numpy module when possible.
- No need for tracking and/or reporting when the birthdays occurred.

## Example

I wrote my script so that it outputs a single probability value (e.g. 0.483). Each run involves many many trials. Below I report the result of several runs of my code to show you that the answer will not be the same every time, but overall the answers are around 0.5 (50% chance). This is because the exact probability value can only be achieved by running infinite trials.

    0.483, 0.507, 0.492, 0.515, 0.512, 0.49, 0.504
    

## Interesting reading and applications

For more infromation about the birthday paradox you can visit some of the links below:

Link 1: <https://www.scientificamerican.com/article/bring-science-home-probability-birthday-paradox/>

Link 2: <https://betterexplained.com/articles/understanding-the-birthday-paradox/>



```python
import numpy as np

# Define inputs
n_groups = 1000  # Increase this value to get a more accurate approximation (i.e. more test groups)
k_members = 23

# Create random days of the year (DOY) from a uniform distribution
birthdays = np.random.randint(1,365,[n_groups,k_members])

# Find the number of unique days of the year in each group
count_match = 0
for i in range(n_groups):
    unique_birthdays = np.unique(birthdays[i]) # Array with unique DOYs
    if unique_birthdays.size != k_members:     # If unique_birthdays < k_memebers, there was a match
        count_match += 1                       # Count birthday matches

prob = count_match/n_groups # Favorable/total scenarios
print(prob)
```

    0.504

