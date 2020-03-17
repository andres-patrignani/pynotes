# Sorting Numbers

Sorting a shuffled list of numbers is one of my favorite puzzles. It has nothing to do with Environmental Sciences, but it is a beatifull example to learn to think logically, refine strategies, and wrestle against the white canvas. At first glance the challenge looks simple. Our brain has been faced with this operation so many times that we do not realize the level of complexity behind sorting a mere list of a handful of integers.

**Challenge**: Write a script to sort the sequence `2, 4, 1, 5, 3` in *ascending* order without using the `sort()` built-in method. 

I suggest you first solve this exercise by hand using pencial and paper. Think about the steps that you take while sorting this list of numbers and develop a **tentative strategy** that you can implement in Python (or any other language). Don't worry if it doesn't work on the first attempt. After each try you will gain more knowledge on what works and what does not. Then go back to you initial plan and try to improve it. If your code does not work after several tries, you need to be willing to completely discard your initial plan and be willing to start from scratch again with a new tentative strategy. It's this process of coming up with new (and sometimes better) alternatives that I encourage you to practice. 

The demon of frustration always shows up after few unsuccessful tries. Just prepare a cup of coffee or tea and be prepare to have some joy.

The solution is obvious, but I will still give you the solution using the `sort()` method.


```python
# Sort the following sequence
seq = [2,4,1,5,3]

```


```python
# Answer using the sort() built-in method
seq.sort()
print(seq)

```


The solution that I present here is neither the most efficient nor the most elegant. The solution below is what I came up when I was faced with the challenge after sketching and trying different alternatives, without searching online for the solution. If you are one of those students that like to sneak peek the solution online "just to get you started", then I recommend you disable your internet connection.

Here are some points on the logic of the solution below for sorting the values in ascending order.

* If the current item of the list is greater than the previous value in the list, then do nothing. For instance, when we get to `4`, since `4 > 2`, then we leave `4` in its current place.

* If the current item of the list is smaller than the previous value in the list, then swap them. We will do this by removing the current item and then inserting it in front of the preceding value. For instance, when the `for` loop gets to the value of `1`, since `1 < 4` then we move `1` before `4`. At this point the initial sequence will be `[2,1,4,5,3]`. Something similar will occur with the number `3`. During this iteration, the loop keeps moving, so to move the number `1` before the number `2` we will need to repeat the entire process again, which gives rise to the next logical step. 

* Repeat until the list is sorted. We will need some sort of condition to let the interpreter know when the list is sorted. We will leave this step for later to focus on the previous steps.



```python
# Compact solution
seq = [2,4,1,5,3]
sorted_seq = []
while len(seq) > 0:  # more pythonic alternative: while seq:
    seqitem = seq[0]
    if len(sorted_seq) == 0: # more pythonic alternative: not sorted_seq:
        sorted_seq.append(seqitem)
        seq.remove(seqitem)
    else:
        for index,sorteditem in enumerate(sorted_seq):
            if seqitem < sorteditem:
                sorted_seq.insert(index,seqitem)
                seq.remove(seqitem)
                break
             
            elif index == len(sorted_seq)-1:
                sorted_seq.append(seqitem)
                seq.remove(seqitem)
                break
            
print(sorted_seq)

```

    [1, 2, 3, 4, 5]



```python
# Annotated solution
seq = [2,4,1,5,3]
sorted_seq = []

print('At the beginning the list seq looks like this:',seq)
print('At the beginning the list sorted_seq looks like this:',sorted_seq)

while seq:
    seqitem = seq[0]
    if not sorted_seq:
        print('sorted_seq is empty, so Python runs this section')
        
        sorted_seq.append(seqitem)
        print('Because sorted_seq is empty, we append:', seqitem, 'to sorted_seq:',sorted_seq)
        
        seq.remove(seqitem)
        print('and we remove:',seqitem, 'from seq, which now has these elements:',seq)

    else:
        print('sorted_seq is no longer empty, so Python is running this section')
        
        for index,sorteditem in enumerate(sorted_seq):
            print(seqitem,'was compared to',sorteditem)
            if seqitem < sorteditem:
                
                sorted_seq.insert(index,seqitem)
                print('because',seqitem,'is smaller than',sorteditem,'it was moved before',sorteditem)
                print('The sorted_seq now looks like this:',sorted_seq)
                
                seq.remove(seqitem)
                print('The list seq now looks like this:',seq)
                
                break
            

            # If we reach the end of the loop that means we are dealing with
            # a number higher than any value in the sorted sequence, 
            # so we move it to the last location
            elif index == len(sorted_seq)-1:
                
                sorted_seq.append(seqitem)
                print('because',seqitem,'is higher than all the numbers in sorted_seq, it was moved to the end of sorted_seq')
                print('The sorted_seq now looks like this:',sorted_seq)
                
                seq.remove(seqitem)
                print('The list seq now looks like this:',seq)

                break
            
            print('because we have not reach the end of sorted_seq, and',seqitem,'is greater than',sorteditem,'we move on onto the next value in sorted_seq')
                
#print('The list seq now looks like this:',seq)
print('Because there are no elements left in seq, the while loop is terminated.')
print('The sorted sequence looks like this:',sorted_seq)

```

    At the beginning the list seq looks like this: [2, 4, 1, 5, 3]
    At the beginning the list sorted_seq looks like this: []
    sorted_seq is empty, so Python runs this section
    Because sorted_seq is empty, we append: 2 to sorted_seq: [2]
    and we remove: 2 from seq, which now has these elements: [4, 1, 5, 3]
    sorted_seq is no longer empty, so Python is running this section
    4 was compared to 2
    because 4 is higher than all the numbers in sorted_seq, it was moved to the end of sorted_seq
    The sorted_seq now looks like this: [2, 4]
    The list seq now looks like this: [1, 5, 3]
    sorted_seq is no longer empty, so Python is running this section
    1 was compared to 2
    because 1 is smaller than 2 it was moved before 2
    The sorted_seq now looks like this: [1, 2, 4]
    The list seq now looks like this: [5, 3]
    sorted_seq is no longer empty, so Python is running this section
    5 was compared to 1
    because we have not reach the end of sorted_seq, and 5 is greater than 1 we move on onto the next value in sorted_seq
    5 was compared to 2
    because we have not reach the end of sorted_seq, and 5 is greater than 2 we move on onto the next value in sorted_seq
    5 was compared to 4
    because 5 is higher than all the numbers in sorted_seq, it was moved to the end of sorted_seq
    The sorted_seq now looks like this: [1, 2, 4, 5]
    The list seq now looks like this: [3]
    sorted_seq is no longer empty, so Python is running this section
    3 was compared to 1
    because we have not reach the end of sorted_seq, and 3 is greater than 1 we move on onto the next value in sorted_seq
    3 was compared to 2
    because we have not reach the end of sorted_seq, and 3 is greater than 2 we move on onto the next value in sorted_seq
    3 was compared to 4
    because 3 is smaller than 4 it was moved before 4
    The sorted_seq now looks like this: [1, 2, 3, 4, 5]
    The list seq now looks like this: []
    Because there are no elements left in seq, the while loop is terminated.
    The sorted sequence looks like this: [1, 2, 3, 4, 5]


To solve the problem I decided to use a `while` loop. This loop can be practical when we want to keep running a routine until a specific condition is satisfied. The only drawback of `while` loops is that during prototyping the condition might never be met, in which case the `while` loop will not stop and can freeze your notebook and even your computer. I suggest using the printing command to display some steps and see if the loop is working as expected. If your computer does not respond or the code is taking long time, then consider interrupting the Python kernel.

## Additional thoughts and practice ideas

* Can you modify the code to sort numbers in *descending* order?

* What happens if we pass a list of non-unique integers? For instance: `[2, 3, 1, 5, 4, 1]`

* What happens if the list has some missing integers? For instance: `[2, 1, 5, 4]`

* What happens if the list has floating point numbers? For instance: `[2, 1.1, 5.1, 4, 3.2, 1]`

* Can you convert the script into a function to sort any sequence of numbers?

