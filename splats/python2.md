---
layout: splat
title: Basic sequence manipulation with Python
---

In this workshop you will learn how to load a text file containing a DNA sequence, convert it to RNA and translate it to protein. The workshop will build on what you learned in your [first workshop]({{ "/splats/python1" | absolute_url }}). You should know basic Python and how to use a Jupyter notebook.

## Downloading the Notebook

To start, download the [python2.ipynb]({{ "/splats/data/python2/python2.ipynb" | absolute_url }}) notebook file, save it in an appropriate folder (the download folder is probably not ideal) and open it through the jupyter file browser.

## Solutions

Below are example solutions for each of the trickier exercises of this workshop. Give it your best try before you have a look at these, as most of the learning effect will come from you attempting to apply what you learned.

In programming there is always “more than one way to skin a cat”, consequently, there are other possible solutions as well.

<details><summary>Reading the sequence file 1: Click to reveal code</summary>
{% highlight python %}
# edit the code to read the text file you previously downloaded
file = open('cds_adora1.txt', mode='r')
# Print the file contents line by line with a for-loop
for line in file:
    print(line, end='')
# close the file
file.close()
{% endhighlight %}
</details>

<details><summary>Reading the sequence file 2: Click to reveal code</summary>
{% highlight python %}
# Use the `with` annotation to print the contents of cds_adora.txt
with open('cds_adora1.txt','r') as file:
    for line in file:
        print(line, end='')
{% endhighlight %}
</details>

<details><summary>Reading the sequence file 3: Click to reveal code</summary>
{% highlight python %}
# Read the whole file content into a list using the `readlines()` method
with open('cds_adora1.txt','r') as file:
     lines = file.readlines()
print(lines)
{% endhighlight %}
</details>

<details><summary>Removing line breaks 1: Click to reveal code</summary>
{% highlight python %}
# To strip each item's whitespace, create a new empty list, iterate over
cds_list = []
for line in lines:
    line_without_whitespace = line.rstrip()
    cds_list.append(line_without_whitespace)
print (cds_list)
{% endhighlight %}
</details>
<details><summary>Removing line breaks 2: Click to reveal code</summary>
{% highlight python %}
# join `cds_list` into a single string `cds`
cds = ''.join(cds_list)
print(cds)
{% endhighlight %}
</details>
<details><summary>From DNA to RNA: Click to reveal code</summary>
{% highlight python %}
rna = cds.upper().replace('T','U')
print(rna)
{% endhighlight %}
</details>
<details><summary>Chopping RNA into codons: Click to reveal code</summary>
{% highlight python %}
rna_length = len(rna)
codons = [] # an empty list to store the codons
for base_index in range(0,rna_length,3):
    # slicey-dicey using base_index
    codon = rna[base_index:base_index+3]
    codons.append(codon)
print(codons)
{% endhighlight %}
</details>
<details><summary>Translating codons into amino acids: Click to reveal code</summary>
{% highlight python %}
protein = "" # an empty string to store the aa (could also use a list) 
for codon in codons:
    protein += codon_table[codon]
print(protein)
{% endhighlight %}
</details>
<details><summary>Make it functional - load file: Click to reveal code</summary>
{% highlight python %}
def file_to_string (filename):
  """Loads a textfile of DNA sequence, strips any right side
  white spaces and returns the sequence as a string"""
  cds = ''
  with open(filename,'r') as file:
    for line in file.readlines():
      cds += line.rstrip()
  return cds
{% endhighlight %}
</details>
<details><summary>Make it functional - upper case and RNA: Click to reveal code</summary>
{% highlight python %}
def dna_to_upper_rna (dna):
  """Converts a string of DNA into uppercase RNA"""
  rna = dna.upper().replace('T','U')
  return rna
{% endhighlight %}
</details>
<details><summary>Make it functional - translate RNA: Click to reveal code</summary>
{% highlight python %}
def translate_rna (rna):
  """Translates a string of uppercase RNA
  returns a string of amino acids"""
  codon_table  = {"GCA":"A", "GCC":"A", "GCG":"A", "GCU":"A",
          "UGC":"C", "UGU":"C", "GAC":"D", "GAU":"D",
          "GAA":"E", "GAG":"E", "UUC":"F", "UUU":"F",
          "GGA":"G", "GGC":"G", "GGG":"G", "GGU":"G",
          "CAC":"H", "CAU":"H", "AUA":"I", "AUC":"I",
          "AUU":"I", "AAA":"K", "AAG":"K", "UUA":"L",
          "UUG":"L", "CUA":"L", "CUC":"L", "CUG":"L",
          "CUU":"L", "AUG":"M", "AAC":"N", "AAU":"N",
          "CCA":"P", "CCC":"P" ,"CCG":"P", "CCU":"P",
          "CAA":"Q", "CAG":"Q", "AGA":"R", "AGG":"R",
          "CGA":"R", "CGC":"R", "CGU":"R", "CGG":"R",
          "AGC":"S", "AGU":"S", "UCA":"S", "UCC":"S",
          "UCG":"S", "UCU":"S", "ACA":"T", "ACC":"T",
          "ACG":"T", "ACU":"T", "GUA":"V", "GUC":"V",
          "GUG":"V", "GUU":"V", "UGG":"W", "UAC":"Y",
          "UAU":"Y", "UAG":"!", "UAA":"!", "UGA":"!"}
  rna_length = len(rna)
  protein = ''
  for b in range(0,rna_length,3):
    codon = rna[b:b+3]
    protein += codon_table[codon]
  return protein
{% endhighlight %}
</details>