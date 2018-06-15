# File formatting

## PAINTOR
---

### Input requirement
- Locus file (contain Zscore)
- LD matrix file
- Annotation matrix file

For locus file, first line must be the header, only Zscore is required, other information could also be added.  
For LD matrix file, if you have the raw genotype data, you can generate the LD matrix yourself, but if you don't, you can use the 1000 Genome data as the reference and use the script provided by PAINTOR to generate the LD matrix  
Note: Snps in the locus file must be in the same order as in the LD matrix file and the annotation matrix file

### If finemapping alone

| Header | Option       | Note |
|--------|--------------|------|
| Zscore | **required** |      |

### If using 1000G reference to generate LD matrix (most common case)

| Header           | Option       | Note |
|------------------|--------------|------|
| chr              | **required** |      |
| pos              | **required** |      |
| effective allele | **required** |      |
| baseline allele  | **required** |      |
| Zscore           | **required** |      |

## fgwas
---

### Input requirement
- Only one file containing both Zscore and functional annotation

fgwas does **NOT** require a LD matrix file, but it require the Zscore and the annotations to be incorporated into **ONE** file. And all loci should be merged into **ONE** file and identified by a SEGNUMBER column.

In addition, it has different input requirement for quantitative trait and case-control trait  

### Quantitative trait
The columns have no enforced order, but are identified from the header  

#### Type 1

| Header      | Option       | Note |
|-------------|--------------|---------------------------------------------------------|
| SNPID       | **required** | |
| CHR         | **required** | |
| POS         | **required** | |
| F           | **required** | allele frequency of one of the alleles of the SNP |
| Z           | **required** | Zscore |
| N           | **required** | sample size |
| SEGNUMBER   | **required** | each locus should have a unique SEGNUMBER, and the file should be ordered accordingly |
| annotation1 | optional     | |
| annotation2 | optional     | |
| ...         |              | |

#### Type 2

| Header      | Option       | Note                                                                                  |
|-------------|--------------|--------------------------|
| SNPID       | **required** | |
| CHR         | **required** | |
| POS         | **required** | |
| F           | **required but not used** | allele frequency of one of the alleles of the SNP |
| Z           | **required** | Zscore |
| N           | **required but not used** | sample size |
| SE          | **required** | standard error |
| SEGNUMBER   | **required** | each locus should have a unique SEGNUMBER, and the file should be ordered accordingly |
| annotation1 | optional     | |
| annotation2 | optional     | |
| ...         |              | |

!!! Warning
    
    for type 2, even though input SE will override F and N, you still need to add these two columns otherwise the program will raise an error. If you do not have the real value of F and N, you can add some pseudo values since they will not be used
    
### Case/control studies

#### Type 1
| Header | Option | Note |
|---|---|---|
| SNPID | **required** | |
| CHR | **required** | |
| POS | **required**| |
| F | **required** | allele frequency of one of the alleles of the SNP |
| Z | **required** | Zscore |
| NCASE | **required** | number of cases used in the association study at this SNP |
| NCONTROL | **required** | number of controls used in the association study at this SNP |  
| SEGNUMBER | **required** | each locus should have a unique SEGNUMBER, and the file should be ordered according to this column |
| annotation1 | optional | |
| annotation2 | optional | |
| ... | | |

#### Type 2

| Header | Option | Note |
|---|---|---|
| SNPID | **required** | |
| CHR | **required** | |
| POS | **required** | |
| F | **required but not used** | allele frequency of one of the alleles of the SNP |
| Z | **required** | Zscore |
| NCASE | **required but not used** | number of cases used in the association study at this SNP | 
| NCONTROL | **required but not used** | number of controls used in the association study at this SNP |  
| SE | **required** | standard error | |
| SEGNUMBER | **required** | each locus should have a unique SEGNUMBER, and the file should be ordered according to this column |
| annotation1 | optional | |
| annotation2 | optional | |
| ... | | |

!!! Warning

    For type 2, even though input SE will override F and NCASE and NCONTROL, you still need to add these two columns otherwise the program will raise an error. If you do not have the real value of F and N, you can add some pseudo values since they will not be used

## CAVIAR
---
to be added

## CAVIARBF
---
to be added


