# **A Kmer-based paired-end read (KPR) *de novo* assembler and genotyper to genotype major histocompatibility complex class I (MHC-I) alleles for the dog** 

Kmer-based paired-end read (KPR) de novo assembler and genotyper take paired-end RNA-seq reads of an individual as input and output the MHC-I alleles of the individual.  The tools currently only work for the dog, consisting the following steps: 1) mapping paired-RNA-seq reads to the canine MHC-I (i.e., DLA-I) reference alleles; 2)  assemble paired-end RNA-seq reads mapped to the DLA-I regions into contigs de novo; and 3) genotyping the assembled contig.  

The software is described in detail at https://www.biorxiv.org/content/10.1101/2020.07.15.205559v2.   

### **Requirements**
The following package/software are required to run the KPR de novo assembler and genotyper  
  Python 2.7.14  
  SAMtools 1.9  
  Trimmomatic 0.36  
  BWA 0.7.17  
  Clustal-Omega 1.2.4  


### **Usage**
The package contains an Unix/Linux shell script (KPR_canine_MHC-I_assemblerNgenotyper.sh), a Python script directory and a DLA-I allele reference directory.  

##### Steps to run the package  
1. Downloading the package (Linux/Unix platform is required)  
2. Modify the shell script by specifying the actual path to each of the Python script directory, the input RNA-seq fastq file directory, the result directory, and to the reference directory, as well as the names of both the forward and reverse RNA-seq fastq files, as below.

script=''     # path to the KPR scripts  
data=''       # path to the RNA-seq paired-end fastq files  
result=''     # path to the output directory  
ref=''        # path to the KPR reference  

fastq1=''     # RNA-seq fastq file 1 (forward)  
fastq2=''     # RNA-seq fastq file 2 (reverse)  

3.  Adjust two running paramenters: 1) the K-mer length, K; and 2) the number of assembly runs, N, based on the input RNA-seq data. Normally, we will suggest K=half of the RNA-seq read length and N>=100. Larger N will take more time, but will yield more comprehensive genotyping.  
  
K=50          # K-mer length  . 
N=30000       # The number of assembly runs  
  
4.  Required package/software loading:  the shell script contain lines for loading required package/software that are unique to the UGA (Sapelo2) platform.  If your platform uses a different approach of loading package/software, you will need to make corresponding changes in the script for lines below.  
  
module load SAMtools/1.9-foss-2016b  
module load Trimmomatic/0.36-Java-1.8.0_144  
module load BWA/0.7.17-foss-2016b  
module load SRA-Toolkit/2.9.1-centos_linux64  
module load Clustal-Omega/1.2.4-foss-2016b  
  
5.  Once the shell script is finishing running, the result file, Merged_genotyping_result_withGeneAssignment.txt, will appear in the $result directory, containing the following columns: 
 
[1] Sample  
[2] Contig name  
[3] Allele frequency   
[4] Allele name (if match a known allele)  
[5] Allele group name (if match a known allele group based on hypervariable region sequence)  
[6] Gene name  
[7] Exons 2 and 3 protein sequence  
[8] Hypervariable region sequence  
  
An example output line is provided below:  

Dog1 NormAlign_12 0.271979759646 DLA-88*006:01 DLA-88*006 DLA-88 GSHSLRYFYTSVSRPGRGDPRFIAVGYVDDTQFVRFDSDAATGRTEPRAPWVEQEGPEYWDPQTRTIKETAQLYRVDLDTLRGYYNQSEAGSHTRQTMYGCDLGPGGRLLRGYSQDAYDGADYIALNEDLRSYTAADTAAQITRRKWEAAGTAEHDRNYLETTCVEWLRRYLEMGKETLQRA PQTRTIKETAQLYRVDGSHTRQTMYGCDLGPGGRLLRGYSQDTAEHDR  
