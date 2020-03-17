# Strings

At first it seems that programming languages are only useful to only crunch numbers, but often times we realize that we need to manipulating strings. Strings are ubiquitous, they are appear in sample names, locations, table headers, DNA sequences, and even for printing short strings to debug your own code.

## Define strings


```python
# Apostrophes
mystr1 = 'This is a string'
type(mystr1)

```




    str




```python
# Quotation marks
mystr2 = "This is also a string"
type(mystr2)

```




    str




```python
# Triple quotes

print("""This is a long string
spanning multiple 
lines""") # We will use this to document functions

mystr3 = '''This is also a 
multiline 
string'''
print(mystr3)

# Triple quotes can span multiple lines and are super useful to document functions and scripts
snark = '''Just the place for a Snark!" the Bellman cried,
As he landed his crew with care;
Supporting each man on the top of the tide
By a finger entwined in his hair.'''

print(snark)
```

    This is a long string
    spanning multiple 
    lines
    This is also a 
    multiline 
    string
    Just the place for a Snark!" the Bellman cried,
    As he landed his crew with care;
    Supporting each man on the top of the tide
    By a finger entwined in his hair.


## Split text lines into a list


```python
# Split text lines into a list

print(snark) # Print original text

snark_lines = snark.splitlines(); # Split

print('') # Add some space for readability
print(snark_lines) # Print splitted text
print(snark_lines[0])

```

    Just the place for a Snark!" the Bellman cried,
    As he landed his crew with care;
    Supporting each man on the top of the tide
    By a finger entwined in his hair.
    
    ['Just the place for a Snark!" the Bellman cried,', 'As he landed his crew with care;', 'Supporting each man on the top of the tide', 'By a finger entwined in his hair.']
    Just the place for a Snark!" the Bellman cried,


## Split string on a given character


```python
csv_string = 'This, is, a, string, separated, by, commas'
splitted_csv_string = csv_string.split(',')
print(splitted_csv_string)
print(splitted_csv_string[4]) # Now we can access individual words

```

    ['This', ' is', ' a', ' string', ' separated', ' by', ' commas']
     separated



```python
# Split a single string. This method is useful when we encode information in file names and URL links.
data = 'lat_36.7_lon_-97.5_elev_345_meters'.split('_')
print(data)
print(data[1])
lat = float(data[1]) # Convert string into float and store it in a variable called 'lat'

```

    ['lat', '36.7', 'lon', '-97.5', 'elev', '345', 'meters']
    36.7


## Replace characters


```python
# Use the replace function to add or strip a character (or sequence of characters) from a string
print('file_name'.replace('_', ''))      # Remove underscore
print('file_name'.replace('-', 'hello')) # Replace underscore with 'hello'
```

    filename
    filehelloname


## Join strings


```python
# Concatenate strigs in list using custom delimiter

# Useful when building file names and URLs. You can pass a tuple or a list.
texture_list = ["Silty", "clay", "loam"]
print(" ".join(texture_list))
print("-".join(texture_list))

```

    Silty clay loam
    Silty-clay-loam



```python
# Concatenate strigs in tuple using custom delimiter

texture_tuple = ("Silty", "clay", "loam")
print(" ".join(texture_tuple))
```

    Silty clay loam



```python
# Concatenate strings using the '+' sign

filename = "myfile"
extension = ".csv"
path = "/User/Documents/Datasets/"
fullpath = path + filename + extension
print(fullpath)

```

    /User/Documents/Datasets/myfile.csv



```python
# Merge strings and numbers

A = 2
print('A = ' + str(A)) # Need to convert the integer to a string using the str() function
```

    A = 2


## Formatting strings


```python
# Check whether ALL the characters in the string are numbers 

str.isnumeric('20')   # Returns True

```




    True




```python
# Pperiods are not numbers!
str.isnumeric('2.0')  # Returns False
```




    False




```python
# Traditional way of formatting strings. Also known as %-formatting
print("The sky is %s as the ocean" % "blue") # Still works, but might be deprecated in the future
print("The sky is %s as the ocean" , "blue") # This won't work

# New f-string formatting
print("The sky is {} as the ocean".format("blue")) # Variables must follow the order of the brackets
print("The sky is {color} as the ocean".format(color="blue")) # Recommended

```

    The sky is blue as the ocean
    The sky is %s as the ocean blue
    The sky is blue as the ocean
    The sky is blue as the ocean



```python
# Print in new line
print("\nJan\nFeb\nMar") # \n represents a new line
```

    
    Jan
    Feb
    Mar



```python
# Escaping using the backslash since inches are represented by "
print("The height is 6' 4\"") 
```

    The height is 6' 4"



```python
diameter = 1
circle_area = 3.14
print('The area of a circle with a diameter of {diameter} cm has an area of {circle_area} cm'
      .format(diameter=diameter,circle_area=circle_area))

```

    The area of a circle with a diameter of 1 cm has an area of 3.14 cm



```python
# Write a label for a plot using %-formatting

# % Denotes the beginning of the string format
# f stands for float
parvalue = [0.3,0.1,120] # Three parameter values, typically obtained by curve fitting
label = 'fit: a=%5.3f, b=%5.3f, c=%3.0f' % tuple(parvalue)
print(label)

# 5 is the field width (columns including the dot), and 3 is the number of decimal places

print('DOY: %03.0f' % tuple([1]))
print('DOY: %03.0f' % tuple([12]))
print('DOY: %03.0f' % tuple([365]))

# Example for MODIS URL request date
print('DOY: A%03.0f' % tuple([1]))
print('DOY: A%03.0f' % tuple([365]))

# An alternative to fill with leading zeros
print('34'.zfill(3))

```

    fit: a=0.300, b=0.100, c=120
    DOY: 001
    DOY: 012
    DOY: 365
    DOY: A001
    DOY: A365
    034


## Comparing strings


```python
# Compare strings
str1 = 'orange'
str2 = 'apple'
str3 = 'lemon'

str1 == str2 # or simply 'orange' == 'apple'

```




    False




```python
# Case matters
'apple' == 'Apple'

# Capitalize
print('Original string: ' + str1)
print(str1.capitalize())

print(str1.upper())
```

    Original string: orange
    Orange
    ORANGE


## String sequences


```python
# String sequences
str2.count('p') # Count number of 'p' characters in apple

# or sequence of characters
'apple'.count('pl') # You can also use a string, without declaring a new variable.

# Find if word starts with one of the following sequences
print(str2.startswith(('app','ora'))) # Note that the input is a tuple: ('app','ora')

# Find if word ends with one of the following sequences
print(str2.endswith(('ple','nge')))   # Note that the input is a tuple ('ple','nge')
```

    True
    True

