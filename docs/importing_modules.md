# Importing modules

One of the most basic operations in Python is importing modules. Modules (a.k.a packages, toolboxes) contain python code that we can load to add extra functionality to our program. Of course, this means that somehow we need to learn which are the available modules that we can import. This is something that you can learn in a coding class, looking at the Python documentation, or by simply searching on the web.

This chapter focuses on simple and frequently used core Python modules. More extensive and powerful modules such as `numpy` and `pandas` have an entire chapter dedicated to it.

## Importing module syntax
There are multiple ways of importing Python modules depending on whether you want to import the entire module, assign a shorter alias, or import a specific sub-module.

**Option 1**<br/>
Syntax:  `import <module>`<br/>
Example: `import math` <br/>
Commonly used when the name of the module is already short

**Option 2**<br/>
Syntax:  `import <module> as <alias>` <br/>
Example: `import numpy as np`<br/>
Commonly used with modules that are used extensively and that may be called multiple times within a single line of code.

**Option 3**<br/>
Syntax:  `import <module>.<submodule> as <alias>`<br/>
Example: `import matplotlib.pyplot as plt`

**Option 4**<br/>
Syntax:  `from <module> import <submodule> as <alias>`<br/>
Example: `from numpy import random as rand` 

## System module
The system, "sys", module has multiple functions (also known as methods) that allow us to obtain information about the python interpreter. Modules are imported once, usually at the the top of the code or notebook. For instance, we can check our python version as follows:


```python
import sys
print(sys.version) # Useful to check your python version
```

    3.7.0 (default, Jun 28 2018, 07:39:16) 
    [Clang 4.0.1 (tags/RELEASE_401/final)]

