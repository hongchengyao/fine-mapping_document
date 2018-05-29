# Finemapping Tutorial 
## Introduction - General process of GWAS fine-mapping  
### Preprocessing 
Usually this step is complex and time consuming, and this tutorial will **focus on this step**  

- File formatting  
- File splitting  
- LD matrix generation  
- Functional annotation matrix generation  

### Software running
After the preprocessing, running fine-mapping software is usually straightforward

- Specify parameters for different softwares

## 1. File formatting
### PAINTOR
#### Input requirement
- Locus file (contain Zscore)
- LD matrix file
- Annotation matrix file

For locus file, first line must be the header, only Zscore is required, other information could also be added.  
For LD matrix file, if you have the raw genotype data, you can generate the LD matrix yourself, but if you don't, you can use the 1000 Genome data as the reference and use the script provided by PAINTOR to generate the LD matrix  
Note: Snps in the locus file must be in the same order as in the LD matrix file and the annotation matrix file
#### If finemapping alone
Zscore [**required**]  

#### If using 1000G reference to generate LD matrix (most common case)
chr [**required**]  
pos [**required**]  
effective allele [**required**]  
baseline allele [**required**]   
Zscore [**required**]

### fgwas
#### Input requirement
- Only one file containing Zscore and functional annotation

fgwas does not require a LD matrix file, but it require the Zscore and the annotations to be included into a single file. And all loci should be merged into one file and identified by a SEGNUMBER column.

In addition, it has different input requirement for quantitative trait and case-control trait  

#### Quantitative trait
The columns have no enforced order, but are identified from the header  
##### Type 1
SNPID [**required**]  
CHR [**required**]  
POS [**required**]  
F [**required**]allele frequency of one of the alleles of the SNP  
Z [**required**]  Zscore  
N [**required**] sample size  
SEGNUMBER [**required**] each locus should have a unique SEGNUMBER, and the file should be ordered according to this column  
annotation1 [optional]  
annotation2 [optional]  
...

##### Type 2
SNPID [**required**]  
CHR [**required**]  
POS [**required**]  
F [**required but not used**] allele frequency of one of the alleles of the SNP  
Z [**required**]  Zscore  
N [**required but not used**] sample size  
SE [**required**]  
SEGNUMBER [**required**] each locus should have a unique SEGNUMBER, and the file should be ordered according to this column  
annotation1 [optional]  
annotation2 [optional]  
...

Note: for type 2, even though input SE will override F and N, you still need to add these two columns otherwise the program will raise an error. If you do not have the real value of F and N, you can add some pseudo values since they will not be used
#### Case/control studies
##### Type 1
SNPID [**required**]  
CHR [**required**]  
POS [**required**]  
F [**required**]allele frequency of one of the alleles of the SNP  
Z [**required**]  Zscore  
NCASE [**required**] number of cases used in the association study at this SNP  
NCONTROL  [**required**] number of controls used in the association study at this SNP  
SEGNUMBER [**required**] each locus should have a unique SEGNUMBER, and the file should be ordered according to this column  
annotation1 [optional]  
annotation2 [optional]  
...

##### Type 2
SNPID [**required**]  
CHR [**required**]  
POS [**required**]  
F [**required but not used**]allele frequency of one of the alleles of the SNP  
Z [**required**]  Zscore  
NCASE [**required but not used**] number of cases used in the association study at this SNP  
NCONTROL  [**required but not used**] number of controls used in the association study at this SNP  
SE [**required**]  
SEGNUMBER [**required**] each locus should have a unique SEGNUMBER, and the file should be ordered according to this column  
annotation1 [optional]  
annotation2 [optional]  
...

Note: for type 2, even though input SE will override F and NCASE and NCONTROL, you still need to add these two columns otherwise the program will raise an error. If you do not have the real value of F and N, you can add some pseudo values since they will not be used
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
### If using PAINTOR script to generate functional annotation matrix
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

### If using bedtools to generate functional annotation matrix
to be added

## 5. Software running
As abovementioned, after preprocessing, running fine-mapping software is usually straightforward and you are recommended to look through each software's documentation  
### [PAINTOR](https://github.com/joepickrell/fgwas/blob/master/man/fgwas_manual.pdf)  
#### Example code

	paintor -input input_loci_list
	-in input_dir
	-out output_dir
	-Zhead Zscore
	-LDname ld
	-enumerate 1
	-annotations annotation1,annotation2,annotation3

###[fgwas](https://github.com/joepickrell/fgwas/blob/master/man/fgwas_manual.pdf)  
#### Example code

	fgwas -i input_file
	-fine
	-print
	-w annotation1+annotation2
	-o outputfile
### [CAVIAR](http://genetics.cs.ucla.edu/caviar/manual.html)  
### [CAVIARBF](https://bitbucket.org/Wenan/caviarbf/src)

## Reference
### Approximately independent linkage disequilibrium blocks in human populations
Paper: [Pickrell et al. Bioinformatics. 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4731402/)  
Website: <https://bitbucket.org/nygcresearch/ldetect-data/src>
### PAINTOR
Paper: [Kichaev et al. PLOS Genetics, 2014](http://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1004722), [Kichaev et al. American Journal of Human Genetics, 2015](https://www.cell.com/ajhg/fulltext/S0002-9297(15)00243-8), [Kichaev et al. Bioinformatics, 2016](https://academic.oup.com/bioinformatics/article/33/2/248/2525720)  
Website: <https://github.com/gkichaev/PAINTOR_V3.0>