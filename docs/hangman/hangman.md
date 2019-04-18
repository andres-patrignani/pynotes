
# Challenge: Create a hangman game

Create the popular hangman game using Python. For this challenge we will leave out some of the details of the game and we will contrate on the main logical flow. So, here are some of the rules that will set the basis for the game:

- Two players (player 1 and player 2)
- Player 1 defines a word
- Player 2 guesses letters one at the time
- Single word (no sentences)
- Display location of letters in secret word using underscores



```python
# Load module to clear Jupyter Lab cell
from IPython.display import clear_output

# Welcome message
print('Welcome    O')
print('  to      -|-')
print('Hangman   / \ ')
print('')

# Request players info
player_one_name = input('Name of player 1:')
player_two_name = input('Name of player 2:')

# Get secret word from player 1
word = input(player_one_name + ' turn, write a secret word: ').upper()

# Create list with same number of underscores as the secret word
answer = ['_'] * len(word)

# Define maximum number of attempts by player 2
max_attempts = 5

# Initilize the counter for failed attempts
fail_counter = 0

while fail_counter < max_attempts:
    
    clear_output(wait=True)
    
    # Print number of attempts left
    print(player_two_name + ' has',max_attempts - fail_counter,'attempts left')
    
    # Gather letter from player 2
    guess = input(' '.join(answer) + '   ' + player_two_name + ', guess a letter: ').upper()
    
    # Initialize (in each iteration of the while loop) 
    # a dummy variable that keeps track of the correct/incorrect guessed made by player 2
    guessed_correct = False
    
    # Iterate over each letter in the word and check for matching with user guessed letter
    for i in range(len(word)):
        if word[i] == guess:
            guess_correct = True # Change dummy variable to True
            answer[i] = guess    # Replace underscore in position i with the guessed letter
    
    # Check if the dummy variable still remains false
    if guessed_correct == False:  
        fail_counter += 1  # If yes, then we add a failed attempt
    
    # If failed attempts is equal to the maximum number, then the game is over for player 2
    # and player 1 wins.
    if fail_counter == max_attempts:
        clear_output(wait=True)
        print(player_one_name + ' wins! The word was:',word)
        break # We break the while loop because the game is over if we reach this point
    
    # If the answer is equal to the secret word, then player 2 wins
    # Here we join the list before comparing it with the secret word
    if ''.join(answer) == word:
        clear_output(wait=True)
        print(player_two_name + ' wins! The word was:',word)
        break # We break the while loop because the game is over if we reach this point

```

    Computer wins! The word was: HELLO

