
# **Glob module**

An easy and complete module for directory navigation. Somewhat similar to the operating system module (os module).


```python
import glob
```

One of the most useful commands is the `getcwd()`, which stands for *get current working directory*. With this command we know where the Python interpreter is at. If we want to work in a project that requires navigation over several folders, we might consider first navigating to the root folder of the project. Imagine you need to load files in a specific folder, then crunch some numbers, and finally store the outputs in a different folder. The `glob` module would be ideal for this.

Let's first print (you could also store the output into a variable) the current working directory


```python
print(glob.os.getcwd())
```

    /Users/andrespatrignani/Dropbox/Teaching/Scientific programming/pynotes/notebooks


We can navigate one level up (outside the current folder) in the directory using `..`


```python
glob.os.chdir('..')
```

    None


We can repeat one more time the `getcwd()` command to indeed ensure we are in the right directory


```python
print(glob.os.getcwd())
```

    /Users/andrespatrignani/Dropbox/Teaching/Scientific programming/pynotes


Another alternative is to navigate one level down the directory structure (insde a new folder) by simply typing the name of the folder. We can also multiple folders in tandem. For instance: `folder1/folder2/folder3/`


```python
glob.os.chdir('Datasets/')
```


```python
print(glob.os.getcwd())
```

    /Users/andrespatrignani/Dropbox/Teaching/Scientific programming/pynotes/datasets


Another useful function is to list all the files in a directory. Sometimes you might be interested in files of a specific type (`csv`, `txt`, `jpg`, etc.) that are in the same directory mixed with other files. In the latter case we can make use of a wildcard (a placeholder meaning *all files with a specific extension*). The wildcard is represented with a `*`. For instance, to retrieve all the text files in a given directory we would specify it as: `*.txt`. Let's looks at these two examples below:


```python
# All files in the directory
print(glob.os.listdir())
```

    ['.DS_Store', '.ipynb_checkpoints', 'corn_allometric_biomass.csv', 'dna_sequence.txt', 'faostats_usa_arable_land.csv', 'fargo_hourly_deep_soil_temperature.csv', 'global_wheat.csv', 'gypsum_hourly.csv', 'gypsum_ks_daily_2018.csv', 'gypsum_ks_drydown_2017.csv', 'jabberwocky_lewis_carroll.txt', 'KS_Manhattan_6_SSW.txt', 'mauna_loa_co2.csv', 'morse_lookup_table.txt', 'mosquito_abundance.csv', 'nobel_physics.txt', 'ok_mesonet_8_apr_2019.csv', 'oliver_twist.txt', 'population_area_and_density.csv', 'soil_water_retention_curve.csv', 'storage_20180701_1km_v1.tif', 'wheat.jpg', 'world_wheat.csv']



```python
# List and store all the text files in the same directory
txtfiles = []
for file in glob.glob("*.txt"):
    txtfiles.append(file)
    print(file)

```

    dna_sequence.txt
    jabberwocky_lewis_carroll.txt
    KS_Manhattan_6_SSW.txt
    morse_lookup_table.txt
    nobel_physics.txt
    oliver_twist.txt


The last exercise of this short tutorial is to navigate a level up and then down to return to the directory in which we started this tutorial.


```python
print(glob.os.getcwd())
glob.os.chdir('../notebooks/')
print(glob.os.getcwd())
```

    /Users/andrespatrignani/Dropbox/Teaching/Scientific programming/pynotes/datasets
    /Users/andrespatrignani/Dropbox/Teaching/Scientific programming/pynotes/notebooks


 As you can see it's farily easy to navigate the directory. You can use to initialize a project or inside for loops to control the the directories of your code.
