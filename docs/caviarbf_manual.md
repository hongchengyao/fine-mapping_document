# caviarbf
## R version
Since the documentation of caviarbf is not clear enough and sometimes mixes the documentation of the C++ version and the R version this documentation serves as a supplement
### Dependencies
The R version depends on `Rcpp`, `RcppEigen` and `glmnet`, remember to install these package before installing caviarbf 

### Software Detail 
#### Usage

    caviarbfFineMapping(lociListFile, maxCausal, nSample, priorType01 = 0, priorValue = c(0.1, 0.2, 0.4), exact = T, eps = 0, useIdentityMatrix = F, BFFileListFile = NA, priorProb = 0, fittingMethod = "glmnetLASSOMin", hyperParamSelection = "cv", nFold = 5, K = 10, rThreshold = 0.2, pvalueThreshold = 0.05, lambda = c(2^seq(from = -15, by = 2, to = 5), 100, 1000, 10000, 1e+05, 1e+06), alpha = c(0, 0.2, 0.3, 0.5, 0.7, 0.8, 1), annotationIndices = -1, annotationSuffix = ".annotations", LDSuffix = ".LD", outputPrefix, keepBF = F, overwriteExistingResults = F, maxIter = 50, deltaLik = 0.01, useParallel = F, ncores = 1, verbose = F)

#### Argument

`lociListFile: A file listing all the z score files.`

`maxCausal: The assumed maximal number of causal variants in each locus`

`nSample: The number of individuals used to calculate summary statistics`

`priorType01: 0 to specify sigmaa. An experimental 1 is used to specify the proportion of the phenotype variance explained (pve)`

`priorValue: If priorType01 is 0, it specifies the value of sigmaa. Otherwise, it is the pve value. For sigmaa, a vector of values can be used and the final Bayes factors are averaged. For GWAS fine mapping, c(0.1, 0.2, 0.4) might be a good option. For eQTL, c(0.1, 0.2, 0.4, 0.8, 1.6) might be a good option to accout for large eQTL effects.`

`exact: Whether to calculate the exact Bayes factors. This is only valid for quantitative traits. For binary traits, only approximated Bayes factors are available`

`eps: The small value to add to the correlation matrix. This is useful when the LD matrix is from a reference panel`

`useIdentityMatrix: If true, use the identity matrix as the LD matrix. This improves the computing speed when only considering one causal variant for each locus`

`BFFileListFile: Specify the files of precomputed Bayes factors corresponding to each locus in the lociListFile. If NA, the Bayes factors will be calculated first. Bayes factors can precomputed for each locus using the C++ program This has the advantage of running in parallel, and can saved for later fine mapping.`

`priorProb: The initial probability of each variant being causal. If it is 0, the initial probability will be the number of loci divided by the number of all variants`

`fittingMethod: Specify different penalization models: glmnetLASSOMin for L1 penalization, glmnetRidgeMin for L2 penalization, glmnetENETMin for elastic net penalization. This is only used when hyperParamSelection is "cv".`

`hyperParamSelection: Method to select hyperparameter lambda and alpha. "cv" for loci based cross validation to select lambda and alpha, "aic", "bic", "bic2" uses AIC, BIC models. For the BIC model, "bic2" use the number of SNPs as the number of data points, "bic" uses the number of individuals as the number of data points, but they often show similar results. "topK" produces the top significant and relative independent annotations. No penalization is used in the fitting. As a by-product, it also outputs the pvalues for each individual annotation when considered separately`

`nFold: The fold of cross validation, only used when hyperParamSelection is "cv". If the number of loci is less than then nFold, the number of loci is used`

`K: Specify the top K annotations to select, used for "topK"`

`rThreshold: Specify the upper bound of the correlation among top annotations, used for "topK"`

`pvalueThreshold: Specify the p value threshold for top annotations, used for "topK"`

`lambda: The lambda values for parameter selection. If lambda is 0, then no penalization is used in the fitting. This can be used for statistical testing, e.g., comparing two nested annotation models.`

`alpha: The alpha values for parameter selection, only used when the fitting method is glmnetENETMin`

`annotationIndices: A vector to specify annotations. If it is -1, use all annotations, 0 for only the intercept, otherwise it specifies the corresponding columns of annotations. Default uses all annotations.`

`annotationSuffix: The annotation suffix, default ".annotations"`

`LDSuffix: The correlation file suffix, default ".LD"`

`outputPrefix: The prefix of the output files`

`keepBF: Whether to keep produced Bayes factor files`

`overwriteExistingResults: Whether to overwrite existing output files`

`maxIter: The maximal iteration in the EM method`

`deltaLik: The minimum improvement of log likelihood to continue the EM iteration`

`useParallel: Whether to use library parallel to use multiple cores This can increase the speed of calculate PIPs in the EM iteration`

`ncores: Number of cores to use in parallel when useParallel is T`

`verbose: If true, more information are printed`
 
#### value
 
    The results are saved in several files.  
    
    <outputPrefix>.marginal
    saves the indices of all variants following the order in the list file and the their PIPs sorted in a descring order.
    
    <outputPrefix>.marginalz includes the extra region index, the
         variant ID and the z score.  <outputPrefix>.loglik stores the
         relative log likelihood.  <outputPrefix>.gamma stores the
         estimated annotation effect size and the effect size of
         standardized annotation.  <outputPrefix>.log stores the running
         log.  <outputPrefix>.time saves the running time.

