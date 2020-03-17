# Comparative Anatomy of For Loops

A short example was coded in several common languages for data science and scientific computing to illustrate syntax similarities of `for loops`. The goal is to understand that if you know how to break down a problem logically, then implementing the logic in different languages is mostly a matter of learning some syntax. Certainly, experience with a particular language will allow you to write better and optimized code, but you can translate and prototype simple scripts or code snippets fairly quickly.

To simplify the example I wrote a simple code that returns the first ten numbers of the Fibonacci sequence: `0, 1, 1, 2, 3, 5, 8, 13, 21, 34`

Note that in the examples below the step of the loop was added for completeness. The default value of the step is one and in most cases you will not need to specify the step increment of the `for loop` when the increment is assumed to be one unit.


## Python

```python
F_seq = [0,1] # Seed values
N = 10
step = 1

for i in range(2,N,step):
    F_seq_new = sum(F_seq[-2:])
    F_seq.append(F_seq_new)
```

```
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```


---

## Matlab

Appended new values using the `(end+1)` notation. This is inefficient and not recommended in Matlab, but it was used to illustrate a straight forward solution
```matlab
F_seq = [0,1]; % Seed values
N = 10;
step =1;

for i = 3:step:N
    F_seq_new = sum(F_seq(end-1:end)); % Get last two items of the array
    F_seq(end+1) = F_seq_new;          % Append new value.
end
```

```
0     1     1     2     3     5     8    13    21    34
```

---

## Javascript

Solved without using the reduce() method for easier readability for non Javascript programmers. Javascript does not have a `sum()` method.

```javascript
F_seq = [0,1];

for (i=2; i<10; i++){
    F_last_two_digits = F_seq.slice(-2); // Get last two elements of the sequence
    F_seq_new = F_last_two_digits[0] + F_last_two_digits[1]; // Add the last two elements
    F_seq.push(F_seq_new);  // Append to array
}
```

```
 [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
 ```

---

## Julia

```julia
F_seq = [0,1] # Seed values
N = 10
step =1

for i = 3:step:N
    F_seq_new = sum(F_seq[end-1:end]) # Get last two items of the array
    append!(F_seq,F_seq_new)          # Append new value.
end
```

```
10-element Array{Int64,1}:
  0
  1
  1
  2
  3
  5
  8
 13
 21
 34
 ```

## Synthesis

- All languages contain a keyword to define the `for loop`

- All languages contain syntax for defining the start, end, and step of the iterative process

- All languages specify an iterator that carries the current value of the iteration
