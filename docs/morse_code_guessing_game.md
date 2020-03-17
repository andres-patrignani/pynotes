# Morse code guessing game

Create player-versus-computer game to learn Morse code. The game has to track the score for the player and the machine. If the player gets the random symbol correct, then the player gets a point, otherwise the computer gets a point.


```python
import pandas as pd
import random

# Load lookup table
morse_table = pd.read_csv('../datasets/morse_lookup_table.txt',sep='\t',engine='python')

# Initialize scores
score_class = 0
score_machine = 0

# Game length
game_length = 3
game_iter = 0

print('Initializing game...')
print('Score machine: 0')
print('Score human: 0')

print('Loading reference Morse code lookup table...')
print(morse_table)
print('Come on class!')


```

    Initializing game...
    Score machine: 0
    Score human: 0
    Loading reference Morse code lookup table...
       character     code
    0          A       .-
    1          B     -...
    2          C     -.-.
    3          D      -..
    4          E        .
    5          F     ..-.
    6          G      --.
    7          H     ....
    8          I       ..
    9          J     .---
    10         K      -.-
    11         L     .-..
    12         M       --
    13         N       -.
    14         O      ---
    15         P     .--.
    16         Q     --.-
    17         R      .-.
    18         S      ...
    19         T        -
    20         U      ..-
    21         V     ...-
    22         W      .--
    23         X     -..-
    24         Y     -.--
    25         Z     --..
    26         0    -----
    27         1    .----
    28         2    ..---
    29         3    ...--
    30         4    ....-
    31         5    .....
    32         6    -....
    33         7    --...
    34         8    ---..
    35         9    ----.
    36         .   .-.-.-
    37         ,   --..--
    38         ?   ..--..
    39         =    -...-
    40         '   .----.
    41         /    -..-.
    42         (    -.--.
    43         )   -.--.-
    44         &    .-...
    45         ;   -.-.-.
    46         :   ---...
    47         "   .-..-.
    48         $  ...-..-
    49         !   -.-.--
    50         _  ..--.-.
    51         -   -....-
    52         @   .--.-.
    53         +    .-.-.
    Come on class!



```python
# Select a random row from lookup table
rnd_row = random.randint(0,morse_table.shape[0]+1)

# Create input question
question = 'What is the character for this Morse code  ' + morse_table.code[rnd_row] + ' :'

# Get answer from user
answer = input(question)

# Check user's answer
if answer.upper() == morse_table.character[rnd_row]:
    score_class += 1
    print('Correct')
else:
    score_machine += 1
    print('Incorrect. The correct answer is:', morse_table.character[rnd_row])

# Print cumulative scores
print('Score machine: {score_machine}'.format(score_machine=score_machine))
print('Score class: {score_class}'.format(score_class=score_class))

```

    What is the character for this Morse code  ...-..- : f


    Incorrect. The correct answer is: $
    Score machine: 1
    Score class: 0



```python
### import pandas as pd
import random

# Load lookup table
morse_table = pd.read_csv('../datasets/morse_lookup_table.txt',sep='\t',engine='python')

# Initialize scores
score_class = 0
score_machine = 0

# Game length
game_length = 3
game_iter = 0

print('Initializing game...')
print('Score machine: 0')
print('Score human: 0')

print('Loading reference Morse code lookup table...')
print(morse_table)
print('Come on class!')

while game_iter < game_length:
    # Select a random row from lookup table
    rnd_row = random.randint(0,morse_table.shape[0]+1)

    # Create input question
    question = 'What is the character for this Morse code  ' + morse_table.code[rnd_row] + ' :'

    # Get answer from user
    answer = input(question)

    # Check user's answer
    if answer.upper() == morse_table.character[rnd_row]:
        score_class += 1
        print('Correct')
    else:
        score_machine += 1
        print('Incorrect. The correct answer is:', morse_table.character[rnd_row])

    # Print cumulative scores
    print('Score machine: {score_machine}'.format(score_machine=score_machine))
    print('Score class: {score_class}'.format(score_class=score_class))
    print('')
    
    if game_iter+1 == game_length:
        if score_class == score_machine:
            print('This is tie')
        elif score_class > score_machine:
            print('Class wins!')
        else:
            print('Computer wins!')

    game_iter += 1
```
