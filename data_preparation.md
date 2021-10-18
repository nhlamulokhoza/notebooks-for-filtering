# Filtering .tsv files with Python 

load python libraries and modules and convert .tsv file from python to .csv file

```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt

with open("file.tsv", "r") as input_file:
    for line in input_file:
        with open("file.csv", "a") as output:
            line = line.replace("\t", ",")
            output.write(line)
```

read .csv file and drop unwanted columns

```python
d = pd.read_csv('file.csv')
d = d.drop(columns=['ReferenceSequence', 'Variant Stop', 'Type'])
```

Select haplotypes of interest and reset file index. VCF files from PharmVar can be run on VEP and one can filter for specific features - this should return the names of haplotypes of intereest. 

```python
data = d.loc[(d['Haplotype Name'] == 'haplotype name') | (d['Haplotype Name'] == 'haplotype name 2')]
data.reset_index(drop = True, inplace = True)
```

Select positions of genomic positions of interest

```python
filtered = data.loc[(data['Variant Start'] == 'variant position') | (data['Variant Start'] == 'variant position 2')]
```

write the results to a .csv file

```python
filtered.to_csv('star_variants.csv')
```

------

## Filtering for unique varaiants based on rsID

The produced file star_variants.csv may produce a dataframe with redundant variants. In the instence that one is looking for a non-redundant dataframe, additional filters may be applied. 

to filter for unique variants (by rsID) from the the produced star_allele.csv file, you may run the script: 

```python
data = pd.read_csv('star_variants.csv')
data = data.drop_duplicates(subset = ["rsID"])
data.to_csv('star_unique_variants.csv')
```

this scrip will produce the csv file star_unique_variants.csv
