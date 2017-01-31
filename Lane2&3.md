#Notes:

##download the datasets in the background without hangups
Lane2.tar
Lane3.tar
Lane_redo.tar

#Lane_redo
##unwrap Lane_redo tarball:
tar vxf Lane_redo/Lane_redo.tar
writes files:
Project_Library_2/
Project_Library_3/
##move to different folder:
mv Project_Library_2 Lane2_redo
mv Project_Library_3 Lane3_redo

#Lane2
##unwrap Lane2 tarball
tar vxf Lane2/Lane2.tar
##move to different folder:
mv Project_Library_2 Lane2

#Lane3
##unwrap Lane3 tarball
tar vxf Lane3/Lane3.tar
##move to different folder:
mv Project_Library_3 Lane3

#Combine 'redo' with Lane2
cd Lane2_redo/
mv ./*/*.fastq* .
for f in *.fastq.gz; do mv "$f" "$(basename $f .fastq.gz).redo"; done
for f in *.redo; do mv "$f" "$f.fastq.gz"; done
ls
cd..
mv Lane2_redo/*fastq.gz* Lane2/
cd Lane2

mv Sample_10_TAGCTT_L001_R1_001.redo.fastq.gz Sample_Sample_10
mv Sample_11_TAATCG_L001_R1_001.redo.fastq.gz Sample_Sample_11
mv Sample_12_TACAGC_L001_R1_001.redo.fastq.gz Sample_Sample_12
mv Sample_1_ATCACG_L001_R1_001.redo.fastq.gz Sample_Sample_1
mv Sample_2_TTAGGC_L001_R1_001.redo.fastq.gz Sample_Sample_2
mv Sample_3_CAGATC_L001_R1_001.redo.fastq.gz Sample_Sample_3
mv Sample_4_CTTGTA_L001_R1_001.redo.fastq.gz Sample_Sample_4
mv Sample_5_AGTCAA_L001_R1_001.redo.fastq.gz Sample_Sample_5
mv Sample_6_CCGTCC_L001_R1_001.redo.fastq.gz Sample_Sample_6
mv Sample_7_GTTTCG_L001_R1_001.redo.fastq.gz Sample_Sample_7
mv Sample_8_GGTAGC_L001_R1_001.redo.fastq.gz Sample_Sample_8
mv Sample_9_CAACTA_L001_R1_001.redo.fastq.gz Sample_Sample_9

for f in Sample1; cd $f; cat *001.redo.fastq.gz *001.fastq.gz > ; done

for dir in Sample*; 
  do echo "$dir"; 
    for file in "$dir"/*001.fastq.gz;
      do cat "$file" "$dir"/*redo.fastq.gz > "$dir/$(basename $file .fastq.gz).comb.fastq.gz";
    done;
done
   
   
#Combine 'redo' with Lane3
cd Lane3_redo/
mv ./*/*.fastq* .
for f in *.fastq.gz; do mv "$f" "$(basename $f .fastq.gz).redo"; done
for f in *.redo; do mv "$f" "$f.fastq.gz"; done
ls
cd ..
mv Lane3_redo/*fastq.gz* Lane3/
cd Lane3

mv Sample_10_TAGCTT_L002_R1_001.redo.fastq.gz Sample_Sample_10
mv Sample_11_TAATCG_L002_R1_001.redo.fastq.gz Sample_Sample_11
mv Sample_12_TACAGC_L002_R1_001.redo.fastq.gz Sample_Sample_12
mv Sample_1_ATCACG_L002_R1_001.redo.fastq.gz Sample_Sample_1
mv Sample_2_TTAGGC_L002_R1_001.redo.fastq.gz Sample_Sample_2
mv Sample_3_CAGATC_L002_R1_001.redo.fastq.gz Sample_Sample_3
mv Sample_4_CTTGTA_L002_R1_001.redo.fastq.gz Sample_Sample_4
mv Sample_5_AGTCAA_L002_R1_001.redo.fastq.gz Sample_Sample_5
mv Sample_6_CCGTCC_L002_R1_001.redo.fastq.gz Sample_Sample_6
mv Sample_7_GTTTCG_L002_R1_001.redo.fastq.gz Sample_Sample_7
mv Sample_8_GGTAGC_L002_R1_001.redo.fastq.gz Sample_Sample_8
mv Sample_9_CAACTA_L002_R1_001.redo.fastq.gz Sample_Sample_9

for dir in Sample*; 
  do echo "$dir"; 
    for file in "$dir"/*001.fastq.gz;
      do cat "$file" "$dir"/*redo.fastq.gz > "$dir/$(basename $file .fastq.gz).comb.fastq.gz";
    done;
done
   
#Lane 2:
mv Sample_Sample_1 LibA
mv Sample_Sample_2 LibB
mv Sample_Sample_3 LibC
mv Sample_Sample_4 LibD
mv Sample_Sample_5 LibE
mv Sample_Sample_6 LibF
mv Sample_Sample_7 LibG
mv Sample_Sample_8 LibH
mv Sample_Sample_9 LibI
mv Sample_Sample_10 LibJ
mv Sample_Sample_11 LibK
mv Sample_Sample_12 LibL

## get barcode files into system (run from your computer, not the HPC):
scp /Users/emily/Dropbox/Mangomics/Lib2_barcode_files/* ewars001@hpclogin01.fiu.edu:/home/ewars001/Mangomics/

###Move them to the right folder:
cp ~/Mangomics/Lane2_barcodes/LibA LibA/LibA_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibB LibB/LibB_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibC LibC/LibC_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibD LibD/LibD_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibE LibE/LibE_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibF LibF/LibF_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibG LibG/LibG_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibH LibH/LibH_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibI LibI/LibI_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibJ LibJ/LibJ_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibK LibK/LibK_barcodes.txt
cp ~/Mangomics/Lane2_barcodes/LibL LibL/LibL_barcodes.txt

# Make a params file:
ipyrad -n L3

```
------- ipyrad params file (v.0.5.15)-------------------------------------------
LibA                           ## [0] [assembly_name]: Assembly name. Used to name output directories for assembly steps
./                             ## [1] [project_dir]: Project dir (made in curdir if not present)
LibA/*comb.fastq.gz                               ## [2] [raw_fastq_path]: Location of raw non-demultiplexed fastq files
LibA/LibA                               ## [3] [barcodes_path]: Location of barcodes file
                               ## [4] [sorted_fastq_path]: Location of demultiplexed/sorted fastq files
denovo                         ## [5] [assembly_method]: Assembly method (denovo, reference, denovo+reference, denovo-reference)
                               ## [6] [reference_sequence]: Location of reference sequence file
ddrad                            ## [7] [datatype]: Datatype (see docs): rad, gbs, ddrad, etc.
CATG, AATT                         ## [8] [restriction_overhang]: Restriction overhang (cut1,) or (cut1, cut2)
5                              ## [9] [max_low_qual_bases]: Max low quality base calls (Q<20) in a read
33                             ## [10] [phred_Qscore_offset]: phred Q score offset (33 is default and very standard)
6                              ## [11] [mindepth_statistical]: Min depth for statistical base calling
6                              ## [12] [mindepth_majrule]: Min depth for majority-rule base calling
1000                          ## [13] [maxdepth]: Max cluster depth within samples
0.95                           ## [14] [clust_threshold]: Clustering threshold for de novo assembly
1                              ## [15] [max_barcode_mismatch]: Max number of allowable mismatches in barcodes
1                              ## [16] [filter_adapters]: Filter for adapters/primers (1 or 2=stricter)
35                             ## [17] [filter_min_trim_len]: Min length of reads after adapter trim
2                              ## [18] [max_alleles_consens]: Max alleles per site in consensus sequences
5, 5                           ## [19] [max_Ns_consens]: Max N's (uncalled bases) in consensus (R1, R2)
8, 8                           ## [20] [max_Hs_consens]: Max Hs (heterozygotes) in consensus (R1, R2)
9                              ## [21] [min_samples_locus]: Min # samples per locus for output
20, 20                         ## [22] [max_SNPs_locus]: Max # SNPs per locus (R1, R2)
8, 8                           ## [23] [max_Indels_locus]: Max # of indels per locus (R1, R2)
0.5                            ## [24] [max_shared_Hs_locus]: Max # heterozygous sites per locus (R1, R2)
0, 0                           ## [25] [edit_cutsites]: Edit cut-sites (R1, R2) (see docs)
0, 0, 0, 0                     ## [26] [trim_overhang]: Trim overhang (see docs) (R1>, <R1, R2>, <R2)
l, p, s, v                     ## [27] [output_formats]: Output formats (see docs)
                               ## [28] [pop_assign_file]: Path to population assignment file
```

ipyrad -n LibB
ipyrad -n LibC
ipyrad -n LibD
ipyrad -n LibE
ipyrad -n LibF
ipyrad -n LibG
ipyrad -n LibH
ipyrad -n LibI
ipyrad -n LibJ
ipyrad -n LibK
ipyrad -n LibL


mv LibA/LibA LibA/LibA_barcodes.txt
mv LibB/LibB LibB/LibB_barcodes.txt
mv LibC/LibC LibC/LibC_barcodes.txt
mv LibD/LibD LibD/LibD_barcodes.txt
mv LibE/LibE LibE/LibE_barcodes.txt
mv LibF/LibF LibF/LibF_barcodes.txt
mv LibG/LibG LibG/LibG_barcodes.txt
mv LibH/LibH LibH/LibH_barcodes.txt
mv LibI/LibI LibI/LibI_barcodes.txt
mv LibJ/LibJ LibJ/LibJ_barcodes.txt
mv LibK/LibK LibK/LibK_barcodes.txt
mv LibL/LibL LibL/LibL_barcodes.txt


run s1

merge:
ipyrad -m Lane2 /scratch/ewars001/ipyrad/Lane2/LibA/params-LibA.txt /scratch/ewars001/ipyrad/Lane2/LibB/params-LibB.txt /scratch/ewars001/ipyrad/Lane2/LibC/params-LibC.txt /scratch/ewars001/ipyrad/Lane2/LibD/params-LibD.txt /scratch/ewars001/ipyrad/Lane2/LibE/params-LibE.txt /scratch/ewars001/ipyrad/Lane2/LibF/params-LibF.txt /scratch/ewars001/ipyrad/Lane2/LibG/params-LibG.txt /scratch/ewars001/ipyrad/Lane2/LibH/params-LibH.txt /scratch/ewars001/ipyrad/Lane2/LibI/params-LibI.txt /scratch/ewars001/ipyrad/Lane2/LibJ/params-LibJ.txt /scratch/ewars001/ipyrad/Lane2/LibK/params-LibK.txt /scratch/ewars001/ipyrad/Lane2/LibL/params-LibL.txt

