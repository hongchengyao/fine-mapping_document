# FINEMAP
FINEMAP only works for **SINGLE** locus finemapping **WITHOUT** functional annotations.

## Running Process
1. Prepare Z file, LD file and other optional input files as described in [Input file format](http://127.0.0.1:8000/FINEMAP/#input-file-format)
2. Define datasets in the `master` file, each dataset correspond to one locus
3. Run FINEMAP program, get the results

### Notes
1. There are 3 subprogram which you need to choose from, `--cond` (stepwise conditional search), `--config` (Evaluate a single causal configuration without performing shotgun stochastic search), `--sss` (Fine-mapping with shotgun stochastic search), `--sss` is the most commonly used mode when performing finemapping
2. `--corr-config` This is the option to set the posterior probability of a causal configuration to zero if it includes a pair of SNPs with absolute correlation above this threshold. This option is **required** with a default of 0.95. It is necessary because of the algorithm implementation. If a pair of SNPs is perfectly or near perfectly correlated, an important matrix (Rcc) will become not invertible. 


## Argument
- `--cond` Fine-mapping with stepwise conditional search
    - Subprogram
- `--config` Evaluate a single causal configuration without performing shotgun stochastic search
    - Subprogram
- `--corr-config` Option to set the posterior probability of a causal configuration to zero if it includes a pair of SNPs with absolute correlation above this threshold
    - Default: 0.95
- `--corr-group` Option to set the threshold for grouping a pair of SNPs with absolute correlation above this threshold
    - Default: 0.99
- `--dataset` Option to specify a delimiter-separated list of datasets for fine-mapping as given in the master file (e.g. 1,2 or 1|2)
    - Default: All datasets will be processed
- `--flip-beta` Option to read a column 'flip' in the Z file with binary indicators specifying if the direction of the estimated SNP effect sizes needs to be flipped
    - With --cond, --config and --sss
- `--group-snps` Option to group SNPs on the basis of their correlations
    - With --cond and --sss
- `--in-files` Option to specify a semicolon separated master file with the following column names: 'z', 'ld', 'snp', 'config', 'n_samples' and optionally 'k' and 'log'. Each line is a dataset with file extensions corresponding with column names. The column 'n_samples' represents the GWAS sample size
    - With --cond, --config and --sss
- `--log` Option to write output to log files specified in column 'log' in the master file
    - No log files are written by default
- `--n-causal-snps` Option to set the maximum number of allowed causal SNPs
    - Default: 5
- `--n-configs-top` Option to set the number of top causal configurations to be saved
    - Default: 50000
- `--n-convergence` Option to set the number of iterations that the added probability mass is required to be below the specified threshold (--prob-tol) before shotgun stochastic search is terminated
    - Default: 1000
- `--n-iterations` Option to set the maximum number of iterations before shotgun stochastic search is terminated
    - Default: 100000
- `--prior-k` Option to use prior probabilities for the number of causal SNPs from K files as specified in column 'k' in the master file
    - SNPs are by default assumed to be causal with probability 1/(# of SNPs in the region)
- `--prior-k0` Option to set the prior probability that there is no causal SNP in the genomic region. Only used when computing posterior probabilities for the number of causal SNPs but not during fine-mapping itself
    - Default: 0.0
- `--prior-std` Option to specify a comma-separated list of prior standard deviations of effect sizes
    - Default: 0.05
- `--prob-tol` Option to set the tolerance at which the added probability mass (over --n-convergence iterations) is considered small enough to terminate shotgun stochastic search
    - Default: 0.001
- `--rsids` Option to specify a comma-separated list of SNP identifiers corresponding with the 'rsid' column in Z files as specified in column 'z' in the master file
    - Required with `--config`
- `--sss` Fine-mapping with shotgun stochastic search
    - Subprogram

## Input file format
### Input requirement
- Master file (**required**)
    - Used to specify all information needed
- Z file (**required**)
    - The `dataset.z` file is a space-delimited text file and contains the GWAS summary statistics one SNP per line
- LD file (**required**)
    - The `dataset.ld` file is a space-delimited text file and contains the SNP correlation matrix (Pearson's correlation)
- K file (*optional*)
    - specify prior probabilities for the number of causal SNPs in the genomic region by using a `dataset.k` file
- BGEN, BGI, SAMPLE and INCL file (*optional*)
    - These are Oxford file formats, details see link  [BGEN](https://www.well.ox.ac.uk/~gav/bgen_format/), [BGI](https://bitbucket.org/gavinband/bgen/wiki/The_bgenix_index_file_format). The `dataset.incl` file is a text file to restrict estimation of SNP correlations to genotype data from a subset of samples in `dataset.sample`. It contains one sample ID per line

#### Master file

The `master` file is a semicolon-separated text file and contains no space. It contains the following mandatory column names and one dataset per line.

##### Input
- `z` column contains the names of Z files (**required**)
- `ld` column contains the names of LD files (**required**)
- `n_samples` column contains the GWAS sample sizes (**required**)
- `k` column contains the optional K files (*optional*)
- `bgen` column contains the names of BGEN files (*optional*)
- `bgi` column contains the names of BGI files (*optional*)
- `sample` column contains the names of SAMPLE files (*optional*)
- `incl` column contains the names of INCL files (*optional*)

!!! Note

    File extensions must correspond with the column names in the header line!
    
##### Output
- `snp` column contains the names of SNP files (**required**)
- `config` column contains the names of CONFIG files (**required**)
- `cred` column contains the names of CRED files (**required**)
- `dose` column contains the names of DOSE files (*optional*)
- `log` column contains the optional LOG files (*optional*)

##### Example

- A `master` file with two datasets using precomputed SNP correlations could look as follows.

```
z;ld;snp;config;cred;log;n_samples
dataset1.z;dataset1.ld;dataset1.snp;dataset1.config;dataset1.cred;dataset1.log;5363
dataset2.z;dataset2.ld;dataset2.snp;dataset2.config;dataset2.cred;dataset2.log;5363
```

- A `master` file with two datasets using precomputed SNP correlations in the first dataset and BGEN support in the second dataset could look as follows.

```
z;ld;bgen;bgi;dose;snp;config;cred;log;n_samples
dataset1.z;dataset1.ld;;;;dataset1.snp;dataset1.config;dataset1.cred;dataset1.log;5363
dataset2.z;;dataset2.bgen;dataset2.bgi;dataset2.dose;dataset2.snp;dataset2.config;dataset2.cred;dataset2.log;5363
```

- A `master` file with one datasets using BGEN support and a subset of 5,000 samples could look as follows.

```
z;bgen;bgi;dose;sample;incl;snp;config;cred;log;n_samples
dataset2.z;dataset2.bgen;dataset2.bgi;dataset2.dose;dataset.sample;dataset.incl;dataset.snp;dataset.config;dataset.cred;dataset.log;5000
```

#### Z file
The `dataset.z file` is a space-delimited text file and contains the GWAS summary statistics one SNP per line. It contains the mandatory column names in the following order.

- `rsid` (*can be specified arbitrarily*) column contains the SNP identifiers. The identifier can be a rsID number or a combination of chromosome name and genomic position (e.g. XXX:yyy)
- `chromosome` (*can be specified arbitrarily*) column contains the chromosome names. The chromosome names can be chosen freely with precomputed SNP correlations (e.g. 'X', '0X' or 'chrX')
- `position` (*can be specified arbitrarily*) column contains the base pair positions
- `allele1` (*can be specified arbitrarily*) column contains the "first" allele of the SNPs. In SNPTEST this corresponds to 'allele_A', whereas BOLT-LMM uses 'ALLELE1'
- `allele2` (*can be specified arbitrarily*) column contains the "second" allele of the SNPs. In SNPTEST this corresponds to 'allele_B', whereas BOLT-LMM uses 'ALLELE0'
- `maf` (**needed** to output posterior effect size estimates on the allelic scale) column contains the minor allele frequencies
- `beta` (**required**) column contains the estimated effect sizes as given by GWAS software
- `se` (**required**) column contains the standard errors of effect sizes as given by GWAS software
- `flip` optional column - see below 

##### Example
- A `dataset.z` file with three SNPs could look as follows.

```
rsid chromosome position allele1 allele2 maf beta se
rs1 10 1 T C 0.35 0.0050 0.0208
rs2 10 1 A G 0.04 0.0368 0.0761
rs3 10 1 G A 0.18 0.0228 0.0199
```


### Example
## Output file format