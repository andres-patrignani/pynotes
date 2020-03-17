# Monty Hall

A simple Python version of the popular game show "Let's Make a Deal". The rules of the game are simple:

- There are three doors and the doors are closed
- Behind one door there is a car and behind each of the remaining doors there is a goat
- The player selects a door
- The host reveals one of the doors containing a goat, which means that two doors remain closed (including the door selected by the player)
- The host asks the player if he/she wishes to change the selection of his/her door
- Player makes a decision whether he/she stays with the initial choice or selects the other door
- Hosts reveals what is behind the door selected by the player

The most important part of the game is when the host reveals the location of one of the goats. At this point the host is giving valuable information to the player. The player has the chance to exploit this new information by swapping the doors.



```python
import random

# Initialize game
print('Door 1: X',' | Door 2: X | ','Door 3: X')

# Randomly assign prizes to each door
prizes = ['goat','goat','car']
doors = random.sample(prizes, k=3)

# Ask player to select a door
player_door = int(input('Select a door (1, 2, or 3): '))
print('Player door:',player_door)

# Find the door of each goat (don't show this to the player)
goats = []
for i in range(3):
    if doors[i] == 'goat':
        goats.append(i+1)
        
# Identify door with a goat that is not the door selected by the player
if player_door == goats[0]:
    opened_door = goats[1]
    
elif player_door == goats[1]:
    opened_door = goats[0]
    
else:
    opened_door = goats[0]
    
# Show player one of the goats
print('I can only tell you that there is a goat behind door',opened_door)

# Identify the only door that the player could select to change his/her current door 
swapping_possibilities = [1,2,3]
swapping_possibilities.remove(player_door)
swapping_possibilities.remove(opened_door)
swap_door = input('Would you like to change door ' + str(player_door) + ' for door ' + str(swapping_possibilities[0]) + '? (y/n)')

# Update player door in case player decided to change doors
if swap_door == 'y':
    player_door = swapping_possibilities[0]
    print('Your new door is:',player_door)

# Check if player won the car or a goat
if doors[player_door-1] == 'car':
    print('You win')

else:
    print('You lose')
    
# For transparency, disclose the prizes behind all doors
print('Door 1:',doors[0],' | Door 2:',doors[1], ' | Door 3:', doors[2])
```

    Door 1: X  | Door 2: X |  Door 3: X


    Select a door (1, 2, or 3):  2


    Player door: 2
    I can only tell you that there is a goat behind door 1


    Would you like to change door 2 for door 3? (y/n) n


    You lose
    Door 1: goat  | Door 2: goat  | Door 3: car


## Comments

- Play the game several times (say 10 times) **without changing** your initial decision. Keep track of the number of times you win.

- Play the game several times (say 10 times) **this time changing** your initial decision. Keep track of the number of times you win.

- What strategy resulted most successful? Why?

- Can you modify the code above to test these two different strategies thousands of times and compute the probability of winning for each? Hint: Adapt the code as a function


To learn more about the game and why it works the way it does, you can read an excellent and concise article entitled ["The 3-Door Monty Hall Problem"](https://www.scientificamerican.com/article/the-3-door-monty-hall-problem/) by Michael Shermer and publsihed in February of 2009 in Scientific American.


## Detailed solution

We will import the random module to randomly assign the prizes to each of the three doors. 


```python
import random
```

    ['goat', 'car', 'goat']


Initialize the game by providing the user a graphical layout of the options. This step is rather irrelavant, but it helps making the game a bit easier to follow. We will print a similar statement showing the prizes behind each door at the end of the game.


```python
print('Door 1: X',' | Door 2: X | ','Door 3: X')
```

    Door 1: X  | Door 2: X |  Door 3: X


Store prizes in a list and randomly assign them to each door using the `sample` method. By setting `k=3` we ensure that all three prizes are randomly selected (selection without replacement)


```python
prizes = ['goat','goat','car']
doors = random.sample(prizes, k=3)

print(doors) # Let's print current output
```

    ['goat', 'car', 'goat']


Request a door number from the player. Note that for now we will assume the player is entering integers 1, 2, or 3. Checking if the user input is valid will take additional lines of code and during the prototyping stage we will ignore that. However, once we get the logic right and the game is up and running you can take this exercise to the next level by implementing a while loop to validate user inputs and ask again in case the input was invalid.


```python
player_door = int(input('Select a door (1, 2, or 3): '))
print('Player door:', player_door) # Just to make sure the player knows his/her door number
```

    Select a door (1, 2, or 3):  1


    Player door: 1


Knowing the logic of the game, we are approaching the step where we need to display one of the doors containing a goat. If the user selected the door with the car, then any of the doors containing a goat will work. However, if the user selected a door containing a goat behind, then we need to ensure we display the door with the other goat. We certainly don't want to reveal the prize behind the door selected by the user. We need to keep the suspense to the very end.

The way I found the solution is by keeping track of the position of the goats. I'm sure that there are more elegant solutions, but we know that the Monty Hall involves only three doors and I wanted to keep it simply. Again we are just prototyping, and different people break down problems in different ways. With experience you will learn more advanced tools or more "tricks" that will enable you to solve problems more efficiently. For now, let's keep it simple.


```python
goats = []
for i in range(3):
    if doors[i] == 'goat':
        goats.append(i+1) # Add one to match door numbers

print(goats)

```

    [1, 3]


Let's show the player a door containing one of the goats. If the user selected a door containing a goat behind we need to make sure that we diplay the door containing the other goat.


```python
# If user_door matches the position of the first goat
if player_door == goats[0]:
    opened_door = goats[1] # then show the second goat

# If user_door matches the position of the second goat
elif player_door == goats[1]:
    opened_door = goats[0] # then show the first goat

# If user_door does not match any goats (selected the car)
else:
    opened_door = goats[0] # We can display either one. I selected the first one.

print('I can only tell you that there is a goat behind door:',opened_door)
```

    I can only tell you that there is a goat behind door: 3


The user selected a door and we display a goat behind a different door. Now the user is faced with a dilemma. Should the user change his/her initial selection? Probability analysis tells us that is better to change at this point, however, you will be surprised how many people stick to their first choice. There is also a misconception that at this point there is a 50% chance, however, if you switch there is a 66% chance of winning the big prize.

At this point we have two doors closed and one door opened showing one of the goats. We need to ask the user if he/she wants to swap their initial door (one of the closed doors) for the other close door.

To solve this problem I decided to find the remaining door (neither the door selected by the user nor the door we opened to show one of the goats) by removing the door selected from the user and the opened door from the set of the three possible doors.


```python
swapping_possibilities = [1,2,3]           # Possible doors
swapping_possibilities.remove(player_door)   # remove user door
swapping_possibilities.remove(opened_door) # Remove opened door

print(swapping_possibilities)
```

    [2]


Now that we know the only possibility for the user to change his/her initial guess we can ask the following question:


```python
swap_door = input('Would you like to swap door ' + str(player_door) + ' for door ' + str(swapping_possibilities[0]) + '? (y/n)')

```

    Would you like to swap door 1 for door 2? (y/n) n


    [2]


Because we are not using any graphics to help the player, I decided to be explicit with the door numbers in the question.

If the player decided to swap doors, then we need to update the player door with the only swapping possibility. Again, I decided to print the new door number to notify the player.


```python
if swap_door == 'y':
    player_door = swapping_possibilities[0]
    print('Your new door is:',player_door)

print(player_door)
```

    1


At this point we know whether there is a car or a goat behind the door selected by the user.


```python
if doors[player_door-1] == 'car':
    print('You win')

else:
    print('You lose')

```

    You win


The last step is to disclose the prizes behind all doors to prove the user that this was a fair game and that we were not hiding anything.


```python
print('Door 1:',doors[0],' | Door 2:',doors[1], ' | Door 3:', doors[2])
```

    Door 1: goat  | Door 2: car  | Door 3: goat

