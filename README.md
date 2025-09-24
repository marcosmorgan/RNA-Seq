Master RNA-seq analysis from start to finish — all in one video!
In this step-by-step tutorial, I walk you through every stage of an RNA-seq pipeline, from downloading FASTQ files to generating a table of differentially expressed genes. You’ll learn how to install the required software, run each command, and understand what each step is doing — all while looking over my shoulder.

Whether you’re new to RNA-seq or looking for a reproducible workflow, this video will give you the tools and confidence to run your own analysis.

🔗 Bonus: If you want to automate the whole process, check out my Snakemake playlist — linked in the video!

Links and code

#Link to a fastq file: https://www.ebi.ac.uk/ena/browser/view/ERR188297

#Alternatively, from the command line using MacOS: 
wget -nc ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR188/ERR188297/ERR188297_1.fastq.gz

#Reference transcriptome and annotations: 
https://www.gencodegenes.org/human/

#Code to pull the Docker images:
docker pull biocontainers/minimap2:v2.15dfsg-1-deb_cv1
docker pull biocontainers/samtools:v1.9-4-deb_cv1
docker pull hadrieng/salmon:0.13.1

#Uploading R libraries
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("tximport")

if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("DESeq2")

#Running the pipeline: 

alias minimap2="docker run --rm --platform linux/amd64 --user $(id -u):59560 -v $(pwd):$(pwd) -w $(pwd) biocontainers/minimap2:v2.15dfsg-1-deb_cv1 minimap2 "

minimap2 -ax sr gencode.v48.transcripts.fa ERR188021_1.fastq.gz x ERR188021.sam
minimap2 -ax sr gencode.v48.transcripts.fa ERR188088_1.fastq.gz x ERR188088.sam
minimap2 -ax sr gencode.v48.transcripts.fa ERR188288_1.fastq.gz x ERR188288.sam
minimap2 -ax sr gencode.v48.transcripts.fa ERR188297_1.fastq.gz x ERR188297.sam
minimap2 -ax sr gencode.v48.transcripts.fa ERR188329_1.fastq.gz x ERR188329.sam
minimap2 -ax sr gencode.v48.transcripts.fa ERR188356_1.fastq.gz x ERR188356.sam

alias samtools="docker run --rm --platform linux/amd64 --user $(id -u):59560 -v $(pwd):$(pwd) -w $(pwd) biocontainers/samtools:v1.9-4-deb_cv1 samtools "

samtools view -S -b ERR188021.sam x ERR188021.bam
samtools view -S -b ERR188088.sam x ERR188088.bam
samtools view -S -b ERR188288.sam x ERR188288.bam
samtools view -S -b ERR188297.sam x ERR188297.bam
samtools view -S -b ERR188329.sam x ERR188329.bam
samtools view -S -b ERR188356.sam x ERR188356.bam

alias salmon="docker run --rm --platform linux/amd64 --user $(id -u):59560 -v $(pwd):$(pwd) -w $(pwd) hadrieng/salmon:0.13.1 salmon "

salmon quant -t gencode.v48.pc_translations.fa -l IU -a ERR188297.bam -o ERR188297
salmon quant -t gencode.v48.pc_translations.fa -l IU -a ERR188088.bam -o ERR188088
salmon quant -t gencode.v48.pc_translations.fa -l IU -a ERR188329.bam -o ERR188329
salmon quant -t gencode.v48.pc_translations.fa -l IU -a ERR188288.bam -o ERR188288
salmon quant -t gencode.v48.pc_translations.fa -l IU -a ERR188021.bam -o ERR188021
salmon quant -t gencode.v48.pc_translations.fa -l IU -a ERR188356.bam -o ERR188356

R code: 

⏱ Chapters:
00:00 – Introduction & Overview
00:40 – RNA-seq Pipeline Outline
02:33 – Downloading FASTQ Files & Reference Transcriptome
03:00 – Setting Up Docker & Pulling Images
04:53 – Mapping Reads with Minimap2
05:54 – Converting SAM to BAM with Samtools
06:18 – Quantifying Reads with Salmon
06:55 – Loading Results into RStudio
07:12 – Converting Transcripts to Genes (tximport)
07:32 – Running DESeq2 Differential Expression Analysis
07:45 – Next Steps: Gene Ontology Analysis
