---
layout: page
---

### III-4. Working with BED files.

In this part, we will learn how to use BED format files to facilitate region-based interpretation of genomic data.

1. The BED format specification is available [HERE](https://genome.ucsc.edu/FAQ/FAQformat.html#format1). While there are many optional columns, here we will focus only on four key fields.
   1. Chromosome name
   2. Chromsome start position (inclusive if 0-based, exclusive if 1-based)
   3. Chromosome end position  (exclusive if 0-based, inclusive if 1-based)
   4. (Optional) Name of the region.

2. **(DIY)** Convert the refFlat.txt.gz file to BED format, using the transcription start and end positions and boundary, and using canonical gene name as the name of the region.
  - Try using `cut` command to extract the columns you want.
  
    <pre>
    $ zcat refFlat.txt.gz | cut -f 3,5,6,1 | less </pre>
    - Does it conform to BED file format we want? What is the problem?
  - Use `awk` to allow reordering of columns
  
    <pre>
    $ zcat refFlat.txt.gz | awk 'BEGIN {OFS="\t"}; {print($3,$5,$6,$1)}' | less </pre>
    
    - Does it look better now? Then store the output into a BED file
    <pre>
    $ zcat refFlat.txt.gz | awk 'BEGIN {OFS="\t"}; {print($3,$5,$6,$1)}' > refFlat.bed </pre>    

3. **(DIY)** Write a `python` program that print out lines that overlaps with a region
   - **Given**
     - Use refFlat.bed produced above as your input file
     - Take three input arguments, (1) chromosome, (2) start, and (3) end position.
   - **Want**
     - Print the lines overlapping with the specified region.
     - Overlaps can be partial. If any part of the interval overlaps, it is considered overlapping.
     - Example run is shown below
    
       <pre>
       $ python bed-overlap.py chr20 10000000 10100000
       chr20 	 10004459 	 10200154 	 SNAP25-AS1
       chr20 	 10015646 	 10037410 	 ANKEF1
       chr20 	 10015646 	 10037410 	 ANKEF1
       chr20 	 10015646 	 10037410 	 ANKEF1 </pre>
     - (SPOILER ALERT!) Here is an example skeleton code
       <pre>  
       import sys
       
       script, chrom, beg, end = sys.argv  # get arguments from command line
       f = open('refFlat.bed','r')         # read from the input file
       for line in f:			   # iterate over the file by line
         tok = line.split()		   # tokenize the line by whitespaces (list of size 4)
	 ## HERE YOU NEED TO PRINT THE LINE ONLY WHEN IT OVERLAPS
       f.close() </pre>
     - To check example solutions, [CLICK HERE](../class-material/day3-answers.html#write-a-python-program-that-print-out-lines-that-overlaps-with-a-region)
     - Do you think this is an efficient solution to get the answer?

4. **(DIY)** Alternative way without writing a program is to utilize `tabix`.
   - First, we need to sort the BED file by chromosome and coordinates, and compress it with `bgzip`.
     <pre>
     $ sort -k 1,1 -k 2,2n -k 3,3n refFlat.bed | bgzip -c > refFlat.sorted.bed.gz </pre>
   - Next, we can index the sorted and bgzipped BED file using `tabix`.
     <pre>
     $ tabix -pbed refFlat.sorted.bed.gz </pre>
   - Then you will be able to perform random access of the BED file by region in an efficient way.
     <pre>
     $ tabix refFlat.sorted.bed.gz chr20:10000000-10100000
     chr20	10004459	10200154	SNAP25-AS1
     chr20	10015646	10037410	ANKEF1
     chr20	10015646	10037410	ANKEF1
     chr20	10015646	10037410	ANKEF1 </pre>
   - If you need to perform this query repeatedly, there will be huge difference in performance in this way, compared to the unsorted version in the way above.

Congratulations! Well done in understanding BED formats.
<br>
Now let's go back to [Day 3 Overview](../day3).