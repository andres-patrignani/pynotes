
# Import tabular data using the `open()` function

Loading data is an essential step when using Python for scientific data analysis. In this tutorial we will cover three ways of importing data. This tutorial will not focus on how to access or analyze the data.


```python
import glob

dataset_dir = '/Users/andrespatrignani/Dropbox/Teaching/Scientific programming/introcoding-spring-2019/Datasets/'
glob.os.chdir(dataset_dir)

```


```python
# List files in directory

# print(glob.os.listdir()) # Lists all files and returns a list 
for file in glob.glob('*.csv'): print(file) # Easier to list files with specific extensions
```

    corn_allometric_biomass.csv
    fargo_hourly_deep_soil_temperature.csv
    global_wheat.csv
    gypsum_daily_2018.csv
    gypsum_hourly.csv
    mauna_loa_co2.csv
    mosquito_abundance.csv
    population_area_and_density.csv
    tribune_daily_historical.csv
    world_wheat.csv


## Function arguments

`"r"` - Read - Default.

`"a"` - Append

`"w"` - Write

`"x"` - Creates the specified file

`"t"` - Text - Default

`"b"` - Binary (e.g. images)

Source: <https://www.w3schools.com/python/python_file_handling.asp>


```python
file = open('nobel_physics.txt','r')  # 'r' is the default, so you can just pass the filenmae
names_list = file.read() # A single long string

print(names_list)
print(names_list[0])

```

    Albert Einstein,
    Werner Heisenberg,
    Marie Curie,
    Guglielmo Marconi,
    Peter Higgs,
    Enrico Fermi,
    Ernest Lawrence,
    Paul Dirac,
    Lord Rayleigh,
    Antoine Henri Becquerel,
    Wilhelm Conrad Röntgen,
    Hendrik Lorentz,
    Max Planck,
    Niels Bohr,
    Gustav Hertz,
    Werner Heisenberg,
    Erwin Schrödinger,
    Paul Dirac,
    Enrico Fermi,
    Hideki Yukawa,
    Richard Phillips Feynman
    A



```python
names_list = open(filename).read().split('\n')
print(names_list)

```

    ['Albert Einstein,', 'Werner Heisenberg,', 'Marie Curie,', 'Guglielmo Marconi,', 'Peter Higgs,', 'Enrico Fermi,', 'Ernest Lawrence,', 'Paul Dirac,', 'Lord Rayleigh,', 'Antoine Henri Becquerel,', 'Wilhelm Conrad Röntgen,', 'Hendrik Lorentz,', 'Max Planck,', 'Niels Bohr,', 'Gustav Hertz,', 'Werner Heisenberg,', 'Erwin Schrödinger,', 'Paul Dirac,', 'Enrico Fermi,', 'Hideki Yukawa,', 'Richard Phillips Feynman']



```python
# Using advanced list comprehensions
names_list = [line.strip('\n') for line in open(filename)]
print(names_list)

```

    ['Albert Einstein,', 'Werner Heisenberg,', 'Marie Curie,', 'Guglielmo Marconi,', 'Peter Higgs,', 'Enrico Fermi,', 'Ernest Lawrence,', 'Paul Dirac,', 'Lord Rayleigh,', 'Antoine Henri Becquerel,', 'Wilhelm Conrad Röntgen,', 'Hendrik Lorentz,', 'Max Planck,', 'Niels Bohr,', 'Gustav Hertz,', 'Werner Heisenberg,', 'Erwin Schrödinger,', 'Paul Dirac,', 'Enrico Fermi,', 'Hideki Yukawa,', 'Richard Phillips Feynman']



```python
#Jabberwocky by Lewis Carroll

# Read text files in Python is easy
poem = open("jabberwocky_lewis_carroll.txt", "r").read() 
print(poem)

```

    ’Twas brillig, and the slithy toves 
          Did gyre and gimble in the wabe: 
    All mimsy were the borogoves, 
          And the mome raths outgrabe. 
    
    “Beware the Jabberwock, my son! 
          The jaws that bite, the claws that catch! 
    Beware the Jubjub bird, and shun 
          The frumious Bandersnatch!” 
    
    He took his vorpal sword in hand; 
          Long time the manxome foe he sought— 
    So rested he by the Tumtum tree 
          And stood awhile in thought. 
    
    And, as in uffish thought he stood, 
          The Jabberwock, with eyes of flame, 
    Came whiffling through the tulgey wood, 
          And burbled as it came! 
    
    One, two! One, two! And through and through 
          The vorpal blade went snicker-snack! 
    He left it dead, and with its head 
          He went galumphing back. 
    
    “And hast thou slain the Jabberwock? 
          Come to my arms, my beamish boy! 
    O frabjous day! Callooh! Callay!” 
          He chortled in his joy. 
    
    ’Twas brillig, and the slithy toves 
          Did gyre and gimble in the wabe: 
    All mimsy were the borogoves, 
          And the mome raths outgrabe.



```python
# String operations in large text files
word_search = 'Jabberwock' 
wordcount = poem.count(word_search)
print(word_search + ' appeared: ' + str(wordcount) + ' times')
```

    Jabberwock appeared: 3 times

