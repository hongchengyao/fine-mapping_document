# LD matrix generation
---

!!! Note
    
    Snps in the fine-mapping file should be in the same order as in the LD matrix file
    
## LD matrix generation using PAINTOR script
---

| Header | Option | Note |
| --- | --- | --- |
| chr | **required** | Used to choose the reference file |
| pos | **required** |  |
| effective allele | **required** ||
| baseline allele | **required** ||
| Zscore | **required** ||
| 1000g reference | **required** ||
| 1000g panel file | **required** ||
| population ethnicity | **required** ||

Usually 1000 genome reference file are splitted into chromosomes, and CalcLD_1KG_VCF.py need to specify reference with matched chromosome

### Example code

!!! Note
    
    This should not be used directly since it applies to specific files

```
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
```
## LD matrix generation using raw genotype data and Plink
to be added



