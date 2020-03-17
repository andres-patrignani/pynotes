# Regular Expressions

One of the most useful coding skills when heavily working with text and text patterns. In Environmental Sciences data in the form of text is far less common than in other disciplines like journalism, history, and digital humanities. Nonetheless, matching of string patterns becomes relevant when trying to decode the genetic code represented as a sequence of nucleotides or proteins represented by sequences of aminoacids.

In this exercise we will learn the basics of regular expressions (regexp for short). A regular expression is a sequence of characters that follow a specific syntax and are used to express a search pattern within a string. 

The fact that we don't typically use regular expressions in Environmental Sciences does not mean that there is no potential. Beyond genomics, regular expressions could be useful to match plot or sample identification numbers, reading specific files, and efficiently conducting literature research.

Official documentation: https://docs.python.org/3/library/re.html


```python
# Import modules
import re

```


```python
# Some random DNA sequence as a sample text to get us started
# This DNA contains 4 random sequences of 18 bases each
# Source: https://www.bioinformatics.org/sms2/random_coding_dna.html
DNA = 'atggcgcacggaccctagatgcgacgtccgtactaaatgcagcaacacgtgtagatgcggcggtcgcaatga'

```


```python
# Count the number of Guanine
guanine = re.findall('g', DNA)
print(len(guanine))

# Alternative solution using the built-in count( method)
# This is to remind you that there isn't always need for a library
print(DNA.count('g'))

```

    22
    22



```python
# Chargaff's rule states that the GC ratio should be 0.5
# Now we can use the full power of the regular expressions 
# module using the 'set' operator
gc = re.findall('[gc]', DNA)
chargaff_rule = len(gc) / len(DNA)
print(round(chargaff_rule,2))

```

    0.57



```python
# Substitute Thymine for Uracil (i.e. replace t for u)
# This will convert DNA into RNA
RNA = re.sub("t", "u", DNA)
print(RNA)

```

    auggcgcacggacccuagaugcgacguccguacuaaaugcagcaacacguguagaugcggcggucgcaauga



```python
# Find all methinine codons 'aug'
start_codons = re.findall('aug',RNA)
print(start_codons)
print(len(start_codons))

# There is an extra 'aug'
```

    ['aug', 'aug', 'aug', 'aug', 'aug']
    5



```python
re.finditer
```


```python
# Find all stop codons: uaa, uag, uga using the 'or' operator
stop_codons = re.findall('uaa|uag|uga',RNA)
print(stop_codons)

```

    ['uag', 'uaa', 'uag', 'uga']



```python
re.search('aug', RNA)
```


```python
# Find position of starting codons for Methionine (aug)
methionine = re.search('aug', RNA)
print(methionine.start())
print(methionine.end())
print(methionine.span()) # Returns both start and end in a tuple

```

    0
    3
    (0, 3)



```python
# Find position of starting codon for Methionine (aug)
methionine = re.search('aug', RNA)
print(methionine.start())
print(methionine.end())
print(methionine.span()) # Returns both start and end in a tuple

```


```python
# Split RNA at the stop codons. Note that stop codons have been removed from the resulting list
splitted_RNA = re.split("uaa|uag|uga", RNA)
print(splitted_RNA)

```

    ['auggcgcacggaccc', 'augcgacguccguac', 'augcagcaacacgug', 'augcggcggucgcaa', '']



```python
# Example with serial numbers (match last four digits)
stations_string = 'Ottawa 20SE,Tribune 6NE,Ashland 8S,Elmdale 1SE,Belleville 2W'

```


```python
re.findall('[A-Z]+[a-z]+', stations_string)

```




    ['Ottawa', 'Tribune', 'Ashland', 'Elmdale', 'Belleville']




```python
re.findall('\d+', stations_string)

```




    ['20', '6', '8', '1', '2']




```python
re.findall('\B[A-Z][A-Z]?', stations_string)

```




    ['SE', 'NE', 'S', 'SE', 'W']



`\B` located at the beginning of the statements means: not at the beginning of the string

`[A-Z]` any upper case letter

`[A-Z]?` optionally followed by a seconda any upper case letter

## References

https://docs.python.org/3/library/re.html
