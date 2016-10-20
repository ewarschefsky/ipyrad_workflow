#ipyrad workflow: Oct 13 2016

###Copy the data
The tarball is up on the cluster: 
Copy it to your current directory
```cp ~/Mangomics/mangomics.tar .```
Unwrap the tarball
```tar -xvf mangomics.tar```
This puts the files in a stupid directory called ```Project_Emily``` move them to the current directory
```mv Project_Emily/Sample* .```
Rename the files to correspond to the real library name


###Make a new blank/original params file for each sublibrary

```
ipyrad -n Lib_3
ipyrad -n Lib_6
ipyrad -n Lib_9
ipyrad -n Lib_11
ipyrad -n Lib_12
ipyrad -n Lib_T
ipyrad -n Lib_U
ipyrad -n Lib_V
ipyrad -n Lib_W
ipyrad -n Lib_X
ipyrad -n Lib_Y
ipyrad -n Lib_Z
```

The params file will look like:

```
------- ipyrad params file (v.0.3.42)-------------------------------------------
Lib_#	                           ## [0] [assembly_name]: Assembly name. Used to name output directories for assembly steps
							   ## [1] [project_dir]: Project dir (made in curdir if not present)
							   ## [2] [raw_fastq_path]: Location of raw non-demultiplexed fastq files
							   ## [3] [barcodes_path]: Location of barcodes file
							   ## [4] [sorted_fastq_path]: Location of demultiplexed/sorted fastq files
                   		       ## [5] [assembly_method]: Assembly method (denovo, reference, denovo+reference, denovo-reference)
                               ## [6] [reference_sequence]: Location of reference sequence file
          		               ## [7] [datatype]: Datatype (see docs): rad, gbs, ddrad, etc.
		                       ## [8] [restriction_overhang]: Restriction overhang (cut1,) or (cut1, cut2)
5                              ## [9] [max_low_qual_bases]: Max low quality base calls (Q<20) in a read
33                             ## [10] [phred_Qscore_offset]: phred Q score offset (33 is default and very standard)
6                              ## [11] [mindepth_statistical]: Min depth for statistical base calling
6                              ## [12] [mindepth_majrule]: Min depth for majority-rule base calling
10000                          ## [13] [maxdepth]: Max cluster depth within samples
0.85                           ## [14] [clust_threshold]: Clustering threshold for de novo assembly
0                              ## [15] [max_barcode_mismatch]: Max number of allowable mismatches in barcodes
1                              ## [16] [filter_adapters]: Filter for adapters/primers (1 or 2=stricter)
35                             ## [17] [filter_min_trim_len]: Min length of reads after adapter trim
2                              ## [18] [max_alleles_consens]: Max alleles per site in consensus sequences
5, 5                           ## [19] [max_Ns_consens]: Max Ns (uncalled bases) in consensus (R1, R2)
8, 8                           ## [20] [max_Hs_consens]: Max Hs (heterozygotes) in consensus (R1, R2)
4                              ## [21] [min_samples_locus]: Min # samples per locus for output
20, 20                         ## [22] [max_SNPs_locus]: Max # SNPs per locus (R1, R2)
8, 8                           ## [23] [max_Indels_locus]: Max # of indels per locus (R1, R2)
0.5                            ## [24] [max_shared_Hs_locus]: Max # heterozygous sites per locus (R1, R2)
0, 0                           ## [25] [edit_cutsites]: Edit cut-sites (R1, R2) (see docs)
4, 4, 4, 4                     ## [26] [trim_overhang]: Trim overhang (see docs) (R1>, <R1, R2>, <R2)
* 							   ## [27] [output_formats]: Output formats (see docs)
                               ## [28] [pop_assign_file]: Path to population assignment file
```

###Steps 1-3: demultiplexing, filtering, clustering.

**Set parameters for each params file**:
1. project dir (probably leave this the default)
2. raw fastq path
3. barcodes path
5. assembly method: denovo
7. datatype: pairddrad
8. restriction overhang: CATG, AATT
15. max barcode mismatch: 1
16. filter adapters: 1

command: use a for loop to run each sublibrary as a separate job for steps 1-3

###Branch each sublibrary###:
```
ipyrad -p params-Lib_3.txt -b L3_90
ipyrad -p params-Lib_3.txt -b L3_95
ipyrad -p params-Lib_6.txt -b L6_90
ipyrad -p params-Lib_6.txt -b L6_95
ipyrad -p params-Lib_9.txt -b L9_90
ipyrad -p params-Lib_9.txt -b L9_95
ipyrad -p params-Lib_11.txt -b L11_90
ipyrad -p params-Lib_11.txt -b L11_95
ipyrad -p params-Lib_12.txt -b L12_90
ipyrad -p params-Lib_12.txt -b L12_95
ipyrad -p params-Lib_T.txt -b LT_90
ipyrad -p params-Lib_T.txt -b LT_95
ipyrad -p params-Lib_U.txt -b LU_90
ipyrad -p params-Lib_U.txt -b LU_95
ipyrad -p params-Lib_V.txt -b LV_90
ipyrad -p params-Lib_V.txt -b LV_95
ipyrad -p params-Lib_W.txt -b LW_90
ipyrad -p params-Lib_W.txt -b LW_95
ipyrad -p params-Lib_X.txt -b LX_90
ipyrad -p params-Lib_X.txt -b LX_95
ipyrad -p params-Lib_Y.txt -b LY_90
ipyrad -p params-Lib_Y.txt -b LY_95
ipyrad -p params-Lib_Z.txt -b LZ_90
ipyrad -p params-Lib_Z.txt -b LZ_95
```
###Edit branched params files: 0.90 & 0.95
`nano params*90.txt`
`nano params*95.txt`
14. clustering threshold 0.90, 0.95, respectively

###Merge all 12 0.85 libraries
ipyrad -m 12libs params-Lib_3.txt params-Lib_6.txt params-Lib_9.txt params-Lib_11.txt params-Lib_12.txt params-Lib_T.txt params-Lib_U.txt params-Lib_V.txt params-Lib_W.txt params-Lib_X.txt params-Lib_Y.txt params-Lib_Z.txt

###Branch phylo individuals
###Branch popgen individuals

###Run steps 4-7

###Rerun step 3 for the different clustering thresholds

merge all 12 0.90 libraries
merge all 12 0.95 libraries

branch subset of phylogenetic analysis individuals for each threshold: 0.85, 0.90, 0.95
branch subset of population level analysis individuals: 0.85, 0.90, 0.95

set params for each params file:
phylo85
phylo90
phylo95

pop85
pop90
pop95

Step 4: joint estimation of heterozygosity and error rate, Step 5: consensus base calling and filtering, Step 6: clustering/mapping across individuals, Step 7: filtering and formatting data output

run step 4-7 on phylo 0.85
run step 4-7 on phylo 0.90
run step 4-7 on phylo 0.95

run step 4-7 on pop 0.85
run step 4-7 on pop 0.90
run step 4-7 on pop 0.95

