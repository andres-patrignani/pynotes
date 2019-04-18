
# Operating system module

The OS module is extremely useful to navigate directories and load and save data in different locations of your computer. Here we cover the most common functions within the module:

- `getcwd()`   = get current working directory
- `os.sep`    = directory separator '/' in macs and '\' in Windows
- `os.chdir()` = change directory. See that in Python is slightly different than in the terminal

We will first learn how to gather information and then how to navigate the directory.


```python
import os
```

## Access directory information


```python
# Get current working directory (cwd)
my_dir = os.getcwd()  
print(my_dir)
print(type(my_dir))   # Print type. It should be a string

```

    /Users/andrespatrignani/Desktop
    <class 'str'>



```python
# Split full path at each slash. Windows has a back slash (\) and Mac has a forward slash (/) to 
# separate diretories.

my_var  = os.getcwd().split(os.sep)
print(my_var)        # Print list
print(type(my_var))  # Type list. The split function converts it from string to list, so that we can access individual items.

print(os.sep)        # Prints the slash accoridng to your system. Handy when sharing code between different platforms.

```

    ['', 'Users', 'andrespatrignani', 'Desktop']
    <class 'list'>
    /



```python
# Access first folder name in the list (the zero index is empty)
foldername = my_var[1]
print(foldername)

# Access last folder name in the list
print(os.getcwd().split(os.sep)[-1])
```

    Users
    Desktop


## Navigate directory with Python

Changing directories using Python commands will enable you to load files and access data located in different folders. 
>**Note** that the file navigation bar on the left of the Jupyter Lab and the Python interprer can be in different directories. In other words, when you change the directory using Python, the navigation bar on the left of the Jupyter Lab will not change. They are independent and you need to focus on the Python interpreter for this to work.


```python
# Navigation

# Go to a specific directory:
os.chdir("/Users/andrespatrignani/Desktop/Coding/sandbox/")

# You can type the tilde character followed by the Tab key to auto-complete the home directory:
# os.chdir("~/Desktop/Coding/sandbox/")

# If in doubt, just get the current working directory with the following command:
print(os.getcwd())

# Navigate one directory up (outside current folder)
os.chdir("..")

# Find again where the Python is:
print(os.getcwd())

# Navigate back into one of the folders
os.chdir("sandbox/")

# Find again where Python is:
print(os.getcwd())

# Navigate two directories up (outside current folder)
# I will use os.sep to automatically insert the correct slash. Try print(os.sep)
os.chdir(".." + os.sep + "..") # Same as: os.chdir("../..") or os.chdir("..\..") 

# Find again where the Python  is:
print(os.getcwd())
```

    /Users/andrespatrignani/Desktop/Coding/sandbox
    /Users/andrespatrignani/Desktop/Coding
    /Users/andrespatrignani/Desktop/Coding/sandbox
    /Users/andrespatrignani/Desktop


## List files


```python
# List all files in current directory (including hidden files)
os.listdir()
```




    ['.DS_Store',
     '.ipynb_checkpoints',
     '.localized',
     'abstract_example.pdf',
     'apatrig',
     'apatrigksu',
     'api_state_sm.m',
     'background_soil.jpg']


