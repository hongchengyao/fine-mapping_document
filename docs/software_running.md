# Software running
---
As abovementioned, after preprocessing, running fine-mapping software is usually straightforward and you are recommended to look through each software's documentation  
## [PAINTOR](https://github.com/gkichaev/PAINTOR_V3.0/wiki)  
---
### Example code

	paintor -input input_loci_list
	-in input_dir
	-out output_dir
	-Zhead Zscore
	-LDname ld
	-enumerate 1
	-annotations annotation1,annotation2,annotation3

## [fgwas](https://github.com/joepickrell/fgwas/blob/master/man/fgwas_manual.pdf)  
---

!!! Note

    fgwas does NOT allow SNPs to have the same position and it will raise an error. Make sure SNPs have unique position before using fgwas


### Example code

	fgwas -i input_file
	-fine
	-print
	-w annotation1+annotation2
	-o outputfile
## [CAVIAR](http://genetics.cs.ucla.edu/caviar/manual.html)  
---
## [CAVIARBF](https://bitbucket.org/Wenan/caviarbf/src)
---


