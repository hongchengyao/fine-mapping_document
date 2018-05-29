# Finemapping Tutorial 
## 1. Input Requirement
### PAINTOR
First line must be the header, only Zscore is required, other information could also be added. However, LD matrix is necessary for PAINTOR, if you have the raw genotype data, you can generate the LD matrix yourself, but if you don't, you can use the 1000 Genome data as the reference and use the script provided by PAINTOR to generate the LD matrix  
Note: Snps in the fine-mapping file must be in the same order as in the LD matrix file 
#### If finemapping alone
Zscore [**required**]  

#### If using 1000G reference to generate LD matrix (most common case)
chr [**required**]  
pos [**required**]  
effective allele [**required**]  
baseline allele [**required**]   
Zscore [**required**]

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
to be added
### CAVIARBF
to be added
## 2. File splitting
The idea of file splitting is to split the whole genome into loci. There are several ways to split the genome, and here the systematic way is to use the [approximately independent LD block](https://bitbucket.org/nygcresearch/ldetect-data).  
### Approximately independent LD block
1) Read the range of each LD block from the approximately independent LD block file  
2) Use tabix to extract the SNPs from the GWAS file according to the LD block range  
3) Filter loci by Pvalue/Zscore since there are ~1500 LD blocks  

#### Example code  
Note: This should not be used directly since it applies to specific files

	bgzip gwas_summary_temp_sorted
	tabix -s 1 -b 2 -e 2 -S 1 gwas_summary_temp_sorted.gz
	cat fourier_ls-all.bed | while read line
	do
		array=(${line})
		chrnum=${array[0]#*chr}
		chrnum=${chrnum// /}
		start=${array[1]// /}
		end=${array[2]// /}
		if [ "${start}" != "start" ]; then
		tabix gwas_summary_temp_sorted.gz ${chrnum}:${start}-${end} > output.${chrnum}_${start}_${end}
	done
### Other methods
It is not necessary to split the genome into loci by approximately independent LD block. Sometimes people may just select the lead SNP and include SNPs within 500K bp of the lead SNP on both sides. If you have already define the loci somehow, it's OK to just skip this step. 

## 3. LD matrix generation
Note: Snps in the fine-mapping file should be in the same order as in the LD matrix file
### LD matrix generation using PAINTOR script
chr [**required**] it is used to choose the reference file   
pos [**required**]  
effective allele [**required**]  
baseline allele [**required**]  
Zscore [**required**]  
1000g reference [**required**]  
1000g panel file [**required**]  
population ethnicity [**required**]  

#### Example code
Note: This should not be used directly since it applies to specific files

	python CalcLD_1KG_VCF.py
	--locus gwas_summary_3_4750808.processed
	--reference ALL.chr3.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz
	--map integrated_call_samples_v3.20130502.ALL.panel
	--effect_allele effect_allele
	--alt_allele baseline_allele
	--population EAS
	--Zhead Zscore
	--out_name gwas_summary_3_4750808.processed
	--position pos

### LD matrix generation using raw genotype data and Plink
to be added


## 4. Functional annotation generation
### For functional annotation matrix generation using PAINTOR script
chr [**required**]  
pos [**required**]  
#### Example code
Note: This should not be used directly since it applies to specific files

	python AnnotateLocus.py
	--input annotation_paths_list
	--locus gwas_summary_3_4750808.processed
	--out gwas_summary_3_4750808.processed.annotations
	--chr chr
	--pos pos




## Reference paper
[Approximately independent linkage disequilibrium blocks in human populations](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4731402/)