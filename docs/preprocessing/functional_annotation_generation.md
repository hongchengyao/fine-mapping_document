# Functional annotation generation
---
The generation of functional annotation matrix relies on two types of information (files)

1. The functional annotation bed files
2. The chromosome and position information of each SNP from GWAS summary data file

## If using PAINTOR script to generate functional annotation matrix
| Header | Option | Note |
|---|---|---|
| chr | **required** | |
| pos | **required** | |

!!! Note

    1. Make sure that the functional annotation bed file are sorted, because the AnnotateLocus.py script used a specific algorithm and unsorted functional annotation bed file will lead to wrong function annotation matrix.
    2. Make sure that the chromosome nomenclature is the same in the GWAS file and the functional annotation bed file. e.g. both should be chr1 or both should be 1

### Example code

!!! Note

    This should not be used directly since it applies to specific files
    
```
	python AnnotateLocus.py
	--input annotation_paths_list
	--locus gwas_summary_3_4750808.processed
	--out gwas_summary_3_4750808.processed.annotations
	--chr chr
	--pos pos
```
## If using bedtools to generate functional annotation matrix
to be added




