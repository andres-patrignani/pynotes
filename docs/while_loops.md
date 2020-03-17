# While loops

Similar to a `for loop`, a `while loop` is intended to iterate over a specific set of lines of code. However, the need for a `while loop` might not be as clear to new programmers as with `for loops`. The difference is in that a `while loop` runs indefinetely until a certain condition is met, which means that in some cases the code could run forever if the condition is never met. In a `while loop` the interpreter is constatnly running the code and checking to whether a specific condition is met or not. If not, then the code runs one more time.

The structure of the `while loop` is simple and its syntax in Python is as follows:

```python
while condition:
    code goes here
```

For example,

```python
A = 0
while A < 3:
    A += 1
    print(A)
```

In the code above we initialize a variable `A` with a value of zero. We then create a `while loop` that will iterate over the code. Note that there is no index, as with the `for loop`. The `while loop` is constatnly checking the condition defined at the top in each iteration. This means that within the code it should be possible to turn the conditonal statement from `True` to `False`. If at some point the condition is no longer `True`, then the loop will break and the Python will continue with other lines (if any) after the `while loop`.

>In Python empty variables are considered `False`, and non-empty variables are considered `True`. We can use this fact to succintly implement a condition in a `while loop`. For instance: `while len(a) > 0:` could be replaced by `while a:`


## A simple while loop

This trivial case will only print values 0, 1, and 2. As soon as the fourth iteration starts the condition is no longet met and the while loop will break.


```python
A = 0
while A < 3:
    print(A)
    A += 1
```

    0
    1
    2


Using these concepts we can create a little game that records a list of *N* elements and prints all the entries at the end. Note that the last line is outside the `while loop`, which means that will only print once the while loop is over. If we put the last line inside the `while loop` the list will print every time we enter something.

## Guess the number

In this example we will create a game that is about guessing a random number selected by the computer. The game ends once we correctly guess the number, otherwise the computer will give us another chance to guess.


```python
# Guess the number
import random

# Set variable to true in order to get started with the game
min_value = 1
max_value = 10
computer_number = random.randint(min_value,max_value)
user_guess = ''

while computer_number != user_guess:

    user_guess = int(input('Guess my number?'))
    
    if user_guess != computer_number:
        print('Try again')
        
    else:
        print('You guessed it! The number was: ' + str(computer_number))
        guess = False # Turn to false in order to stop the game

```

    Guess my number? 1


    Try again


    Guess my number? 2


    You guessed it! The number was: 2


## List of lab supplies

In the next exercise we will create a simple code that records items into a list, similar to a shopping list. The way we stop the program execution in this case is by writing the keyword `done`, which will terminate the while loop. Otherwise the program will keep asking wfor more items.


```python
# Grocery store list maker

# Initialize empty list
supplies_list = []

# Initialize empty item
#item = ''

while True: #item != 'done':
    
    # Request user input
    item_number = str(len(supplies_list) + 1)
    item = input('Add lab supply:')
    
    # If the user inputs 'done', then the while loop will print the list and then break.
    if item.lower() == 'done':
        print('You need to buy: ')
        print(supplies_list)
        break
    
    # Else we append the item to the list of supplies
    else:
        supplies_list.append(item) # Else, append item to list
    
    
```

    Add lab supply: Filter paper
    Add lab supply: Napkins
    Add lab supply: Erlenmeyer flask
    Add lab supply: done


    You need to buy: 
    ['Filter paper', 'Napkins', 'Erlenmeyer flask']

