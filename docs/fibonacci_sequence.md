# Fibonacci Sequence

The Fibonacci sequence is defined by the sum of the preceding two values. So, if start from the values 0 and 1 `sequence = [0,1]`, then then next value is `0 + 1 = 1` and by adding the last value to our sequence we get `sequence = [0,1,1]`. We can repeat this operation one more time. By adding the last two numbers of the list we now have `1 + 1 = 2`, and if we add this computation to the sequence we obtain `sequence = [0,1,1,2]`. Hope you can see the pattern. The next value of the sequence would be `1 + 2 = 3`.

We can write some Python code to automate the calculation for us. Now only taht, we can also instruct Python to give as the entire sequence up to a specific number. Here is the formal definition of the sequence:

$$ F_{0}=0,\quad F_{1}=1 $$


$$ F_{n}=F_{n-1}+F_{n-2} $$

## Fibonacci sequence to a specific value (while loop)


```python

max_val = 100
count = [0,1] 

while True:
    new_val = sum(count[-2:])  # Add last two numbers
    if new_val <= max_val:
        count.append(new_val) 
    else:
        break

print(count)
```

    [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]


## Find the nth Fibonacci value (for loop)


```python
n = 6 # Value must be greater than 2
count = [0,1] # Seed values

for i in range(2,n):
    new_value = sum(count[-2:])
    count.append(new_value)

print(count)
print('The',str(n),'value of the Fibonacci sequence is:',new_value)
```

    [0, 1, 1, 2, 3, 5]
    The 6 value of the Fibonacci sequence is: 5


>Of course, you can alter the initial set of numbers and use the same computation to find out new sequences.

## References

Fibonacci sequence: <https://en.wikipedia.org/wiki/Fibonacci_number?oldformat=true/>
