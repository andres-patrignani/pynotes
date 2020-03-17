# Central Dogma: Decoding a Single Polypeptide

*The DNA is the carrier of genetic information and its simplest form can be viewed as a long string of characters that encode biological features and attributes*

The central dogma of molecular biology consists of a two step process to convert the information contained in DNA sequences into proteins. These steps are transcription (DNA into messenger RNA) and translation (messenger RNA into aminoacids). The RNA polymerase is an enzyme that participates in the transcription process, while ribosomes are large molecular structures that translate messenger RNA into a chain of aminoacids to form polypetides and proteines.


<img src="https://www.nature.com/scitable/content/ne0000/ne0000/ne0000/ne0000/105292327/44350_36a.jpg">

**Figure: A gene is expressed through the processes of transcription and translation.
During transcription, the enzyme RNA polymerase (green) uses DNA as a template to produce a pre-mRNA transcript (pink). The pre-mRNA is processed to form a mature mRNA molecule that can be translated to build the protein molecule (polypeptide) encoded by the original gene.
Source: Nature Education.**

You can learn more by reading an excellent educational article by [Suzanne Clancy and William Brown (2008)](https://www.nature.com/scitable/topicpage/translation-dna-to-mrna-to-protein-393/)

The **goal** of this exercise is to use Python to emulate both the RNA polymerase and a ribosome to decode the aminoacids present in a sequence of DNA. To accomplish this goal we will need a DNA sequence and a lookup table to convert codons into aminoacids. I transcribed the information in the table below into a JSON file.

<img src="https://www.nature.com/scitable/content/ne0000/ne0000/ne0000/ne0000/105292568/44376_38b.jpg">

**Figure: The large ribosomal subunit binds to the small ribosomal subunit to complete the initiation complex.
The initiator tRNA molecule, carrying the methionine amino acid that will serve as the first amino acid of the polypeptide chain, is bound to the P site on the ribosome. The A site is aligned with the next codon, which will be bound by the anticodon of the next incoming tRNA. Source: Nature Education.** 

Codons refer to the triplets in the mRNA. The triplets in the sequence of transfer RNA (tRNA) are called anti-codons and are complementary to the codons in the mRNA. In the figure above, `AUG` (in the 5' to 3' mRNA) is the codon for Methionine, while `UAC` (in the tRNA) is the anti-codon for Methionine

There are total of 23 essential aminoacids that encoded in codons consisting of three bases. Some codons are dedicated at instructing the ribosomes the START or STOP of the sequence. The genetic code includes 64 possible permutations, or combinations, of three-letter nucleotide sequences that can be made from the four nucleotides. Of the 64 codons, 61 represent amino acids, and three are stop signals. For example, the codon CAG represents the amino acid glutamine, and TAA is a stop codon. The genetic code is described as degenerate, or redundant, because a single amino acid may be coded for by more than one codon. When codons are read from the nucleotide sequence, they are read in succession and do not overlap with one another.

## Assumptions

* For simplicity we will ignore additional portions of DNA such as promoter and a terminator regions that play an important role during transcription, but that do not contribute the sequence of the polypeptide.

* All bases will be represented in upper case for consistency.

* We will assume that the ribosome is made of a single unit and contains a single activation site. So basically a simplified version of the figure above.


As usual we will start by importing the necessary modules and by loading all the data for the exercise.


```python
# Import modules
import pandas as pd

```


```python
# Read codon-aminoacids lookup table
# Note that the table mathes aminoacids to codons (not the anti-codon)
lookup = pd.read_csv('../datasets/codon_aminoacids.csv')
lookup.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>codon</th>
      <th>letter</th>
      <th>aminoacid</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AAA</td>
      <td>K</td>
      <td>Lysine</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AAC</td>
      <td>N</td>
      <td>Asparagine</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AAG</td>
      <td>K</td>
      <td>Lysine</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AAU</td>
      <td>N</td>
      <td>Asparagine</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ACA</td>
      <td>T</td>
      <td>Threonine</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Random DNA sequence
# Source: https://www.bioinformatics.org
DNA = 'tacctatttcactgccgtccgttgcactacgaacggaagccgtgctcagaccaacacgtccagcaacaaagaact'
DNA = DNA.upper()
print(DNA)

```

    TACCTATTTCACTGCCGTCCGTTGCACTACGAACGGAAGCCGTGCTCAGACCAACACGTCCAGCAACAAAGAACT


I decided to keep it simple, but you can create longer and more diverse DNA sequences using this [random coding DNA tool](https://www.bioinformatics.org/sms2/random_coding_dna.html). For clarity, notice that this tool already provides the complementary DNA strand. Since in this exercise we are also implementing the transcription step I reversed the DNA sequence obtained from the website. Here is the original DNA strand that I obtained from the online tool: `atggataaagtgacggcaggcaacgtgatgcttgccttcggcacgagtctggttgtgcaggtcgttgtttcttga`, which I reversed using the code from the `complementaty DNA sequence` exercise. If you don't do this step, the code will still decode aminoacids, but the resulting polypeptide will not look right (i.e. Methionine will not be the first aminoacid). I simply reversed the DNA strand to closely match textbook examples and available material online.

## Transcription

Given a sequence of DNA bases we need to find the complementary strand. The catch here is that we also need to account for the fact that the base `thymine` is replaced by the base `uracil` in RNA.

To check for potential typos in the sequence of DNA or to prevent that the user feeds a sequence of mRNA instead of DNA to the transcription function, we will use the `raise` statement, which will automatically stop and exit the `for` loop and throw a custom error message if the code finds a base a base other than A,T,C, or G. The location of the `raise` statement is crucial since we only want to trigger this action if a certain condition is met (i.e. we find an unknown base). So, we will place the `raise` statement inside the `if` statement within the `for` loop. We will also return the location in the sequence of the unknown base using the `find()` method. 

The error catching method described above is simple and practical for small applications, but it has some limitations. For instance, we cannot identify whether there are more than one unknwon bases and we cannot let the user know the location of all these bases. Nonetheless, this is a good starting point.



```python
def transcription(DNA):
    '''
    Docstring: Function that converts a strand of DNA into 
    a strand of messenger RNA based on the following nucleotide paris
    A-U, T-A, C-G, G-C.
    
    Input must be a single strand of DNA in string format.
    '''
    mRNA = '' # initialize strand of messenger RNA
    for base in DNA:
        if base == 'A':
            mRNA += 'U'
        elif base == 'T':
            mRNA += 'A'
        elif base == 'C':
            mRNA += 'G'
        elif base == 'G':
            mRNA += 'C'
        else:
            errorMessage = 'Unknown base: ' + base + ' found at: ' + str(DNA.find(base))
            raise Exception(errorMessage)
    
    return mRNA

```

After solving the problem with a short script, we could easily convert the script into a function. This will allow us to use the function multiple times without re-writing the instructions.

## Translation

We are at a breakpoint. We loaded the lookup table and we have the mRNA sequence. Our next step is to solve the translation problem. Basically we need to find a way to search the matching aminoacid for any given codon. If we we find a way to do this, then we can implement it into a `for` loop and apply to the entire mRNA sequence. We will start simple, I want to show you the value of a trivial example. 



```python
# Test that we can match a codon an retrieve the aminoacid
aa_idx = lookup.codon == 'AAA' # AAA is a possible codon from the lookup the table
lookup.aminoacid[aa_idx]       # Use the resulting boolean vector to obtain the aminoacid

```




    0    Lysine
    Name: aminoacid, dtype: object



We cracked the translation problem! The statement can successfully match and retrieve the name of the matching aminoacid given a codon of three bases. This is exactly what we mean with breaking down problems. It can be extremely rewarding to solve small bits of code and will prevent you from writing long chunks of code that are hard to troubleshoot. With practice you will be able to learn how to breakdown large problems into small pieces that can be solved as a logical sequence of smaller problems.

Let's not get too excited, while the answer is correct, the format does not seem entirely correct. We did not obtain a string. The answer also contains additional information (e.g. Name and dtype) about the object. We need to keep in mind that we need to be able to use the resulting string to do something with it (e.g. create a chain of aminoacid names, which will resemble a polypeptide).

So here is a better solution to extract the information we really need:


```python
lookup.aminoacid[aa_idx].values[0] # This syntax access the string within the Pandas object
```




    'Lysine'



Now we can proceed to translate the mRNA into a chain of aminoacids


```python
def translation(mRNA):
    '''
    Docstring: Function that trsnlates a strand of mRNA into 
    a sequence of aminoacids, which is know as a polypeptide.
    
    Input must be a single strand of mRNA in string format.
    '''
        
    # Initial conditions of our ribosome
    polypeptide = [];

    # Iterate over each codon triplet. We will use a step of 3 in the loop.
    for i in range(0,len(mRNA)-2,3):
        codon = mRNA[i:i+3] # Add 3 to avoid overlapping the bases between iterations.
        aminoacid_idx = lookup.codon == codon # Match current codon with all codons in lookup table
        aminoacid = lookup.aminoacid[aminoacid_idx].values[0]
        polypeptide.append(aminoacid)

    return polypeptide

```

## DNA in action


```python
mRNA = transcription(DNA)
polypeptide = translation(mRNA)

print('DNA sequence:',DNA)
print('mRNA sequence:',mRNA)
print('Polypeptide:','-'.join(polypeptide))
```

    DNA sequence: TACCTATTTCACTGCCGTCCGTTGCACTACGAACGGAAGCCGTGCTCAGACCAACACGTCCAGCAACAAAGAACT
    mRNA sequence: AUGGAUAAAGUGACGGCAGGCAACGUGAUGCUUGCCUUCGGCACGAGUCUGGUUGUGCAGGUCGUUGUUUCUUGA
    Polypeptide: Methionine-Aspartic_acid-Lysine-Valine-Threonine-Alanine-Glycine-Asparagine-Valine-Methionine-Leucine-Alanine-Phenylalanine-Glycine-Threonine-Serine-Leucine-Valine-Valine-Glutamine-Valine-Valine-Valine-Serine-Stop


Despite some simplifications, our exercise makes sense. Methionine is the first aminoacid of new proteins (although it can be removed in subsequent steps) and our last codon resulted to be a stop codon. The resulting sequence is our polypeptide!

If you deal with DNA and mRNA seqeuences on a daily basis, consider creating a module with transcription and translation functions that you can readily use in multiple projects.

Our code can only handle a single sequence encoding a single polypeptide. Below I leave a DNA sequence encoding 3 polypeptides. Can you write a function capable of decoding a DNA sequence of multiple polypeptides, each with different number of aminoacids?

`TACTCGTCACAGGTTACCCCAAACATTTACTGCGACGTATAAACTTACTGCACAAATGTGACT`

## References

Clancy, S. and Brown, W., 2008. Translation: DNA to mRNA to protein. Nature Education, 1(1), p.101.

Stothard P. 2000. The Sequence Manipulation Suite: JavaScript programs for analyzing and formatting protein and DNA sequences. Biotechniques 28:1102-1104.

