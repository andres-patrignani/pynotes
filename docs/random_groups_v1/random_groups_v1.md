
# Random group generator v1

Create a Python script that generates a random list of people organized in `N` groups of `K` members. Use the list of names in the file named `'nobel_physics.txt'`
 
## Rules

Code must create the specified number of groups with the specified number of members.
If the total number of requested people exceeds the number of names in the database, the code must print a useful message and should not run any other line of code after that.
If the number of groups or members is zero, the code must return a useful message and should not run any other line of code after that. We want to avoid running unnecessary code if we know in advance that groups or people will be equal to zero.
People in groups must be unique, which means that a person cannot be part of two groups.
Must use the modules learned in class. Other modules and advanced lines of code from online sources are not allowed.
Maximum number lines of code is 30 (including comments and white lines)
 

## Hint

>You are not required to store the group number and members of each group in a list or dictionary. It's fine if you can make the code to print the names like I show in the example below.
Combine an IF statement with nested FOR loops

>If your list contains duplicate names you could always use the set function to get unique values. For instance, given the following list: `names = ['Joe','Mark','Trent','Mark']`, then `list(set(names))` will result in: `['Trent', 'Mark', 'Joe']`
 

## Example

Given the following input variables:

```python
n_groups = 3
k_members = 4 
```

the code must print the following:

```
Group 1: Albert Einstein
Group 1: Hendrik Lorentz
Group 1: Enrico Fermi
Group 1: Peter Higgs
Group 2: Richard Phillips Feynman
Group 2: Niels Bohr
Group 2: Antoine Henri Becquerel
Group 2: Gustav Hertz
Group 3: Ernest Lawrence
Group 3: Wilhelm Conrad Röntgen
Group 3: Paul Dirac
Group 3: Erwin Schrödinger
```

Note that the actual names returned by your code will differ from mine since this is a random process. The code printed only the names selected at random from the database. Some names will not be selected and this will depend on the total number of names requested by the user.

## Skills required in this challenge

- if statements
- for loops
- random module
- range function
- appending and deleting items from a list


## Solution 1
This solution simply selects a random name, removes the name from the list, and prints the name


```python
import glob
import random

dataset_dir = '/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/'
glob.os.chdir(dataset_dir) # Navigate to Datasets directory
names_list = open('nobel_physics.txt').read().split('\n') # Load data

n_groups = 3  # Define number of groups
k_members = 4 # Define number of members per group
total_people = n_groups*k_members

if total_people > len(names_list):
    print("There are fewer people in the database than those requested")

elif total_people == 0:
    print('Number of groups and number of memebrs per group cannot be zero')
    
else:
    for i in range(0,n_groups):
        for j in range(0,k_members):
            rand_index = random.randint(0,len(names_list)-1) # Need to add -1 to avoid indexing out of range
            rand_person = names_list[rand_index].split(',')[0]
            print('Group ' + str(i + 1) + ':', rand_person)
            names_list.remove(rand_index)
            
```

    Group 1: Guglielmo Marconi



    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-1-45bc0d5eebca> in <module>()
         22             rand_person = names_list[rand_index].split(',')[0]
         23             print('Group ' + str(i + 1) + ':', rand_person)
    ---> 24             names_list.remove(rand_index)
         25 


    ValueError: list.remove(x): x not in list


## Solution 2
This solution is similar to solution 1, but it stores the names into a dcitionary and makes use of the pretty print module for a nices display.


```python
import random
import pprint

dataset_dir = '/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/'
glob.os.chdir(dataset_dir) # Navigate to Datasets directory
names_list = open('nobel_physics.txt').read().split('\n') # Load data

ngroups = 3
npeople = 4
total_people = n_groups*k_members
result = {} # Initialize an empty dictionary

if total_people > len(names_list):
    print("There are fewer people in the database than those requested")
    
elif total_people == 0:
    print('Number of groups and number of memebrs per group cannot be zero')
    
else:
    for i in range(0,ngroups):
        group_name = 'G' + str(i+1)
        group_list = []
        for j in range(0,npeople):
            rand_index = random.randint(0,len(names_list)-1) # Need to add -1 to avoid indexing out of range
            rand_person = names_list[rand_index].split(',')[0]
            group_list.append(rand_person)
            names_list.pop(rand_index)

        result[group_name] = group_list
        
pprint.pprint(result)

```

    {'G1': ['Marie Curie',
            'Niels Bohr',
            'Wilhelm Conrad Röntgen',
            'Erwin Schrödinger'],
     'G2': ['Max Planck',
            'Antoine Henri Becquerel',
            'Albert Einstein',
            'Lord Rayleigh'],
     'G3': ['Werner Heisenberg',
            'Ernest Lawrence',
            'Guglielmo Marconi',
            'Enrico Fermi']}


## Solution 3
Uses sampling without replacement to select all the names at once.


```python
# A version using random.sample
import glob
import random

# Navigate to Datasets directory
dataset_dir = '/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/'
glob.os.chdir(dataset_dir)

# Load data
names_list = open('nobel_physics.txt').read().split('\n') 

# Define inputs
n_groups = 3
k_members = 4

# Select unique random names from list
total_people = n_groups*k_members
if total_people > len(names_list):
    print("There are fewer people in the database than those requested")
elif total_people <= 0:
    print('Number of groups and number of memebrs per group cannot be zero')
else: 
    random_names = random.sample(names_list, k=total_people)

# Print names and assign a group
counter = 0  # Initialize the counter
for i in range(1,n_groups+1):
    for j in range(1,k_members+1):
        print('Group',i,':',random_names[counter])
        counter = counter + 1  # Add one unit to the counter before starting next iteration

```

    Group 1 : Wilhelm Conrad Röntgen,
    Group 1 : Albert Einstein,
    Group 1 : Hideki Yukawa,
    Group 1 : Erwin Schrödinger,
    Group 2 : Enrico Fermi,
    Group 2 : Richard Phillips Feynman
    Group 2 : Max Planck,
    Group 2 : Guglielmo Marconi,
    Group 3 : Lord Rayleigh,
    Group 3 : Gustav Hertz,
    Group 3 : Antoine Henri Becquerel,
    Group 3 : Peter Higgs,

