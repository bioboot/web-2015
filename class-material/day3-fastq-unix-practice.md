---
layout: page
---

### I-1 Exploring FASTQ files in UNIX.

In this part, we will learn how to view FASTQ files from UNIX file system.

1. Login to the server, as you learned from [Day 1](../day1), using
  - Terminal (Mac OS X)
  - MobaXTerm (Windows)
  - or any other SSH client you prefer.
  
   <pre>
   $ ssh [your_id]@flux-login.engin.umich.edu
   (...Enter login credentials...)
   $ mkdir --p bioboot/day3
   $ cd bioboot/day3 </pre>
   
2. Create a symbolic link to the directory containing the input files, and add `PATH`

   <pre> $ ln -s /home/hmkang/bioboot/data/ data </pre>
  - `ln -s [target] [link_name]` creates a *shortcut* of the target file.
  - See `man ln` to see the detailed usage of `ln` 

   <pre> $ export PATH=/home/hmkang/bioboot/bin:${PATH} </pre>
  - Verify that the `data` and `bin` directories are correctly configured.
    <pre>
    $ ls data/
    ls data/
    ALL.chr20.phase3_shapeit2_mvncall_integrated_v5.20130502.genotypes.annotation.vcf.gz      gencode.v19.annotation.gtf.gz                            hs37d5.fa.fai
    ALL.chr20.phase3_shapeit2_mvncall_integrated_v5.20130502.genotypes.annotation.vcf.gz.tbi  HG00096.chrom20.ILLUMINA.bwa.GBR.exome.20120522.bam      ucsc.hg19.fasta
    bioboot_2015a_R1.fastq.gz                                                                 HG00096.chrom20.ILLUMINA.bwa.GBR.exome.20120522.bam.bai  ucsc.hg19.fasta.fai
    bioboot_2015a_R2.fastq.gz                                                                 hs37d5.fa
    
    $ which samtools
    /home/hmkang/bioboot/bin/samtools </pre>
    > (Note) You can scroll the box above to the right
  
3. Examine the contents of the FASTQ files

   <pre> $ zless data/bioboot_2015a_R2.fastq.gz </pre>
   - What do you see? Can you interpret what each line means?
   
   - Let's check whether the two FASTQ files are paired.
     <pre>
     $ zcat data/bioboot_2015a_R1.fastq.gz | wc -l
     $ zcat data/bioboot_2015a_R2.fastq.gz | wc -l </pre>
   - Do you think the files are paired? Why?
   - Let's check another way.
     <pre>
     $ zcat data/bioboot_2015a_R1.fastq.gz | head
     $ zcat data/bioboot_2015a_R2.fastq.gz | head </pre>
   - What additional information does it tell us?
   
4. Display only the sequence read information from FASTQ
   - What should we display? Which line numbers?
   
   <pre> $ zcat data/bioboot_2015a_R2.fastq.gz | awk 'NR % 4 == 2 {print;}' | less </pre>
 

   - `awk` is a script language useful for quick data manipulation.
   - The script above takes each line as input, and print the 2nd line of every four lines (line number NR is 4N+2).
   - We won't introduce `awk` today, but will use it in some occasions explaining specific details.
     - For more information about `awk`, [This link](http://www.linuxfocus.org/English/September1999/article103.html) could be a good starting point.
   - `sed` and `perl` one-liners are also useful tools for manipulating data easily
     - ...without having to write a separate program!

5. **(DIY)** What are the barcodes that appear most frequently?
   - **Given** : Your input file is `data/bioboot_2015a_R2.fastq.gz`
   - **Want** : Use UNIX utilities you learned plus `awk` to find out the sequences that most frequently occurs int the file.
   - Hint 1 : You need to use UNIX pipe multiple times, like the example below
     <pre> $ zcat data/bioboot_2015a_R2.fastq.gz | awk 'NR % 4 == 2 {print;}' | *** DO SOMETHING HERE *** | less </pre>
   - Hint 2 : `sort` utility sorts input lines alphabetically by default. Adding `-n` sorts input lines numerically. Adding `-r` sorts input in decreasing order. See `man sort` for detailed usage.
   - Hint 3 : `uniq` utility filter redundant lines appearing consecutively. `uniq -c` provides the number of repeats for each input line. See `man uniq` for detailed usage
   - Need help or done? [CLICK HERE FOR THE ANSWER](../class-material/day3-answers.html#what-are-the-barcodes-that-appear-most-frequently)

6. **(DIY)** Demultiplex a pair of FASTQ files.
   - **Given**
     - You have a pair of FASTQ files, `data/bioboot_2015a_R1.fastq.gz` and `data/bioboot_2015a_R2.fastq.gz`
     - The first file (51bp) contains actual sequence reads.
     - The second file (7bp) contains sample barcodes.
       <pre>
       Sample1 ACAGTGA
       Sample2 CAGATCA
       Sample3 GCCAATA
       Sample4 TGACCAA
       Sample5 TTAGGCA</pre>
     - Any pair of barcodes differ by 4 or more nucleotides.
   - **Want**
     - Write a python program that splits the first FASTQ files into six parts.
       - Five for each sample
       - and another one for 'UNKNOWN' samples.
     - Find out how many reads belong to each part.
   - Hint 1 : Use `gzip` library to read or write compressed files.
   
     <pre>
     import gzip
     fi = gzip.open(input_file_name, 'r')  // for reading
     fo = gzip.open(output_file_name, 'w') // for writing </pre>
   - Hint 2 : Simplest way is split based on exact match.
     - If the barcode does not match to any of the sample, consider as UNKNOWN.
     - This approach may lose may reads if barcodes have sequencing errors.
     - Need more hints? [CLICK HERE FOR A SKELETON CODE](../class-material/day3-answers.html#a-skeleton-code-for-fastq-demultiplexing)
     - Need a solution? [CLICK HERE FOR A SOLUTION](../class-material/day3-answers.html#b-answer-for-fastq-demultiplexing---exact-match)
   - Hint 3 : Allow up to one mismatch in the barcode
     - Because any pair of barcodes are different by 4 or more nucleotides, if there was one base call error in the barcode, it can be unambiguously resolved.
     - By modifying `outfs` from the exact match code, we can allow up to one mismatch (how?)
     - Need a solution? [CLICK HERE FOR A SOLUTION](../class-material/day3-answers.html#c-answer-for-fastq-demultiplexing---allowing-one-mismatch)
   - Bored? Try an advanced version : Use the base quality scores to probablistically calculate the best-matching barcode.
     - You can use `ord` function to convert characters to ASCII code integer.
     - Calculate the likelihood of barcode reads for each case to determine the best matching one.
     - What would be advantages and disadvantages for this approach?
 
Congratulations! Well done in exploring FASTQ files in UNIX!
<br>
Now let's go back to [Day 3 Overview](../day3).