
# User inputs


```python
# User inputs (strings)
print("Please, write your full name?")
fullname = input()
print("Your full name is: {}".format(fullname))
```

    Please, write your full name?


     Andres


    Your full name is: Andres



```python
# Shorter and more elegant
fullname = input("Please, write your full name?")
print("Your full name is: {fullname}".format(fullname=fullname))
```

    Please, write your full name? Andres P


    Your full name is: Andres P



```python
# User inputs (strings)
fullname = input("Please write your full name?")
degree = input("What is your degree?")
location = input("Where are you from?")

print("Great {}! This is the first time that I meet someone from {} that studies {}".format(fullname,location,degree))

```

    Please write your full name? Andres P
    What is your degree? Soil science
    Where are you from? Arg


    Great Andres P! This is the first time that I meet someone from Arg that studies Soil science



```python
# Numeric input
height = input("How tall are you (in cm)?")
print(type(height))
print("So your height is {}".format(height*2.54))

```

    How tall are you (in cm)? 186


    <class 'str'>



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-9-e5aeb3ac43eb> in <module>()
          2 height = input("How tall are you (in cm)?")
          3 print(type(height))
    ----> 4 print("So your height is {}".format(height*2.54))
    

    TypeError: can't multiply sequence by non-int of type 'float'



```python
height = float(input("How tall are you?")) # in case you want the input to be a float
print(type(height))
print("So your height is {}".format(height/2.54))

```

    How tall are you? 186


    <class 'float'>
    So your height is 73.22834645669292

