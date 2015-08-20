---
layout: page
title: Day 3
permalink: /day3/
---

# Day 3. Data Formats and Visualization

A large fraction of biological science in these days involves
analysis of genomic data.

There are many *de facto* standards for
representing genomic data, including but not limited to,
FASTA, FASTQ, SAM/BAM, VCF, GTF, and BED formats.
The ability to read and manipulate data files stored in these formats
is one of the key skill sets needed for genome scientists.

Effective visualization of the genomic data and its analysis outcome is a
key component for successful communication in the research community
handling a large amount of genomic data.

Today, we will learn the widely used genomic data formats and
practice how to read and manipulate the data in these formats. We will
also learn the basics of data visualization with practice on real data.

<br>

### Schedule:

| Session | Time           | Topics                   | 
| :-----: |:--------------:| :----------------------- | 
| I       | 9:00-10:15 AM  | **Mini-Practice : FASTQ File Manipulation** | 
|         | 10:15-10:30AM  | Coffee Break             | 
| II      | 10:30-11:15 AM | **Lecture : Data Formats and Conversions**       | 
| III     | 11:15-12:00 AM | **Mini-Practice: Select a subset of variant/genotype calls**       | 
|         | 12:00-1:00PM   | Lunch                    | 
| IV      | 1:00-2:15 PM   | **Practice : Analysis with Genomic Data Formats** | 
|         | 2:15-2:30 PM   | Coffee Break             | 
| V       | 2:30-4:00 PM   | **Visualization: Overview and Practice**   | 


<br>

### Instructors:
Hyun Min Kang (HMK)  
Jacob Kitzman (JK)

<br>

### Topics:

#### I) Mini-Practice : FASTQ File Manipulation [1:15 hr]  HMK
- [Introductory Slides - Getting Started with FASTQ Files](../class-material/2015_08_day3_sec01_v1.pdf)
- Set your input file directory

  ```
  export DATA=/home/hmkang/bioboot/data/
  ```
- Examine the contents of the FASTQ files

  ```
  zless $DATA/bioboot_2015a_R2.fastq.gz
  ```
- Show only the sequence read information from FASTQ

  ```
  zcat $DATA/bioboot_2015a_R2.fastq.gz | awk 'NR % 4 == 2 {print;}' | less
  ```
- What are the barcodes that appears most frequently?
- Write a python code that split the input read 1 FASTQ, based on the expected barcodes at read 2
 - How many reads do not belong to any of the samples?
- If we allow one mismatch in the barcode sequence, how does it change?

#### II) Lecture : Overview of Genomic Data Formats and Conversions [0:45 hr]  JK
- [Slides - Data Formats and Conversions](../class-material/2015_08_day3_sec02.pdf)
 
#### III) Mini-Practice: Select a subset of variant/genotype calls [0:45 hr] JK
 - download a VCF file
 - write a simple python script to loop through the VCF file and select specific rows
 - index it the VCF file and use tabix to select the same rows  
 - overlap a VCF and a bed file using bedtools

#### IV) Practice : Analysis with Genomic Data Formats [1:15 hr]  HMK
- Understanding and accessing FASTA files
- SAM/BAM - Aligned sequence reads
- VCF - Variant Call Formats
- Formats for representing genes
- BED - Formats for genomic intervals
- Working with genome browsers: IGV and UCSC 

#### IV) Visualization: Overview and Practice [1:30 hr] JK
- [Slides - Data Visualization](../class-material/2015_08_day3_sec04.pdf)
- Data visualization for exploratory analysis
- Commonly used types of graphs
- Basic graphing in ipython with matplotlib
- Introduction to ggplot graphing
- Analysis and visualization of genotypes and gene expression


<br>

### Reference material
[Referene Commands and Glossary](../class-material/vis-reference.html)  
[Slides]()
