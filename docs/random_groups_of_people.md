# Random group generator

Create a Python script that generates a random list of individuals organized in `N` groups of `K` members.
 
The goal is to write a script that generates the specified number of groups with the specified number of random members.



```python
# Import modules
import random
import pprint

```


```python
# List of names
names_list = [
    'Albert Einstein',
    'Hendrik Lorentz',
    'Enrico Fermi',
    'Peter Higgs',
    'Richard Feynman',
    'Niels Bohr',
    'Antoine Becquerel',
    'Gustav Hertz',
    'Ernest Lawrence',
    'Wilhelm Röntgen',
    'Paul Dirac',
    'Marie Curie',
    'Max Planck',
    'Erwin Schrödinger',
    'Hideki Yukawa'
]

```


```python
# Set parameters
n_groups = 3  # Define number of groups
k_members = 4 # Define number of members per group
total_people = n_groups*k_members

if total_people > len(names_list):
    print("There are fewer people in the database than those requested")
    
if total_people == 0:
    print('Number of groups and number of memebrs per group cannot be zero')
    
```

## Solution using lists

This solution simply selects a random name, removes the name from the list, and prints the name. We use the individual name to remove it from the list using the `remove()` method.


```python
for i in range(0,n_groups):
    for j in range(0,k_members):
        rand_index = random.randint(0,len(names_list)-1) # Need to add -1 to avoid indexing out of range
        rand_person = names_list[rand_index]
        print('Group ' + str(i + 1) + ':', rand_person)
        names_list.remove(rand_person)
            
```

    Group 1: Wilhelm Röntgen
    Group 1: Albert Einstein
    Group 1: Erwin Schrödinger
    Group 1: Peter Higgs
    Group 2: Max Planck
    Group 2: Hideki Yukawa
    Group 2: Ernest Lawrence
    Group 2: Hendrik Lorentz
    Group 3: Niels Bohr
    Group 3: Gustav Hertz
    Group 3: Marie Curie
    Group 3: Enrico Fermi


## Solution using a dictionary

This solution is similar to the prvious solution, but it stores the names into a dictionary and makes use of the pretty print module for a nicer display. In this alternative we also remove a specific individual by its corresponding index using the `pop()` method.


```python
result = {} # Initialize an empty dictionary

for i in range(0,n_groups):
    group_name = 'G' + str(i+1)
    group_list = []
    for j in range(0,k_members):
        rand_index = random.randint(0,len(names_list)-1) # Need to add -1 to avoid indexing out of range
        rand_person = names_list[rand_index].split(',')[0]
        group_list.append(rand_person)
        names_list.pop(rand_index)

    result[group_name] = group_list
        
pprint.pprint(result)

```

    {'G1': ['Peter Higgs', 'Max Planck', 'Albert Einstein', 'Richard Feynman'],
     'G2': ['Enrico Fermi',
            'Wilhelm Röntgen',
            'Erwin Schrödinger',
            'Hideki Yukawa'],
     'G3': ['Niels Bohr', 'Marie Curie', 'Gustav Hertz', 'Paul Dirac']}


## Solution using sample method

Uses sampling without replacement to select all the names at once.


```python
random_names = random.sample(names_list, k=total_people)

# Print names and assign a group
counter = 0  # Initialize the counter
for i in range(1,n_groups+1):
    for j in range(1,k_members+1):
        print('Group',i,':',random_names[counter])
        counter = counter + 1  # Add one unit before starting next iteration

```

    Group 1 : Ernest Lawrence
    Group 1 : Albert Einstein
    Group 1 : Hendrik Lorentz
    Group 1 : Niels Bohr
    Group 2 : Richard Feynman
    Group 2 : Marie Curie
    Group 2 : Enrico Fermi
    Group 2 : Erwin Schrödinger
    Group 3 : Peter Higgs
    Group 3 : Paul Dirac
    Group 3 : Wilhelm Röntgen
    Group 3 : Max Planck


## Solution using Numpy

This script groups a set of people into n groups with a maximum of k randomly selected members per group.


```python
import numpy as np
N = len(names_list)
groups = []

# Generate array of groups (as integers), with a maximum number 
# of k_members per group
for i in range(int(n_groups)):
    groups = np.append(groups,np.ones(k_members)*i+1).astype(int)

# Shuffle the group numbers
np.random.shuffle(names_list)

for i in range(len(groups)):
    print('Group:',groups[i],names_list[i])
```

    Group: 1 Gustav Hertz
    Group: 1 Hendrik Lorentz
    Group: 1 Marie Curie
    Group: 1 Wilhelm Röntgen
    Group: 2 Max Planck
    Group: 2 Albert Einstein
    Group: 2 Richard Feynman
    Group: 2 Erwin Schrödinger
    Group: 3 Peter Higgs
    Group: 3 Paul Dirac
    Group: 3 Niels Bohr
    Group: 3 Hideki Yukawa

