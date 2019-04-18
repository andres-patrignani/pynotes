
# Morse code decoder

The goal of this exercise is to decode a message written using Morse code. Morse code consists of dits and dahs to represent alphanumeric characters. Although there are rules for the spacing between letters and words, in this case there is a single space between individual Morse codes and three spaces between words.

A lookup table of Morse codes and characters is available in the Dataset folder. We will use the Pandas library to read the lookup table and also to identify the different codes and match them with the right letter of the alphabet. The message that we will decode is: 

    .- -..   .- ... - .-. .-   .--. . .-.   .- ... .--. . .-. .-

"Ad Astra per Aspera", which is a Latin expression that means "To the Stars through Difficulties" and is the motto of the state of Kansas.

Our first approach will be simple and will ignore some technical issues (e.g. spacing between words). Our goal is to decode the message and to print it in a human readable format. If we can achieve this, then we can spend more time embellishing the output and polishing technical aspects.

>The solution assumes that the encoded message is a single long string of Morse codes without breaklines.

## Step 1: Load a Morse code lookup table

Load lookup table of Morse codes and alphabet characters using the Pandas library. The `C` engine (default) is faster while the `python` engine is currently more feature-complete.


```python
import pandas as pd

morse_table = pd.read_csv('/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/morse_lookup_table.txt',
                          sep='\t',
                          engine='python')

```

## Step 2: Identify the individual Morse codes

This step is essential to decode the message. The fundamental unit of the lookup table is a single code or character, so we need to work at this level to identify the matching Morse code in the DataFrame and then retrieve the corresponding character.

We will use the `.split()` method of strings to accomplish this.


```python
encoded_string = ".- -..   .- ... - .-. .-   .--. . .-.   .- ... .--. . .-. .-"
list_morse_codes = encoded_string.split(' ') # Splits at (and removes) the space.
print(list_morse_codes) # Let's see the result of the previous line

```

    ['.-', '-..', '', '', '.-', '...', '-', '.-.', '.-', '', '', '.--.', '.', '.-.', '', '', '.-', '...', '.--.', '.', '.-.', '.-']


Note that the method removes the spaces but still leaves some empty strings. We will not worry about this right now. In fact, the `.split()` method removed all the spaces between codes so we can use the remaining empty strings to separate words. Again, maybe not ideal, but this is something we can fix later. **We made great progress splitting up the encoded message into individual Morse codes.**

## Step 3: Match codes and characters
Now that we have the different Morse codes separated into individual elements of a list we need to find a way of matching each code in the encoded string with the Morse code in the lookup, and then we will use the position of the matched Morse code to retrieve the corresponding character. This is similar to what we would do if we solve this problem by hand, it's just that we normally overlook tiny crucial steps. For instance, you may say: *sure after finding the matching Morse code we need to select the character located in the same matching row*, but we need to tell the computer all these tiny decisions, so that it can replicate what we do by hand using pencil and paper.

We will start testing our approach with a single code. At this point it's common for beginners to start writing loops and if statements without ensuring that the task that will be iterated works as expected. A simple test using a trivial case can save substantial amount of time.


```python
morse_table.code == list_morse_codes[0] # Test matching the first Morse code, which is an 'A'
```




    0      True
    1     False
    2     False
    3     False
    4     False
    5     False
    6     False
    7     False
    8     False
    9     False
    10    False
    11    False
    12    False
    13    False
    14    False
    15    False
    16    False
    17    False
    18    False
    19    False
    20    False
    21    False
    22    False
    23    False
    24    False
    25    False
    26    False
    27    False
    28    False
    29    False
    30    False
    31    False
    32    False
    33    False
    34    False
    35    False
    36    False
    37    False
    38    False
    39    False
    40    False
    41    False
    42    False
    43    False
    44    False
    45    False
    46    False
    47    False
    48    False
    49    False
    50    False
    51    False
    52    False
    53    False
    Name: code, dtype: bool



We obtained a boolean vector that will allow us to retrieve the matching character effortlessly. This type of boolean arrays might result confusing at first, but they are extremely useful while matching, filtering, and synthesizing data.

Now we can use this boolean vector to retrieve the corresponding character. The result will only consist of the rows that were designated as True.


```python
idx = morse_table.code == list_morse_codes[0] # store boolean into a variable called `idx`
morse_table.character[idx].values[0]
```




    'A'



Bingo! We were able to decipher the first Morse code of the secret string. Now we need to repeat the same operation for each Morse code. We will also add an `if` statement to handle the empty strings `''`. Basically everytime we hit one of these empty strings we will append a space. This will result in words separated by two spaces rather than one, but this is a small detail at this point.

>Note that if we don't handle the empty strings represented by `''` our code will crash because the look up table does not have such entry. Also, removing them from the list is not wise because we would lose track of the different words.

## Step 4: Repeat for each Morse code


```python
decoded_string = [] # initialize empty list to be populated with characters
space = " "

for code in list_morse_codes:
    
    # Handle empty strings as a consequence of the splitting
    if code == '':
        decoded_string.append(space) # Will add a space per each space between words in the original message
        
    else:
        idx = morse_table.code == code
        decoded_string.append(morse_table.character[idx].values[0])
        
print(''.join(decoded_string)) 
```

    AD  ASTRA  PER  ASPERA


## Entire code

Combining all the steps and adding few more lines of code to handle the emty strings results in the following code:


```python
# A complete and improved version of the previous code that handles strings with multiple lines.

import pandas as pd

morse_table = pd.read_csv('/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/morse_lookup_table.txt',
                          sep='\t',
                          engine='python')

encoded_string = ".- -..   .- ... - .-. .-   .--. . .-.   .- ... .--. . .-. .-"
list_morse_codes = input_string.split(' ') # It removes the spaces, so we don't have to worry about spaces between characters anymore
space = " "
decoded_string = []
preceding_space = False # Track empty spaces

for code in list_morse_codes:
    
    # Handle first empty string to separate words
    if code == '' and not preceding_space:
        decoded_string.append(space) # Will add a space per each space between words in the original message
        preceding_space = True # Convert to True because the next iteration needs to know that we already have a space
    
    # Bypass extra empty strings
    elif preceding_space:
        preceding_space = False # Restore the variable
        continue
        
    else:
        preceding_space = False # Restore the variable
        idx = morse_table.code == code
        decoded_string.append(morse_table.character[idx].values[0])
        
print(''.join(decoded_string)) 

```

    AD ASTRA PER ASPERA

