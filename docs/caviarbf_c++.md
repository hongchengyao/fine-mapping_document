# CAVIARBF C++ version
The C++ version only works for **SINGLE** locus finemapping **WITHOUT** functional annotations.

---

## Running process
1. Prepare all the required files as in [Input file format](caviarbf_c++.md#input-file-format).
2. Run executable program `caviarbf`, and generate the Bayes factor file
3. Use the Bayes factor file from former step and other files (if necessary), run executable program `model_search`, generate the final output files

### Example
#### Running `caviarbf`
    ./caviarbf -z ./example/myfile.Z -r ./example/myfile.LD -t 0 -a 0.1281429 -n 2000 -c 5 -o ./example/myfile.sigma0.1281429.bf
    
In this example, we specify `-z`, `-r` (use `-r` since `-c` is set to 5), `-t` (use 0 which is fully tested), `-a` (set to 0.1281429 which is close to the recommended value 0.1), `-n`, `-c`, `-o`
#### Running `model_search`
    ./model_search -i ./example/pref4.multi.txt -m 50 -p 0 –o ./example/pref4.multi.txt.prior0 
## For `caviarbf`

### Argument

!!! Note

    Be careful about setting `-e` when using a reference panel to generate LD matrix
    
- `-z` (**required**) input file for marginal test statistics
- `-i` (either -i or -r is **required**) use the identity matrix for the correlation matrix, useful for c = 1, i.e., assume only 1 causal variant
- `-r` (either -i or -r is **required**) input file for correlation matrix
    - If `-i` is used, i.e., use identity matrix for the correlation matrix, then there is no need to use `-r` to specify the input file for correlation matrix
    - If `-i` is not used, `-r` is **required**
- `-t` (**required**) prior type for variant effect size
    - 0: specify sigmaa, sigmaa relates to the variance of effect size
    - 1: specify the proportion of variance explained (pve)
    - For -t option, 0 (setting sigmaa) is fully tested. 1 is in an experimental stage. When using sigmaa, 0.1 seems to be a generally good value for -a for GWAS. Other values can be tried include 0.2, 0.4 as recommended by BIMBAM or use the robust multiple value version 0.1,0.2,0.4. For eQTL analysis, a multiple value option 0.1,0.2,0.4,0.8,1.6 is reasonable.
- `-a` (**required**) the prior values associated with the prior type
    - Option `-a` now can support multiple values, for example, -a 0.1,0.2,0.4 to allow multiple values to be used and the final Bayes factor is an average among different values. This will be more robust when we are uncertain about the effect size.
- `-n` (**required**) the total number of samples in the data
- `-c` (**required**) the maximal number of causal variants in the model
    - When running on many SNPs, be careful not to set the `-c` option too large because it may take too much time and also space to store the output. You can start with 1 or 2 and increase it by 1 in each trial. If two settings show similar results, then there is no need to increase it further.
- `-o` (**required**) the output file name for Bayes factors
- `--appr` (*optional*) calculate the approximated Bayes factors instead of the exact
    - For **quanttatve trait**, an **exact** Bayes factor can be calculated instead of an **approximate** Bayes factor. This is critical when the sample size is small or the effect size is large, for example, in expression quantitative trait loci (eQTL) analysis. To use the old approximate Bayes factors, use the option --appr. Use the **approximate** Bayes factor for a **binary trait** because **NO** exact Bayes factor is available.
- `-e` (*optional*) the value added to the diagonal of the correlation matrix. A recommendded value is 0.1 when using estimated correlation, e.g., from the 1000 genomes project. This can improve the performance when the correlaton matrix is an approximaton, such as from a reference panel
    - Default: 0, if using exact Bayes factors
    - Default: 1e-3, if using approximated Bayes factors
- `--` Ignores the rest of the labeled arguments following this flag
- `--version` Displays version information and exits
- `-h` Displays usage information and exits
    
### Input file format
#### Input requirement
- Marginal test statistics file (Z-score file, **required**)
    - A text file with 2 or 3 columns separated by spaces
- Correlation matrix file (LD matrix file, if use identity matrix for the correlation matrix, this is **NOT** necessary)
    - a matrix of the correlaton among variants

!!! Note

    The coding scheme in the calculation of the correlation (LD) matrix should be consistent with that of the z scores, i.e., the allele coded as 1 in the calculation of z scores needs to be coded as 1 in the calculation of the correlation (LD) matrix and vice versa. Because this is how everything is calculated when we have the full genotype data: everything calculated is based on the same coding scheme. Otherwise the results using summary statistics may be arbitrary.


#### Example
##### Marginal test statistics file
Note: This file should NOT contain header

A text file with 2 or 3 columns separated by spaces. The third column is optional, which specifies the variance of each variant. In this case, we assume the effect size is not related to the allele frequency of the variant. The result will be similar to BIMBAM. Without the third column of variance of each variant, we assume that the effect size is larger when the minor allele frequency is smaller.

    rs11922846 0.41199
    rs2587938 -0.631405
    rs1351949 0.269231
    rs2587940 -0.786003
    rs1825507 -0.514003
    
##### Correlation matrix file
Note: This file should NOT contain header

    1.0000e+00 3.4753e-01 1.0000e+00 3.4492e-01 4.3334e-01
    3.4753e-01 1.0000e+00 3.4753e-01 9.8494e-01 -2.3178e-01
    1.0000e+00 3.4753e-01 1.0000e+00 3.4492e-01 4.3334e-01
    3.4492e-01 9.8494e-01 3.4492e-01 1.0000e+00 -2.3003e-01
    4.3334e-01 -2.3178e-01 4.3334e-01 -2.3003e-01 1.0000e+00

### Output file format
The output file is a text file with Bayes factors. It has the same format as that from BIMBAM. 
#### Example
##### Bayes factor file
The first line is a comment. The second line is the header. The first column is the Bayes factor. The rest columns indicate which SNPs/variants are in the model. NA means not in the model.

    ## note:bf=log10(Bayes Factor), SNP IDs are from 1 ... m
    bf		se	snp1	snp2	snp3	snp4	snp5
    -0.591455	NA	1	NA	NA	NA	NA
    +4.658971	NA	2	NA	NA	NA	NA
    -0.127742	NA	3	NA	NA	NA	NA
    +7.052327	NA	4	NA	NA	NA	NA
    +1.289039	NA	5	NA	NA	NA	NA
    +2.519908	NA	6	NA	NA	NA	NA
    +9.619151	NA	1	2	NA	NA	NA
    -0.485480	NA	1	3	NA	NA	NA
    +6.794833	NA	1	4	NA	NA	NA
    +0.954240	NA	1	5	NA	NA	NA
    +2.145068	NA	1	6	NA	NA	NA
    +4.577784	NA	2	3	NA	NA	NA
    +12.322104	NA	2	4	NA	NA	NA
    +6.055098	NA	2	5	NA	NA	NA
    +7.274689	NA	2	6	NA	NA	NA
    +84.266736	NA	3	4	NA	NA	NA
    +1.568898	NA	3	5	NA	NA	NA
    +2.617624	NA	3	6	NA	NA	NA
    +7.998226	NA	4	5	NA	NA	NA
    +9.658610	NA	4	6	NA	NA	NA
    +3.923135	NA	5	6	NA	NA	NA

## For `model_search`
### Argument
- `-p` (either `-p` or `-f` is **required**) the prior probability of each SNP being causal, in the range [0, 1). If it is 0, the prior probability will be set to 1 / m, where m is the number of variants in the region
- `-f` (either `-p` or `-f` is **required**) the file name specifying the prior probabilities of variants
- `-i` (**required**) input file storing Bayes factors
- `-m` (**required**) the total number of variants in the data
- `-o` (**required**) output file prefix
- `-s` output stepwise result with rho confidence level
- `-e` output exhaustive search result with rho confidence level
- `-x` output result first using exhaustive and then stepwise search with rho confidence level
- `--` Ignores the rest of the labeled arguments following this flag
- `--version` Displays version information and exits
- `-h` Displays usage information and exits

### Input file format
#### Input requirement
- Bayes factor file (**required**)
    - The output file from `caviarbf`. Since the format is the same as the output from BIMBAM, it can also be used to process BIMBAM output Bayes factors
- Prior probability file (*optional*)
    - This file can be used to specify different priors of being causal for different variants. Each row corresponds to a variant in the region
    - If all the priors are the same, we can also use `-p` to specify the common prior directly


#### Example
##### Prior probability file
    
    0.02
    0.03
    0.02
    0.05
    0.02
    0.01

### Output file format
- Posterior inclusion probability (PIP) file
    - The file has a .marginal suffix. It has two columns. The first column is the index of each variant in the data. The second is the PIP in a descending order
- Statistics file
    - This file has a .statistics suffix, and contains some statistics information. The first is the ratio between the likelihood of the data averaging over all models to the likelihood of the data under the global null model: no variant is causal. The second is the probability of at least 1 causal variant in the region. The third is the Bayes factor of the region (all alternative models vs. the global null model).
- ρ-confidence level file (when specify `-s`)
    - This file has a .stepwise suffix. It has the same format as the PIP file. Each row shows the ρ-confidence level when including the current variant and all variants above this row. To generate this file you need to specify the `-s` option
- Other experimental output files (when specify `-e` or `-x`)
    - .exhaustive file outputs the best ρ-confidence level by exhaustive search of models for each model size. Due to the time cost, we only do that until model size 4. This needs the `-e` option to generate it.
    - .exhaustivestepwise file outputs the ρ-confidence level by first applying the exhaustive search up to model size 4 and then switching to a stepwise search. This needs the `-x` option

## Explanation of ρ-confidence level file
TBA
