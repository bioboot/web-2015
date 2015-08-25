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
    $ zcat refFlat.txt.gz | awk '{print($3,"\t",$5,"\t",$6,"\t",$1)}' | less </pre>
    - Does it look better now? Then store the output into a BED file
    <pre>
    $ zcat refFlat.txt.gz | awk '{print($3,"\t",$5,"\t",$6,"\t",$1)}' > refFlat.bed </pre>

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
     - To check example solutions, [CLICK HERE](../class-material/day3-answers.html#write-a-python-program-that-print-out-lines-that-overlaps-with-a-region) 