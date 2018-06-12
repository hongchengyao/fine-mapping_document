# Functional annotation generation
---
The generation of functional annotation matrix relies on two types of information (files)

1. The functional annotation bed files
2. The chromosome and position information of each SNP from GWAS summary data file

## If using PAINTOR script to generate functional annotation matrix
chr [**required**]  
pos [**required**]  
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




