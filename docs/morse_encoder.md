# Morse code encoder

Use Python to encode a string into the International Morse Code system. Morse code is a character encoding scheme used in telecommunication that encodes text characters as standardized sequences of two different signal durations called dots and dashes or dits and dahs. Morse code is named for Samuel F. B. Morse, the inventor of the telegraph [1]

[1] https://www.wikiwand.com/en/Morse_code


## Auxilliary files
In the datasets folder there is a `.csv` file containing the Morse code equivalent for the most comoon characters of the English language.

## Skills
- Importing modules
- Import data using Pandas library
- Logical indexing using Pandas library
- Accessing data in Pandas dataframe
- Create lists and append elements
- Manipulation of strings (join strings, change case, multiply strings)
- For loop
- If statement

**Let's do it!** or in Morse code:

    
    `.- -..   .- ... - .-. .-   .--. . .-.   .- ... .--. . .-. .-`


## Goal

For any given string, replace each character (letter, exclamation point, comma, etc.) with its corresponding Morse code. The idea is to return a single string with the input string encoded in Morse code.

## Step 1: Break down the problem into smaller pieces

When we follow a tutorial we typically see nice and logically-organized code. However, we hardly ever write code like this. Instead, we try to break down the problem into smaller components and we test these smaller components. Then, we assemble the code, where new challenges will likely emerge. So, there are several iterations of the polishing process.

Here are few of the steps that I envisioned before writing the code:

- Search or create a lookup table of English characters and Morse codes.

- Iterate over each character of the input string

- Match a character from the input string to the list of characters in the lookup table

- Retrieve the Morse code for the matched character

- Store this code (at this point I still did not know I was going to use a list, althought it sorts of make sense)

- Repeat steps with the following character of the input string

<br/>

**Things that I ignored at this stage and that later on became important**

- Spacing between letters and words. By reading the Wikipedia page I found that there are actually rules for spacing characters and words. So I tried to implement a rough variation of them in my code.

- Join all the Morse codes in a list to form a string. I dealt with this problem once the code was working and I needed to focus on how to print the output string.

- Spaces in input string. The lookup table has Morse codes for characters, but it has no way of dealing with spaces. My first script was unable to handle spaces and was crashing even for something like `Hello world`. So, I focused on getting the right answer for just `Hello`. If I can retrive the correct Morse codes for a simple word, then it means that I'm close. Aim at accomplishing small steps and then proceed. After savoring small victories I feel motivated and engaged to resolve the next step.

## Step 2: Create a Morse code lookup table 

THe first step consists of creating a lookup table between common English characters and Morse codes. I obtained the codes from Wikipedia and saved them into a text file. The file is in the `Datasets` folder.

If you copy-paste text from a website or file, make sure to remove the any formatting. I also had to disable in TextEdit (Mac) software the "smart dashes" and "smart quotes", so that the text editor keeps "---" as three dashes instad of creating a horizontal line.

I compiled the Morse codes for a total of 52 characters and I saved them in a tab-delimeted file.

This is an example of steps that take some 20 to 30 minutes just to prepare the data. 

## Step 3: Load lookup table

Since we have a text file with two columns (character and code), it's pretty obvious that Pandas is a good alternative.

Pandas linrary also allows for logical indexing, which means that we can use a vector of Booleans to easily retrieve information from specific column cells.

To load the lookuptable into Python we need few pieces of information:

1. URL for the file
2. File delimiter
3. Parser engine to use. We will use the python engine, which is more feature-complete. The default parser will throw an error.

After loading the lookup table we will display the entire dataframe to double-check that everything is loaded as expected.

**Note**: I had no idea that Python has different parsers. I first thought that my code was crashing because I encoded something wrong in the text file and that Python was having a hard time to read my file. Much of this problem arose after I added characters such as apostrophes to my lookup table. So, I first found a thread in StackOverflow and then I went to the [Pandas official documentation](<https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html)...and there it was!, a succint but nice explanation about parser engines.


```python
import pandas as pd
```


```python
# Load lookup table
morse_table = pd.read_csv('/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/morse_lookup_table.txt',
                          sep='\t',
                          engine='python')
# Display dataframe
morse_table
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>character</th>
      <th>code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>.-</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-.-.</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>-..</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E</td>
      <td>.</td>
    </tr>
    <tr>
      <th>5</th>
      <td>F</td>
      <td>..-.</td>
    </tr>
    <tr>
      <th>6</th>
      <td>G</td>
      <td>--.</td>
    </tr>
    <tr>
      <th>7</th>
      <td>H</td>
      <td>....</td>
    </tr>
    <tr>
      <th>8</th>
      <td>I</td>
      <td>..</td>
    </tr>
    <tr>
      <th>9</th>
      <td>J</td>
      <td>.---</td>
    </tr>
    <tr>
      <th>10</th>
      <td>K</td>
      <td>-.-</td>
    </tr>
    <tr>
      <th>11</th>
      <td>L</td>
      <td>.-..</td>
    </tr>
    <tr>
      <th>12</th>
      <td>M</td>
      <td>--</td>
    </tr>
    <tr>
      <th>13</th>
      <td>N</td>
      <td>-.</td>
    </tr>
    <tr>
      <th>14</th>
      <td>O</td>
      <td>---</td>
    </tr>
    <tr>
      <th>15</th>
      <td>P</td>
      <td>.--.</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Q</td>
      <td>--.-</td>
    </tr>
    <tr>
      <th>17</th>
      <td>R</td>
      <td>.-.</td>
    </tr>
    <tr>
      <th>18</th>
      <td>S</td>
      <td>...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>T</td>
      <td>-</td>
    </tr>
    <tr>
      <th>20</th>
      <td>U</td>
      <td>..-</td>
    </tr>
    <tr>
      <th>21</th>
      <td>V</td>
      <td>...-</td>
    </tr>
    <tr>
      <th>22</th>
      <td>W</td>
      <td>.--</td>
    </tr>
    <tr>
      <th>23</th>
      <td>X</td>
      <td>-..-</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Y</td>
      <td>-.--</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Z</td>
      <td>--..</td>
    </tr>
    <tr>
      <th>26</th>
      <td>0</td>
      <td>-----</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1</td>
      <td>.----</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2</td>
      <td>..---</td>
    </tr>
    <tr>
      <th>29</th>
      <td>3</td>
      <td>...--</td>
    </tr>
    <tr>
      <th>30</th>
      <td>4</td>
      <td>....-</td>
    </tr>
    <tr>
      <th>31</th>
      <td>5</td>
      <td>.....</td>
    </tr>
    <tr>
      <th>32</th>
      <td>6</td>
      <td>-....</td>
    </tr>
    <tr>
      <th>33</th>
      <td>7</td>
      <td>--...</td>
    </tr>
    <tr>
      <th>34</th>
      <td>8</td>
      <td>---..</td>
    </tr>
    <tr>
      <th>35</th>
      <td>9</td>
      <td>----.</td>
    </tr>
    <tr>
      <th>36</th>
      <td>.</td>
      <td>.-.-.-</td>
    </tr>
    <tr>
      <th>37</th>
      <td>,</td>
      <td>--..--</td>
    </tr>
    <tr>
      <th>38</th>
      <td>?</td>
      <td>..--..</td>
    </tr>
    <tr>
      <th>39</th>
      <td>=</td>
      <td>-...-</td>
    </tr>
    <tr>
      <th>40</th>
      <td>'</td>
      <td>.----.</td>
    </tr>
    <tr>
      <th>41</th>
      <td>/</td>
      <td>-..-.</td>
    </tr>
    <tr>
      <th>42</th>
      <td>(</td>
      <td>-.--.</td>
    </tr>
    <tr>
      <th>43</th>
      <td>)</td>
      <td>-.--.-</td>
    </tr>
    <tr>
      <th>44</th>
      <td>&amp;</td>
      <td>.-...</td>
    </tr>
    <tr>
      <th>45</th>
      <td>;</td>
      <td>-.-.-.</td>
    </tr>
    <tr>
      <th>46</th>
      <td>:</td>
      <td>---...</td>
    </tr>
    <tr>
      <th>47</th>
      <td>"</td>
      <td>.-..-.</td>
    </tr>
    <tr>
      <th>48</th>
      <td>$</td>
      <td>...-..-</td>
    </tr>
    <tr>
      <th>49</th>
      <td>!</td>
      <td>-.-.--</td>
    </tr>
    <tr>
      <th>50</th>
      <td>_</td>
      <td>..--.-.</td>
    </tr>
    <tr>
      <th>51</th>
      <td>-</td>
      <td>-....-</td>
    </tr>
    <tr>
      <th>52</th>
      <td>@</td>
      <td>.--.-.</td>
    </tr>
    <tr>
      <th>53</th>
      <td>+</td>
      <td>.-.-.</td>
    </tr>
  </tbody>
</table>
</div>



## Step 4: Test the steps

Breaking down the problem into smaller pieces in step 1 does not necessarily means that we know how to code these steps. It's important that you understand the difference. If you can break down a complex problem into smaller, simpler problems you will be able to find a solution or workaround.

Below are few examples of the tests that I tried before attempting to write the Morse encoder script. Note that my tests are based on trivial examples. If the code works for the letter `A` then it will work for other letters.

### Test 1: Match a single character to an entire Pandas column of characters


```python
morse_table.character == 'A'
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
    Name: character, dtype: bool



Output is a Boolean vector, where the first value is True and the rest are False. The code successfully identifies the location of the character 'A'.

### Test 2: Retrieve the Morse code for the matched character. 
Here I decided to save the boolean vector into a variable, so that I can pass it to the Pandas column.


```python
idx = morse_table.character == 'A'   
print(morse_table.code[idx]) # I want the Morse code for the row that returned True
print(type(morse_table.code[idx])) 
```

    0    .-
    Name: code, dtype: object
    <class 'pandas.core.series.Series'>


Close, but not exactly what I expected. See the problem is that this is type pandas.series. I just want the Morse code as a string, so that I can store it or concatenate it.

### Test 2 (second attempt)
After trying few alternatives and visiting the official documentation I found that we can access the information inside the pandas.series by specifying the value.


```python
idx = morse_table.character == 'A'
print(morse_table.code[idx].values[0])
print(type(morse_table.code[idx].values[0]))
```

    .-
    <class 'str'>


Bingo! But the code above does not work for lower case characters like 'a'. So we need to learn how to do that. Fortunately, this is easy to implement in python (see next test).


```python
# Test 3: Change case of character

mystr = 'a'
print(mystr.upper())
```

    A


It works!

### Test 4: Append characters
In step 1 I realized that after finding the Morse code for a specific character I had to find a way of storing that string before I move on, otherwise I would keep iterating and overwriting my Morse codes.


```python
# Start with an empty list
output_string = []

# Append an example string (I don't even know what character this Morse code represents)
output_string.append('.-')

# Print string to see its current state
print(output_string)

# Append another random Morse code
output_string.append('.---')

# Print list
print(output_string)
```

    ['.-']
    ['.-', '.---']


It's working. The Morse codes here do not matter, the idea is to test the code that will enable us to store the codes in a list.

### Test 5: Join string list items into a single string.
If I store all the Morse codes in a list, how do I print them all together at the end? See, I want my code to return a 'translated' string.
Since the previous step worked as expected, I will use it in this test. From the Strings tutorial we learned that we can join list items as follows:


```python
print('&'.join(output_string))
```

    .-&-..& &.-&...&-&.-.&.-& &.--.&.&.-.& &.-&...&.--.&.&.-.&.-


So, if I assign a string with a single space into a new variable, I should be able to merge all the Morse codes and separate them by a single space to make it more readable. In other words, each Morse code representing a character will be separated by a blank space.


```python
separator = " "
print(separator.join(output_string))
```

    .- -..   .- ... - .-. .-   .--. . .-.   .- ... .--. . .-. .-


Then I asked myself, what happens if I add more spaces, will the resulting string of Morse codes look better or worse? There is only one way to find out. Here is a cool Python trick. It's much more transparent and readable than adding the spaces


```python
separator = " " * 3
print(separator.join(output_string))
```

## Step 5: Put the pieces together

Now that we ran several tests and that we know how to code the different parts of the problem we are ready to put the puzzle together. It will be the first try, so it's fine if it doesn't work from top to bottom. The goal here is to get at least some steps to work together.



```python
# First attempt to encode strings into Morse code

decoded_string = "Hello"
encoded_string = []
letter_sep = " " * 3

for letter in decoded_string:
    idx = morse_table.character == letter.upper()
    encoded_string.append(morse_table.code[idx].values[0])

print(letter_sep.join(output_string))
```

    ....   .   .-..   .-..   ---


The code works, but it crashes when I add a string with spaces, like "Hello world". This is because spaces are not part of the lookup table. So, we need to handle this in the code using an 'if' statement is probably the first thing that comes to mind. Let's try it


```python
# If we find a space between words then we will add a space larger than that between letters.
decoded_string = "Hello world"
encoded_string = []

letter_sep = " " * 3 # Space between letters
word_sep = " " * 5   # Space between words

for letter in decoded_string:
    if letter == " ":
        encoded_string.append(word_sep)
    else:
        idx = morse_table.character == letter.upper()
        encoded_string.append(morse_table.code[idx].values[0])

print(letter_sep.join(encoded_string))
```

    ....   .   .-..   .-..   ---           .--   ---   .-.   .-..   -..



```python
# A complete and improved version of the previous code that handles strings with multiple lines.

import pandas as pd

morse_table = pd.read_csv('/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/morse_lookup_table.txt',
                          sep='\t',
                          engine='python')

#input_string = """The programmers of tomorrow are the wizards of the future. You are going to look like you have magic powers compared to everybody else. Gabe Newell"""

decoded_string = "Let's do it!"
encoded_string = []

letter_sep = " " * 1
word_sep = " " * 1 # Alternatively to better identify words

for letter in input_string:
    
    # Handle spaces between words
    if letter == ' ':
        encoded_string.append(word_sep)
        
    # Handle new line in text with multiple lines. I basically decided to ignore it.
    elif letter == "\n":
        continue
        
    else:
        idx = morse_table.character == letter.upper()
        encoded_string.append(morse_table.code[idx].values[0])

print(letter_sep.join(encoded_string))

```

    .- -..   .- ... - .-. .-   .--. . .-.   .- ... .--. . .-. .-


## Final comments

We can certainly keep adding features to our code. Other ideas that stem from this project are:
- Convert the script into a function
- Add input validation (e.g. ensure that input is a string)
- Print the actual English string above the Morse code
- Write a script or function that converts Morse code back into English
- Create a game that asks the player to guess which character is a random Morse code
