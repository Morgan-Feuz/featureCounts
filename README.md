# featureCounts Set-Up and Workflow

### Background 
[featureCounts](https://subread.sourceforge.net/featureCounts.html) is a general-purpose read summarization program that counts mapped reads for genomic features (e.g., genes, exons, promoters, gene bodies, genomic bins, chromosomal locations). It is frequently used to count both RNA-seq and genomic DNA-seq reads and is part of the complete [Subread package](https://subread.sourceforge.net/). 

----------------------------------------------------------------------------------------------------

### Linux Installation
First, download the latest [Subread version](https://sourceforge.net/projects/subread/files/) from SourceForge for Linux, move the file to an appropriate location, and unzip the file. 
```
# unzip the Subread file
$ tar zxvf subread-2.0.6-Linux-x86_64.tar.gz

# add to the bin to make executable from any location
$ sudo ln -s /path/featureCounts/subread-2.0.6-Linux-x86_64/bin/featureCounts /usr/bin/

# check symbolic link
$ featureCounts
```

-------------------------------------------------------------------------------------------------------
### Usage
`featureCounts` takes as input SAM/BAM files and a genome annotation file including chromosomal coordinates of features. It outputs numbers of reads assigned to features (or meta-features). It also outputs stat info for the overall summrization results, including number of successfully assigned reads and number of reads that failed to be assigned due to various reasons (these reasons are included in the stat info). The annotation file should be in either [GTF format](https://genome.ucsc.edu/FAQ/FAQformat.html#format4) or a simplified annotation format (SAF). 

[Here](https://manpages.org/featurecounts) is a manual page listing featureCounts arguments. 

+ <ins>Using alternative genome annotations:</ins><br>
In the below example, the GTF file (that should match the FASTA file used for the RNA-seq mapping step) was acquired from [GENCODE](https://www.gencodegenes.org/mouse/). See the guide for checking RNA strandness [here](https://github.com/Morgan-Feuz/RSeQC). 
```
# example (single-end, one file)
$ featureCounts -s 2 -a /path/gencode.vM35.basic.annotation.gtf -o /path/file_featureCounts.txt /path/file.bam

# example  (paired-end, one file)
$ featureCounts -s 2 -p --countReadPairs -a /path/gencode.vM35.basic.annotation.gtf -o /path/file_featureCounts.txt /path/file.bam

```
<br>

+ <ins>Using featureCounts built-in annotations:</ins> <br>
In-built gene annotations for genomes hg38, hg19, mm39, mm10 and mm9 are included in both Bioconductor Rsubread package and SourceForge Subread package. These annotations were downloaded from NCBI RefSeq database and then adapted by merging overlapping exons from the same gene to form a set of disjoint exons for each gene. Genes with the same Entrez gene identifiers were also merged into one gene. Each row in the annotation represents an exon of a gene. There are five columns in the annotation data including Entrez gene identifier (GeneID), chromosomal name (Chr ), chromosomal start position(Start), chromosomal end position (End) and strand (Strand).
```
# example using built-in annotation
$ featureCounts -s 0 -p -F SAF -a ../annotation/mm10_RefSeq_exon.txt -o /path/file_featureCounts.txt /path/file.bam 
```

-------------------------------------------------------------------------------------------------
### Output
`featureCounts` produces two new files: a summary file and a counts file. 

<ins>Create a counts matrix using Linux:</ins>
```
# view the cts file
$ cat featurecounts.txt | less

# view the first and 7th columns (gene_ids and cts)
$ cat featureCounts.txt| cut -f1,7 | less 

# note: type `q` to escape less reading mode

# create new cts file using Linux
cat featureCounts.txt| cut -f1,7 > output.txt
```









