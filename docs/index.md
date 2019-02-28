# Introduction
---

## General process of GWAS fine-mapping  

### Data preprocessing 
Usually this step is complex and time consuming, and this tutorial will **focus on this step**
#### PAINTOR
- File formatting  
- File splitting  
- LD matrix generation  
- Functional annotation matrix generation  

#### fgwas

- File formatting
- File splitting
    - The AnnotateLocus.py of PAINTOR is applied to SNPs from the same chromosome, so you either split the GWAS file into chromosomes or split it into loci in order to use AnnotateLocus.py to generate functional annotation matrix
    - Although fgwas does not require LD matrix file, it's still necessary to define loci and assign SEGNUMBER to each locus
- Functional annotation matrix generation

### Software running
After the preprocessing, running fine-mapping software is usually straightforward

- Specify parameters for different softwares



