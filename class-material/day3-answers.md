---
layout: page
---

## Day 3 Practice - Solutions for Practice Examples

### I-1) Exploring FASTQ files in UNIX.

#### 5. What are the barcodes that appear most frequently? 
  - Answer : For the full command, see below (scroll to right to see all)
  
    <pre>zcat data/bioboot_2015a_R2.fastq.gz | awk 'NR % 4 == 2 {print;}' | sort | uniq -c | sort -n -r | less</pre>

  - Expected Output : First several lines are shown below.
     <pre>
     20442 TGACCAA
     20095 CAGATCA
     17475 TTAGGCA
     16342 ACAGTGA
        91 ACAGTGC
        74 GCCCATA
        72 ACCGTGA
        69 TTAGCAT
        69 CAGATCC
     ...</pre>

#### 6a. Skeleton Code for FASTQ Demultiplexing
  - Below is the skeleton python code you can start with
    <pre>
    import gzip

    bc2id = { ... }  # dictory to map barcodes to sample IDs
    outfs = {}       # dictory to map barcodes to output file handles
    for bc in bc2id:
    	outfs[bc] = gzip.open( ... )  # open output file for each barcode
    unknown = gzip.open( ... )        # open output file for unknown file

    f1 = gzip.open( ... )  # open FASTQ file for the first read
    f2 = gzip.open( ... )  # open FASTQ file for the second read

    for line in f2:
       # fill in the code below in the following way
       # 1. Read barcode information from second read
       # 2. determine the file handle to write to based on the barcode
       # 3. Read from the first FASTQ and write to the file handle

    # close output handle if needed </pre>

#### 6b. Answer for FASTQ Demultiplexing - Exact Match
  - Below is the example solution based on exact match
    <pre>
    import gzip

    bc2id = {'ACAGTGA' : 'Sample1', 'CAGATCA' : 'Sample2' , 'GCCAATA' : 'Sample3', 'TGACCAA' : 'Sample4' ,'TTAGGCA' : 'Sample5'}
    outfs = {}
    for bc in bc2id:
      f = gzip.open('out/bioboot_2015a_'+bc2id[bc]+'.fastq.gz','w')
      outfs[bc] = f
    outunknown = gzip.open('out/bioboot_2015a_UNKNOWN.fastq.gz','w')

    f1 = gzip.open('data/bioboot_2015a_R1.fastq.gz','r')
    f2 = gzip.open('data/bioboot_2015a_R2.fastq.gz','r')

    for rname2 in f2:
      seq2 = f2.readline().rstrip()
      dummy2 = f2.readline()
      qual2 = f2.readline()
      outf = outunknown
      if ( seq2 in outfs ):
        outf = outfs[seq2]
      outf.write(f1.readline())
      outf.write(f1.readline())
      outf.write(f1.readline())
      outf.write(f1.readline())

    outunknown.close()
    for bc in outfs:
      outfs[bc].close() </pre>

  - See the expected output. (Number of unclassified reads)
    <pre>
    $ zcat data/bioboot_2015a_R1.fastq.gz | awk 'NR % 4 == 2 {print;}' | wc -l
    100000
    $ zcat zcat out/bioboot_2015a_UNKNOWN.fastq.gz | awk 'NR % 4 == 2 {print;}' | wc -l
    4976 </pre>
    - 4.98% of reads are unclassified.

#### 6c. Answer for FASTQ Demultiplexing - Allowing one mismatch
  - Below is the example solution allowing up to one mismatch
    <pre>
    import gzip

    bc2id = {'ACAGTGA' : 'Sample1', 'CAGATCA' : 'Sample2' , 'GCCAATA' : 'Sample3', 'TGACCAA' : 'Sample4' ,'TTAGGCA' : 'Sample5'}
    outfs = {}
    nucleotides = ('N','A','C','G','T')
    for bc in bc2id:
      f = gzip.open('out/bioboot_2015a_'+bc2id[bc]+'.fastq.gz','w')
      outfs[bc] = f
      bclist = list(bc)  # convert string to list
      for i in range(len(bclist)): # choose position to substitute
        for n in nucleotides:    # choose a nucleotide substitute to
          bclist[i] = n
          newbc = "".join(bclist)
          if ( newbc != bc ):
            outfs[newbc] = f
      bclist[i] = bc[i] # convert the character back to the original one
    outunknown = gzip.open('out/bioboot_2015a_UNKNOWN.fastq.gz','w')

    f1 = gzip.open('data/bioboot_2015a_R1.fastq.gz')
    f2 = gzip.open('data/bioboot_2015a_R2.fastq.gz')

    for rname2 in f2:
      seq2 = f2.readline().rstrip()
      dummy2 = f2.readline()
      qual2 = f2.readline()
      outf = outunknown
      if ( seq2 in outfs ):
        outf = outfs[seq2]
      outf.write(f1.readline())
      outf.write(f1.readline())
      outf.write(f1.readline())
      outf.write(f1.readline())

    outunknown.close()
    for bc in outfs:
      outfs[bc].close() </pre>

  - See the expected output. (Number of unclassified reads)
    <pre>
    $ zcat data/bioboot_2015a_R1.fastq.gz | awk 'NR % 4 == 2 {print;}' | wc -l
    100000
    $ zcat zcat out/bioboot_2015a_UNKNOWN.fastq.gz | awk 'NR % 4 == 2 {print;}' | wc -l
    3556 </pre>
    - 3.56% of reads are unclassified.

### III-1) Understanding how to access FASTA files.

#### 7. Are the two subsets of sequences in the above identical to each other?
  - You can extract the two sequences into separate files using `samtools`
  
    <pre>
    $ samtools faidx data/hs37d5.fa 20:10000000-10001000 > out/day3.extracted.hs37d5.txt
    $ samtools faidx data/ucsc.hg19.fasta chr20:10000000-10001000 > out/day3.extracted.hg19.txt </pre>
  - Running `diff` on these data will highlight many differences in case.
  
    <pre>
    $ diff out/day3.extracted.hs37d5.txt out/day3.extracted.hg19.txt
    1,10c1,10
    < >20:10000000-10001000
    < TTGTTTACTACAGCTTTGTAGTAAATTTTGAACTCTAAAGTGTTAGTTCTCTAACTTTGT
    < TTGTTTTTCAAGAGTGTTTTGACTCTTCTTACTGCATCCCTTGCATTTCCATATGGACTT
    < TATAATCAGCCATGTCAACTTCTGCAAGAAAGACAGCTAGGATTTTGATAAGGATTGTGT
    < TGAATCTGTAGTCCAATTTTGGGAATACTACCATGTTAACATCGTCTTCCCATCCATGCA
    < CGTGCAATAGCTTACCATTTATTTGGTCTTCCTCAATTTCTGTCAACAATGATTTGTAGT
    < TTTCAGTTGCAAGTCTTGCACTTCTTTTGTTAAACTTTTTCCAAATATTTATTCTTCTTA
    < ATTACATAATTCTCATAATTAATAAAATTCTGAAATTTTCTTAATTTCATTTTTGTGGCC
    < ATGCACTTTAAAACTTGCCTTTTAACAAGACTTCCAGATGATTCTTGTGAACATTAAGTT
    < TGTGAAGTGCTTCTCTATTGACAAACTGAACTCACGTGACATTCCACACAGCACTGGCAA
    ---
    > >chr20:10000000-10001000
    > Ttgtttactacagctttgtagtaaattttgaactctaaagtgttagttctctaactttgt
    > ttgtttttcaagagtgttttgactcttcttactgcatcccttgcatttccatatggactt
    > tataatcagccatgtcaacttctgcaagaaagacagctaggattttgataaggattgtgt
    > tgaatctgtagtccaattttgggaatactaccatgttaacatcgtcttcccatccatgca
    > cgtgcaatagcttaccatttatttggtcttcctcaatttctgtcaacaatgatttgtagt
    > tttcagttgcaagtcttgcacttcttttgttaaactttttccaaatatttattcttctta
    > attacataattctcataattaataaaattctgaaattttcttaatttcatttttgTGGCC
    > ATGCACTttaaaacttgccttttaacaagacttccagatgattcttgtgaacattaagtt
    > tgtgaagtgctTCTCTATTGACAAACTGAACTCACGTGACATTCCACACAGCACTGGCAA
    17,18c17,18
    < CTTTCTGGTTCCAGATCTGACACCAATTACCTTTCTGATTTTGGACAAACCACTTAATAT
    < TTGTAACTTACAATTACTTCAACTGAATAATAAAAGAATTG
    ---
    > CTttctggttccagatctgacaccaattacctttctgattttggacaaaccacttaatat
    > ttgtaacttacaattacttcaactgaataataaaagaattg </pre>
  - Changing the UCSC (hg19) sequences to uppercases will show that they are identical.
    <pre>
    $ samtools faidx data/ucsc.hg19.fasta chr20:10000000-10001000 | awk '{ print toupper($0); }' > out/day3.extracted.hg19.txt
    $ diff out/day3.extracted.hs37d5.txt out/day3.extracted.hg19.txt 
    1c1
    < >20:10000000-10001000
    ---
    > >CHR20:10000000-10001000 </pre>


### III-3) Representing genes and transcripts using GTF and genePred format

#### 2a. How many total refFlat entries exist? What is the unique number of canonical gene names and transcript IDs that exist in the refFlat database?
  - Number of total refFlat entries
    <pre>
    $ zcat refFlat.txt.gz | wc -l
    54196 </pre>
  - Number of unique canonical gene names
    <pre>
    $ zcat refFlat.txt.gz | cut -f 1 | sort | uniq | wc -l
    26397 </pre>
  - Number of unique transcript IDs
    <pre>
    $ zcat refFlat.txt.gz | cut -f 2 | sort | uniq | wc -l
    50354 </pre>

#### 2b. How many different transcript exist for *LDLR* gene? How are they similar and different?
  - List all transcripts with canonical gene name *LDLR*
    <pre>
    $ zcat refFlat.txt.gz | grep -w LDLR
    LDLR	NM_001195799	chr19	+	11200037	11244505	11200224	11241992	17	11200037,11210898,11215895,11217240,11218067,11221327,11222189,11223953,11224210,11226769,11227534,11230767,11231045,11233849,11238683,11240188,11241956,	11200291,11211021,11216276,11217363,11218190,11221447,11222315,11224125,11224438,11226888,11227674,11230909,11231198,11234020,11238761,11240346,11244505,
    LDLR	NM_001195798	chr19	+	11200037	11244505	11200224	11241992	18	11200037,11210898,11213339,11215895,11217240,11218067,11221327,11222189,11223953,11224210,11226769,11227534,11230767,11231045,11233849,11238683,11240188,11241962,	11200291,11211021,11213462,11216276,11217363,11218190,11221447,11222315,11224125,11224438,11226888,11227674,11230909,11231198,11234020,11238761,11240346,11244505,
    LDLR	NM_001195803	chr19	+	11200037	11244505	11200224	11241992	16	11200037,11210898,11213339,11217240,11218067,11221327,11222189,11223953,11224210,11226769,11227534,11230767,11233849,11238683,11240188,11241956,	11200291,11211021,11213462,11217363,11218190,11221447,11222315,11224125,11224438,11226888,11227674,11230909,11234020,11238761,11240346,11244505,
    LDLR	NM_001195800	chr19	+	11200037	11244505	11200224	11241992	16	11200037,11210898,11213339,11218067,11221327,11222189,11223953,11224210,11226769,11227534,11230767,11231045,11233849,11238683,11240188,11241956,	11200291,11211021,11213462,11218190,11221447,11222315,11224125,11224438,11226888,11227674,11230909,11231198,11234020,11238761,11240346,11244505,
    LDLR	NM_000527	chr19	+	11200037	11244505	11200224	11241992	18	11200037,11210898,11213339,11215895,11217240,11218067,11221327,11222189,11223953,11224210,11226769,11227534,11230767,11231045,11233849,11238683,11240188,11241956,	11200291,11211021,11213462,11216276,11217363,11218190,11221447,11222315,11224125,11224438,11226888,11227674,11230909,11231198,11234020,11238761,11240346,11244505, </pre>
  - They all have the same start/end positions, but the number of exons and coding sequences are slightly different.


#### 2c. For multiple transcripts with the same canonical gene names, are they always located in the same chromosome or not?
  - First, the total number of canonical gene names is `26,397`
    <pre>
    $ zcat refFlat.txt.gz | cut -f 1 | sort | uniq | wc -l
    26397 </pre>
  - Next, the total number of unique chromosome x canonical gene names is `27,843`
    <pre>
    $ cat refFlat.txt.gz | cut -f 1,3 | sort | uniq | wc -l
    27843 </pre>
  - This means that up to 1,446 genes are duplicated across multiple chromosomes.

#### 4a. How many genes, transcripts, exons, and CDS exists in GENCODE v19 GTF?
  - You can check the third column in the GTF
    <pre>
    $ zcat data/gencode.v19.annotation.gtf.gz | grep -v ^# | cut -f 3 | sort | uniq -c
    723784 CDS
    1196293 exon
    57820 gene
    114 Selenocysteine
    84144 start_codon
    76196 stop_codon
    196520 transcript
    284573 UTR </pre>


### III-4)  Working with BED files.

#### 3. Write a python program that print out lines that overlaps with a region.
<pre>
import sys

script, chrom, beg, end = sys.argv

f = open('refFlat.bed','r')
for line in f:
    tok = line.split()
    if ( ( tok[0] == chrom ) and ( ( tok[2] > beg ) and ( tok[1] < end ) ) ):
        print line,
f.close() </pre>