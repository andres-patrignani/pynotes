# NBconvert Export

A short snippet to export all Jupyter Lab notebooks in a directory into Markdown with associated image files.



```python
import glob
```

## All notebooks


```python
#directory = ''
#glob.os.chdir(directory)
notebooks = [file for file in glob.glob("*.ipynb")]
notebooks[0:3] # List few of them

```




    ['anonymous_functions.ipynb',
     'anscombe_quartet.ipynb',
     'antecedent_precipitation_index.ipynb']




```python
for file in notebooks:
    !jupyter nbconvert $file --to markdown --output-dir ../docs

```

    [NbConvertApp] Converting notebook anonymous_functions.ipynb to markdown
    [NbConvertApp] Writing 2810 bytes to ../docs/anonymous_functions.md
    [NbConvertApp] Converting notebook anscombe_quartet.ipynb to markdown
    [NbConvertApp] Support files will be in anscombe_quartet_files/
    [NbConvertApp] Making directory ../docs/anscombe_quartet_files
    [NbConvertApp] Writing 6445 bytes to ../docs/anscombe_quartet.md
    [NbConvertApp] Converting notebook antecedent_precipitation_index.ipynb to markdown
    [NbConvertApp] Support files will be in antecedent_precipitation_index_files/
    [NbConvertApp] Making directory ../docs/antecedent_precipitation_index_files
    [NbConvertApp] Making directory ../docs/antecedent_precipitation_index_files
    [NbConvertApp] Making directory ../docs/antecedent_precipitation_index_files
    [NbConvertApp] Making directory ../docs/antecedent_precipitation_index_files
    [NbConvertApp] Making directory ../docs/antecedent_precipitation_index_files
    [NbConvertApp] Writing 25212 bytes to ../docs/antecedent_precipitation_index.md


## Single notebook


```python
# Define a single notebook
specific_notebook = 'nbconvert_export.ipynb'

# Export single notebook
!jupyter nbconvert $specific_notebook --to markdown --output-dir ../docs

```

    [NbConvertApp] Converting notebook nbconvert_export.ipynb to markdown
    [NbConvertApp] Writing 1816 bytes to ../docs/nbconvert_export.md

