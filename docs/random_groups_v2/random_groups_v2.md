
# Random group generator v2

This script groups a set of students into n groups with a maximum of k randomly selected members per group.


```python
import numpy as np
```


```python
names_list = [
'Albert Einstein',
'Hendrik Lorentz',
'Enrico Fermi',
'Peter Higgs',
'Richard Phillips Feynman',
'Niels Bohr',
'Antoine Henri Becquerel',
'Gustav Hertz',
'Ernest Lawrence',
'Wilhelm Conrad Röntgen',
'Paul Dirac'
]

```


```python
# Number of students

kmembers = 3
N = len(names_list)
ngroups = np.ceil(N/kmembers)
groups = []

# Generate array of groups (as integers), with a maximum number of kmembers per group
for i in range(int(ngroups)):
    groups = np.append(groups,np.ones(kmembers)*i+1).astype(int)

# Shuffle the group numbers
np.random.shuffle(names_list)

for i in range(0,len(names_list)):
    print('Group: ' + str(int(groups[i])) + ': ' + names_list[i])

```

    Group: 1: Paul Dirac
    Group: 1: Richard Phillips Feynman
    Group: 1: Wilhelm Conrad Röntgen
    Group: 2: Enrico Fermi
    Group: 2: Hendrik Lorentz
    Group: 2: Ernest Lawrence
    Group: 3: Niels Bohr
    Group: 3: Gustav Hertz
    Group: 3: Antoine Henri Becquerel
    Group: 4: Albert Einstein
    Group: 4: Peter Higgs

