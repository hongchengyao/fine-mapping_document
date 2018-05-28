# Finemapping Tutorial 
## Input Requirement
### PAINTOR
First line must be the header, only Zscore is required, other information could also be added. However, LD matrix is necessary for PAINTOR, if you have the raw genotype data, you can generate the LD matrix yourself, but if you don't, you can use the 1000 Genome data as the reference and use the script provided by PAINTOR to generate the LD matrix  
Note: Snps in the fine-mapping file should be in the same order as in the LD matrix file 
#### For finemapping alone
Zscore [**required**]  

#### Recommended format(some may not be necessary)
snpid  
chr  
pos  
effective allele  
baseline allele  
effect size  
standard error  
Pvalue  
Zscore

### fgwas
fgwas does not require a LD matrix file, and it has different input requirement for quantitative trait and case-control trait  
Note:  
#### Quantitative trait
The columns have no enforced order, but are identified from the header  
##### Type 1
SNPID [**required**]  
CHR [**required**]  
POS [**required**]  
F [**required**]allele frequency of one of the alleles of the SNP  
Z [**required**]  Zscore  
N [**required**] sample size
##### Type 2
SNPID [**required**]  
CHR [**required**]  
POS [**required**]  
F [**required but not used**] allele frequency of one of the alleles of the SNP  
Z [**required**]  Zscore  
N [**required but not used**] sample size  
SE [**required**]  

Note: for type 2, even though input SE will override F and N, you still need to add these two columns otherwise the program will raise an error. If you do not have the real value of F and N, you can add some pseudo values since they will not be used
### CAVIAR

### CAVIARBF
## 1. File formatting

## 2. File splitting
## 3. LD matrix generation
Note: Snps in the fine-mapping file should be in the same order as in the LD matrix file
#### For LD matrix generation using PAINTOR script
chr [**required**] it is used to choose the reference file   
pos [**required**]  
effective allele [**required**]  
baseline allele [**required**]  
Zscore [**required**]  
1000g reference [**required**]  
1000g panel file [**required**]  
population ethnicity [**required**]  
## 4. Functional annotation generation
#### For functional annotation matrix generation using PAINTOR script
chr [**required**]  
pos [**required**]  
