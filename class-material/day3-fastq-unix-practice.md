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

   <pre> $ export PATH=${PATH}:/home/hmkang/bioboot/bin </pre>
  - Type `samtools` to see that the `$PATH` variable was properly updated.
  
3. Examine the contents of the FASTQ files

   <pre> $ zless data/bioboot_2015a_R2.fastq.gz </pre>
   
4. Show only the sequence read information from FASTQ
   <pre> $ zcat data/bioboot_2015a_R2.fastq.gz | awk 'NR % 4 == 2 {print;}' | less </pre>
 

   - `awk` is a script language useful for quick data manipulation.
   - The script above takes each line as input, and print the 2nd line of every four lines (line number NR is 4N+2).
   - We won't introduce any more specific details for `awk` today. For more information about `awk`, [This link](http://www.linuxfocus.org/English/September1999/article103.html) could be a good starting point.
   - `sed` and `perl` one-liners are also useful tools for manipulating data easily, without having to write a separate program.

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
   - Advanced version : Use the base quality scores to probablistically calculate the best-matching barcode.
     - You can use `ord` function to convert characters to ASCII code integer.
     - Calculate the likelihood of barcode reads for each case to determine the best matching one.
     - What would be advantages and disadvantages for this approach?
