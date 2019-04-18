
# While loops

While loops are similar to for loops, however, the need for while loops might not be clear to new programmers. The difference between a for loop and a while loop is that the latter runs until a certain condition is met, which means that it could run indefinitely if the condition is never met. In a while loop the interpreter is constatnly running the code and checking for the condition of the loop. While loops are great for creating games.

The structure fo the `while` loop is simple and its syntax in Python is as follows:

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

In the code above we initialize a variable `A` with a value of zero. We then create a `while loop` that will iterate the code. Note that there is no index, as with the `for loop`. The `while loop` is constatnly checking the condition defined at the top in each iteration. If at some point the condition is no longer `True`, then the loop will break and the Python will continue with other lines (if any).


## A simple `while` loop

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

## A shopping list

In the next exercise we will create a simple code that records items into a list, similar to a shopping list. The way we stop the program execution in this case is by writing the keyword `done`, which will terminate the while loop. Otherwise the program will keep asking wfor more items.


```python
# Grocery store list maker

# Initialize empty list
market_list = []

# Initialize exit word
input_item = ''

while input_item != 'done':
    
    # Request user input
    input_item = input('Write something:')
    
    # Filter user input item to avoid storing 'done'
    # If the user inputs 'done', then the interpreter will skip the appending
    # and in the next iteration the process will stop since the conditional in the
    # while statement will no longer be true.
    if input_item != 'done':
        market_list.append(input_item) # Else, append item to list
    
    
print('You need to buy: ')
print(market_list)
```

    Write something: kiwi
    Write something: carrot
    Write something: done


    You need to buy: 
    ['kiwi', 'carrot']


## Guess the number

In this example we will create a game that is about guessing a random number selected by the computer. The game ends once we correctly guess the number, otherwise the computer will give us another chance to guess.


```python
# Guess the number
import random

# Set variable to true in order to get started with the game
min_value = 1
max_value = 10
computer_number = random.randint(min_value,max_value)

while computer_number != user_guess:

    user_guess = int(input('Guess my number?'))
    
    if user_guess != computer_number:
        print('Try again')
        
    else:
        print('You guessed it! The number was: ' + str(computer_number))
        guess = False # Turn to false in order to stop the game

```

    Guess my number? 2


    Try again


    Guess my number? 1


    You guessed it! The number was: 1

