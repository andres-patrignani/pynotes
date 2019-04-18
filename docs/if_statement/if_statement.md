
# If Statements

We can use decision rules to instruct the computer to execut specific commands. This way we can control the flow of our code.


```python
# Basic if statement
value = 8

if value == 8:
    print('This is value is an eight!')

```

    This is value is an eight!



```python
# If and Else statement
value = 8

if value % 2 == 0:
    print('Number is even')
else:
    print('Number is odd')

```

    Number is even



```python
# If, Else-if, and Else statement
value = 8

if value > 10:
    print('Value is too high')
    
elif value <=10 & value >= 5:
    print("That's perfect")
    
else:
    print('Number is way too low')

```

    That's perfect



```python
# For simple expressions we can use a one-line If statement
print("A") if a > b else print("B")
```

    A



```python
crop = 'corn'

if crop == 'corn':
    Tbase = 8
    print('Tbase:',Tbase)
    
elif crop == 'wheat':
    Tbase = 0
    print('Tbase:',Tbase)
    
else:
    print('Crop not found. Options are corn or wheat. Your input was:',crop)
    

```

    Tbase: 8



```python
for value in range(0,10):
    if value % 2 == 0:
        print('Number',value,'is even')
    else:
        print('Number',value,'is odd')
        

```

    Number 0 is even
    Number 1 is odd
    Number 2 is even
    Number 3 is odd
    Number 4 is even
    Number 5 is odd
    Number 6 is even
    Number 7 is odd
    Number 8 is even
    Number 9 is odd

