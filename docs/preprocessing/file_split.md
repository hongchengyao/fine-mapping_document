# File splitting
The idea of file splitting is to split the whole genome into loci. There are several ways to split the genome, and here the systematic way is to use the [approximately independent LD block](https://bitbucket.org/nygcresearch/ldetect-data).  
## Approximately independent LD block
1) Read the range of each LD block from the approximately independent LD block file  
2) Use tabix to extract the SNPs from the GWAS file according to the LD block range  
3) Filter loci by Pvalue/Zscore since there are ~1500 LD blocks  

### Example code  
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
## Other methods
It is not necessary to split the genome into loci by approximately independent LD block. Sometimes people may just select the lead SNP and include SNPs within 500K bp of the lead SNP on both sides. If you have already define the loci somehow, it's OK to just skip this step. 




