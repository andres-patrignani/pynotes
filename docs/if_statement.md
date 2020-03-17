# If Statements

The `if` statements play a fundamental role controlling the logical flow of the code. A sequence of decision rules instruct the computer to execut specific commands. `If` statements evaluate whether a condition is `True` or `False`, and at that point the code bifurcates. Sometimes there are multiple conditions as part of a single `if` statement, in which the code could branch into many different options.



```python
## Trivial example
T_air = -4 # degrees Celsius

if T_air <= 0:
    print("It's freezing outside")
elif T_air > 0 and T_air <= 15:
    print("It's cool outside")

elif T_air > 15 and T_air <30:
    print("It's nice outside")

else:
    print("It's too hot outside")
```

    It's freezing outside


>`if` statements allow us to include multiple options using the `elif` (else if) keyword accompained by a conditional statement. 


## Odd or even?

A simple exercise to learn the basic structure of `If` staments is to compute and print whether a given number is odd or even. Even numbers are divisible by 2 with a reminder of zero whereas odd numbers are not divisible by 2 and have remainder greater than zero. To perform this operation we can use the Python `modulo` operator, which represented by `%`.


```python
# If and Else statement
value = 8

if value % 2 == 0:
    print('Number is even')
else:
    print('Number is odd')

```

    Number is even


Using the same code within a `for loop` we can easily check all the numbers from one to ten.


```python
for value in range(1,11):
    if value % 2 == 0:
        print('Number',value,'is even')
    else:
        print('Number',value,'is odd')
        
```

    Number 1 is odd
    Number 2 is even
    Number 3 is odd
    Number 4 is even
    Number 5 is odd
    Number 6 is even
    Number 7 is odd
    Number 8 is even
    Number 9 is odd
    Number 10 is even

