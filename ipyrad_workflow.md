#ipyrad workflow: Oct 13 2016

##Data downloading and organization
###Copy the data onto hard drive:
The tarball is up on the cluster. Copy it to your current directory.
`cp ~/Mangomics/mangomics1.tar .`

###Unwrap the tarball
`tar -xvf mangomics1.tar`
This puts the files in a stupid directory called `Project_Emily` Move them to the current directory
`mv Project_Emily/Sample* .`

###Rename the files to correspond to the real library name
```
mvdir Sample_Sample_1 Lib_X
mvdir Sample_Sample_2 Lib_Z
mvdir Sample_Sample_3 Lib_3
mvdir Sample_Sample_2 Lib_W
mvdir Sample_Sample_5 Lib_Y
mvdir Sample_Sample_6 Lib_6
mvdir Sample_Sample_7 Lib_T
mvdir Sample_Sample_8 Lib_U
mvdir Sample_Sample_9 Lib_9
mvdir Sample_Sample_10 Lib_V
mvdir Sample_Sample_11 Lib_11
mvdir Sample_Sample_12 Lib_12
```

##Making params files
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
Lib_ExampleName	                           ## [0] [assembly_name]: Assembly name. Used to name output directories for assembly steps
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

##Steps 1-3: demultiplexing, filtering, clustering.

###Set parameters for each params file:
1. project dir (probably leave this the default)
2. raw fastq path
3. barcodes path
5. assembly method: denovo
7. datatype: pairddrad
8. restriction overhang: CATG, AATT
15. max barcode mismatch: 1
16. filter adapters: 1

###Run steps 1-3
**Use a for loop to run each sublibrary as a separate job for steps 1-3**
```
#!/bin/bash

export cores=32

for file in ./params-Lib_*.txt; do
bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n $cores -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p "$file" -s 123 -d -f -c $cores --MPI > "$file"_run.log 2>&1;
done
```

###Merge all 12 0.85 libraries
`ipyrad -m 12libs params-Lib_3.txt params-Lib_6.txt params-Lib_9.txt params-Lib_11.txt params-Lib_12.txt params-Lib_T.txt params-Lib_U.txt params-Lib_V.txt params-Lib_W.txt params-Lib_X.txt params-Lib_Y.txt params-Lib_Z.txt`

###Steps 4-7:
**Run step 4-7 on 0.85 using MPI**
```
export cores=32
export file=12libs

bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n "$cores" -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p params-12libs.txt -s 4567 -d -f -c "$cores" --MPI > "$file"_run.l$
```

#Branching to change parameters:
##Branch by different clustering threshholds:

##For 0.90 clustering
```
ipyrad -p params-Lib_3.txt -b L3_90
ipyrad -p params-Lib_6.txt -b L6_90
ipyrad -p params-Lib_9.txt -b L9_90
ipyrad -p params-Lib_11.txt -b L11_90
ipyrad -p params-Lib_12.txt -b L12_90
ipyrad -p params-Lib_T.txt -b LT_90
ipyrad -p params-Lib_U.txt -b LU_90
ipyrad -p params-Lib_V.txt -b LV_90
ipyrad -p params-Lib_W.txt -b LW_90
ipyrad -p params-Lib_X.txt -b LX_90
ipyrad -p params-Lib_Y.txt -b LY_90
ipyrad -p params-Lib_Z.txt -b LZ_90
```
Edit branched params files:
`nano params*90.txt`
14. clustering threshold 0.90

###Rerun step 3:
**Use a for loop to run each sublibrary as a separate job for step 3**
```
#!/bin/bash

export cores=32

for file in ./params-L*90.txt; do
bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n $cores -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p "$file" -s 123 -d -f -c $cores --MPI > "$file"_run.log 2>&1;
done
```

###Merge all 12 0.90 libraries:
`ipyrad -m 12libs_90 params-L3_90.txt params-L6_90.txt params-L9_90.txt params-L11_90.txt params-L12_90.txt params-LT_90.txt params-LU_90.txt params-LV_90.txt params-LW_90.txt params-LX_90.txt params-LY_90.txt params-LZ_90.txt`

###Steps 4-7:
**Run step 4-7 on all 0.90 using MPI**
```
export cores=32
export file=12libs_90

bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n "$cores" -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p params-12libs_90.txt -s 4567 -d -f -c "$cores" --MPI > "$file"_run.l$
```

##For 0.95 clustering
```
ipyrad -p params-Lib_3.txt -b L3_95
ipyrad -p params-Lib_6.txt -b L6_95
ipyrad -p params-Lib_9.txt -b L9_95
ipyrad -p params-Lib_11.txt -b L11_95
ipyrad -p params-Lib_12.txt -b L12_95
ipyrad -p params-Lib_T.txt -b LT_95
ipyrad -p params-Lib_U.txt -b LU_95
ipyrad -p params-Lib_V.txt -b LV_95
ipyrad -p params-Lib_W.txt -b LW_95
ipyrad -p params-Lib_X.txt -b LX_95
ipyrad -p params-Lib_Y.txt -b LY_95
ipyrad -p params-Lib_Z.txt -b LZ_95
```

###Edit branched params files:
`nano params*95.txt`
14. clustering threshold 0.95

###Rerun step 3:
**Use a for loop to run each sublibrary as a separate job for step 3**
```
#!/bin/bash

export cores=32

for file in ./params-L*95.txt; do
bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n $cores -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p "$file" -s 123 -d -f -c $cores --MPI > "$file"_run.log 2>&1;
done
```

###Merge all 12 0.95 libraries:
`ipyrad -m 12libs_95 params-L3_95.txt params-L6_95.txt params-L9_95.txt params-L11_95.txt params-L12_95.txt params-LT_95.txt params-LU_95.txt params-LV_95.txt params-LW_95.txt params-LX_95.txt params-LY_95.txt params-LZ_95.txt`

###Steps 4-7:
**Run step 4-7 on all 0.95 using MPI**
```
export cores=32
export file=12libs_95

bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n "$cores" -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p params-12libs_95.txt -s 4567 -d -f -c "$cores" --MPI > "$file"_run.l$
```

##Branch by individuals:

##Phylogenetic analysis
###Branch individuals for phylogenetic analysis
`ipyrad -p params-12libs.txt -b phylo85 3C 3E 6A 6C 6D 6F 6G 6H 9C 9F 9H 11C 11G 11H 12C 12D 12E 12F 12G 12H TA TB TC TD TE TF TG TH UA UB UC UD UE UF UG UH WA WF XA XB XC XD XE XF XG XH YB YC YG YH ZA ZB ZC ZD ZE ZF ZG ZH`

###Run steps 4-7 on phylo
```
export file=phylo85
export cores=32

bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n $cores -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p params-phylo85.txt -s 4567 -d -f -c $cores --MPI > "$file"_run.log 2>&1;
```
##Population genetic analysis
###Branch individuals for population genetic analysis
`ipyrad -p params-12libs.txt -b pop85 3A 3B 3C 3D 3E 3F 3G 3H 6A 6B 6C 6D 6E 6F 6G 9A 9B 9C 9D 9E 9F 9G 9H 11A 11B 11C 11D 11E 11F 11G 11H 12A 12B 12C 12F 12G 12H TB WA WB WC WD WE WF WG WH XE XF XH YA YB YC YD YE YF YG YH ZC ZF ZG`

###Run steps 4-7 on popgen
```
export file=pop85
export cores=32

bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n $cores -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p params-pop85.txt -s 4567 -d -f -c $cores --MPI > "$file"_run.log 2>&1;
```

##Branch each sublibrary for reference genome
For 0.85 clustering
```
ipyrad -p params-Lib_3.txt -b L3_ref_85
ipyrad -p params-Lib_6.txt -b L6_ref_85
ipyrad -p params-Lib_9.txt -b L9_ref_85
ipyrad -p params-Lib_11.txt -b L11_ref_85
ipyrad -p params-Lib_12.txt -b L12_ref_85
ipyrad -p params-Lib_T.txt -b LT_ref_85
ipyrad -p params-Lib_U.txt -b LU_ref_85
ipyrad -p params-Lib_V.txt -b LV_ref_85
ipyrad -p params-Lib_W.txt -b LW_ref_85
ipyrad -p params-Lib_X.txt -b LX_ref_85
ipyrad -p params-Lib_Y.txt -b LY_ref_85
ipyrad -p params-Lib_Z.txt -b LZ_ref_85
```
###Edit each params file with the appropriate clustering threshhold and alignment params
`nano params-L*_ref_85.txt`
5. Assembly method: reference
6. Reference sequence: filepath
14. Clustering threshhold: 0.85

For 0.90 clustering
```
ipyrad -p params-L3_ref_85 -b L3_ref_90
ipyrad -p params-L6_ref_85 -b L6_ref_90
ipyrad -p params-L9_ref_85 -b L9_ref_90
ipyrad -p params-L11_ref_85 -b L11_ref_90
ipyrad -p params-L12_ref_85 -b L12_ref_90
ipyrad -p params-LT_ref_85 -b LT_ref_90
ipyrad -p params-LU_ref_85 -b LU_ref_90
ipyrad -p params-LV_ref_85 -b LV_ref_90
ipyrad -p params-LW_ref_85 -b LW_ref_90
ipyrad -p params-LX_ref_85 -b LX_ref_90
ipyrad -p params-LY_ref_85 -b LY_ref_90
ipyrad -p params-LZ_ref_85 -b LZ_ref_90
```
###Edit each params file with the appropriate clustering threshhold
`nano params-L*_ref_90.txt`
14. Clustering threshhold: 0.90

For 0.95 clustering
```
ipyrad -p params-L3_ref_85 -b L3_ref_95
ipyrad -p params-L6_ref_85 -b L6_ref_95
ipyrad -p params-L9_ref_85 -b L9_ref_95
ipyrad -p params-L11_ref_85 -b L11_ref_95
ipyrad -p params-L12_ref_85 -b L12_ref_95
ipyrad -p params-LT_ref_85 -b LT_ref_95
ipyrad -p params-LU_ref_85 -b LU_ref_95
ipyrad -p params-LV_ref_85 -b LV_ref_95
ipyrad -p params-LW_ref_85 -b LW_ref_95
ipyrad -p params-LX_ref_85 -b LX_ref_95
ipyrad -p params-LY_ref_85 -b LY_ref_95
ipyrad -p params-LZ_ref_85 -b LZ_ref_95
```

###Edit each params file with the appropriate clustering threshhold
`nano params-L*_ref_95.txt`
5. Assembly method: reference
6. Reference sequence: filepath
14. Clustering threshhold: 0.95

##Rerun step 3 with reference for each library at each clustering threshhold
###For all \_ref\_ clustering (36 jobs total)
**Use a for loop to run each sublibrary as a separate job for step 3**
```
#!/bin/bash

export cores=32

for file in ./params-L*ref_*.txt; do
bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n $cores -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p "$file" -s 3 -d -f -c $cores --MPI > "$file"_run.log 2>&1;
done
```
###Merge the sublibraries for each clustering threshhold
For 0.85 clustering:
`ipyrad -m 12_ref_85 params-L3_ref_85.txt params-L6_ref_85.txt params-L9_ref_85.txt params-L11_ref_85.txt params-L12_ref_85.txt params-LT_ref_85.txt params-LU_ref_85.txt params-LV_ref_85.txt params-LW_ref_85.txt params-LX_ref_85.txt params-LY_ref_85.txt params-LZ_ref_85.txt`

For 0.90 clustering:
`ipyrad -m 12_ref_90 params-L3_ref_90.txt params-L6_ref_90.txt params-L9_ref_90.txt params-L11_ref_90.txt params-L12_ref_90.txt params-LT_ref_90.txt params-LU_ref_90.txt params-LV_ref_90.txt params-LW_ref_90.txt params-LX_ref_90.txt params-LY_ref_90.txt params-LZ_ref_90.txt`

For 0.95 clustering:
`ipyrad -m 12_ref_95 params-L3_ref_95.txt params-L6_ref_95.txt params-L9_ref_95.txt params-L11_ref_95.txt params-L12_ref_95.txt params-LT_ref_95.txt params-LU_ref_95.txt params-LV_ref_95.txt params-LW_ref_95.txt params-LX_ref_95.txt params-LY_ref_95.txt params-LZ_ref_95.txt`


##Step 4: joint estimation of heterozygosity and error rate, Step 5: consensus base calling and filtering, Step 6: clustering/mapping across individuals, Step 7: filtering and formatting data output

###Run step 4-7 on each sublibrary
**Run step 4-7 using MPI**
For 12_ref_0.85
```
export cores=32
export file=12_ref_85

bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n "$cores" -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p params-12libs_85.txt -s 4567 -d -f -c "$cores" --MPI > "$file"_run.l$
```

For 12_ref_0.90
```
export cores=32
export file=12_ref_90

bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n "$cores" -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p params-12libs_90.txt -s 4567 -d -f -c "$cores" --MPI > "$file"_run.l$
```

For 12_ref_0.95
```
export cores=32
export file=12_ref_95

bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n "$cores" -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p params-12libs_95.txt -s 4567 -d -f -c "$cores" --MPI > "$file"_run.l$
```
