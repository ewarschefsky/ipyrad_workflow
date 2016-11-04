
#ipyrad workflow: For reference genome

Rename the files to correspond to my library names
```
mvdir Sample_1 LX
mvdir Sample_2 LZ
mvdir Sample_3 L3
mvdir Sample_2 LW
mvdir Sample_5 LY
mvdir Sample_6 L6
mvdir Sample_7 LT
mvdir Sample_8 LU
mvdir Sample_9 L9
mvdir Sample_10 LV
mvdir Sample_11 L11
mvdir Sample_12 L12
```
Move barcodes files into the corresponding folder:
```
mv barcodes_3.txt L3/
mv barcodes_6.txt L6/
mv barcodes_9.txt L6/
mv barcodes_11.txt L11/
mv barcodes_12.txt L12/
mv barcodes_T.txt LT/
mv barcodes_U.txt LU/
mv barcodes_V.txt LV/
mv barcodes_W.txt LW/
mv barcodes_X.txt LX/
mv barcodes_Y.txt LY/
mv barcodes_Z.txt LZ/
```

##Making params files
Make a new blank/original params file for each sublibrary
```
ipyrad -n L3_ref_0.85
ipyrad -n L6_ref_0.85
ipyrad -n L9_ref_0.85
ipyrad -n L11_ref_0.85
ipyrad -n L12_ref_0.85
ipyrad -n LT_ref_0.85
ipyrad -n LU_ref_0.85
ipyrad -n LV_ref_0.85
ipyrad -n LW_ref_0.85
ipyrad -n LX_ref_0.85
ipyrad -n LY_ref_0.85
ipyrad -n LZ_ref_0.85
```

The params file will look like:
```
------- ipyrad params file (v.0.3.42)-------------------------------------------
	                           ## [0] [assembly_name]: Assembly name. Used to name output directories for assembly steps
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

###Change the following parameters for each params file (this is the annoying part)
2. raw fastq path (example - needs to be changed to correspond to each Library): `./L3/*.fastq.gz`
3. barcodes path (example - needs to be changed to correspond to each Library): `./L3/barcodes_3.txt`
5. assembly method: `reference`
6. reference sequence: ____fill in your filepath____
7. datatype: `pairddrad`
8. restriction overhang: `CATG, AATT`
15. max barcode mismatch: 1
16. filter adapters: 1

###Branch the analysis so it can be run at different clustering threshholds:
####For 0.90 clustering
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
**Edit each params file with the appropriate clustering threshhold**
`nano params-L*_ref_90.txt`
14. Clustering threshhold: 0.90

####For 0.95 clustering
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
**Edit each params file with the appropriate clustering threshhold**
`nano params-L*_ref_95.txt`
14. Clustering threshhold: 0.95

##Run steps 1-3 with reference for each library at each clustering threshhold
###For all \_ref\_ clustering (36 jobs total)
**Use a for loop to run each sublibrary as a separate job for step 3**
```
#!/bin/bash

export cores=32

for file in ./params-L*ref_*.txt; do
bsub -J "$file" -eo "$file".err -oo "$file".out -q PQ_wettberg -n $cores -R "span[ptile=16]" -m "IB_16C_96G" time ipyrad -p "$file" -s 123 -d -f -c $cores --MPI > "$file"_run.log 2>&1;
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
