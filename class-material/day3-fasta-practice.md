---
layout: page
---

### III-1. Understanding how to access FASTA files

In this part, we will learn how to access FASTA files for genomic analysis.

1. If you're logged out, login back to the server.
   <pre>
   $ ssh [your_id]@flux-login.engin.umich.edu
   (...Enter login credentials...)
   $ cd bioboot/day3 </pre>

2. Explore the GRCh37(+decoy) reference in the data directory.

   <pre> $ less data/hs37d5.fa </pre>
  
3. List all chromosomes available in the FASTA file.
   - Hint 1 : use `grep` command.
   - Hint 2 : Adding `^` in the beginning of the `grep` query searches for the string at the beginning of the line.
   - You can list all chromosomes using the following command (make sure to include quotation marks)
     <pre> $ grep "^>" data/hs37d5.fa | less </pre>
   - Is this an efficient way to get the answer? Why?

4. Use FASTA index files produced by `samtools`.
   - `samtools` provides a convenient way to explore FASTA files.
   - Running `samtools index` on a FASTA file will create `.fai` file (you need a write permission to the directory).
     <pre> $ samtools index [input.fasta] </pre>
   - FASTA index file is human-readable.
     <pre>
     $ less data/hs37d5.fa.fai
     1       249250621       52      60      61
     2       243199373       253404903       60      61
     3       198022430       500657651       60      61
     4       191154276       701980507       60      61
     5       180915260       896320740       60      61
     6       171115067       1080251307      60      61
     7       159138663       1254218344      60      61
     8       146364022       1416009371      60      61
     9       141213431       1564812846      60      61
     10      135534747       1708379889      60      61
     11      135006516       1846173603      60      61
     ... </pre>
   - Each column represents
     1. Chromosome (or sequence) name
     2. Number of base pairs (or characters) in each chromosome.
     3. The byte position where each chromosome sequence starts
     4. Number of bases pairs appearing each line
     5. Number of bytes used for each line

5. Use `samtools faidx` to access an arbitrary region of genome.
   - If `.fai` file is available, you can randomly access the FASTA file, without scanning from the beginning of the file
   - For example, to look at the 1000 bases starting from the 10M-th bases at chromosome 20,
     <pre>
     $ samtools faidx data/hs37d5.fa 20:10000000-10001000
     >20:10000000-10001000
     TTGTTTACTACAGCTTTGTAGTAAATTTTGAACTCTAAAGTGTTAGTTCTCTAACTTTGT
     TTGTTTTTCAAGAGTGTTTTGACTCTTCTTACTGCATCCCTTGCATTTCCATATGGACTT
     TATAATCAGCCATGTCAACTTCTGCAAGAAAGACAGCTAGGATTTTGATAAGGATTGTGT
     TGAATCTGTAGTCCAATTTTGGGAATACTACCATGTTAACATCGTCTTCCCATCCATGCA
     CGTGCAATAGCTTACCATTTATTTGGTCTTCCTCAATTTCTGTCAACAATGATTTGTAGT
     TTTCAGTTGCAAGTCTTGCACTTCTTTTGTTAAACTTTTTCCAAATATTTATTCTTCTTA
     ATTACATAATTCTCATAATTAATAAAATTCTGAAATTTTCTTAATTTCATTTTTGTGGCC
     ATGCACTTTAAAACTTGCCTTTTAACAAGACTTCCAGATGATTCTTGTGAACATTAAGTT
     TGTGAAGTGCTTCTCTATTGACAAACTGAACTCACGTGACATTCCACACAGCACTGGCAA
     ATTCTGTCCCGTAACTCCGCTAGCTCTCCAACAAGAAGTGACTTGACGCAGCCCAAGGTT
     ACTACTTAACACAATGAATTAAATGTTTTTAAATAAGAGGAAGCAATAAGATTCTAAAGG
     CTTTTCTGTTTTAATTTTCATGCAATGGAAAACTGGTATTAAATATCTATTTAATTAGGA
     GGAAACTACAATGCTGACTTTTGTCTGAATTATGTAGATAAGTGATTCATTTGAAACAAT
     TATTTTGATAATTGTCAATTATCCATTTCATTTTAATGCATTTTTTATTCTTTTTTCAAA
     AATAGCAACAATTACAACAGTTAAACCTTATAATGAATATGTTTCCTAAACCCTGTTCTA
     CTTTCTGGTTCCAGATCTGACACCAATTACCTTTCTGATTTTGGACAAACCACTTAATAT
     TTGTAACTTACAATTACTTCAACTGAATAATAAAAGAATTG </pre>
   
6. The FASTA file used above is GRCh37 sequence used for the 1000 Genomes project. Another widely used version of GRCh37 sequence can be downloaded from UCSC (hg19).
   - The FASTA and index file is available.
     <pre> 
     $ less data/hs37d5.fa.fai
     chrM    16571   6       50      51
     chr1    249250621       16915   50      51
     chr2    243199373       254252555       50      51
     chr3    198022430       502315922       50      51
     chr4    191154276       704298807       50      51
     chr5    180915260       899276175       50      51
     chr6    171115067       1083809747      50      51
     chr7    159138663       1258347122      50      51
     chr8    146364022       1420668565      50      51
     ... </pre>
   - What are similarities and differences between the two versions of FASTA files?

7. Note the difference between the representations of chromosome names.
   - Running the same command on the new FASTA file would not work
   
     <pre>
     $ samtools faidx data/ucsc.hg19.fasta 20:10000000-10001000 </pre>
   - Adding 'chr' in the chromosome name will give the similar results, except that repeated sequences are represented in lowercases.
   
     <pre>
     $ samtools faidx data/ucsc.hg19.fasta chr20:10000000-10001000   
     >chr20:10000000-10001000
     Ttgtttactacagctttgtagtaaattttgaactctaaagtgttagttctctaactttgt
     ttgtttttcaagagtgttttgactcttcttactgcatcccttgcatttccatatggactt
     tataatcagccatgtcaacttctgcaagaaagacagctaggattttgataaggattgtgt
     tgaatctgtagtccaattttgggaatactaccatgttaacatcgtcttcccatccatgca
     cgtgcaatagcttaccatttatttggtcttcctcaatttctgtcaacaatgatttgtagt
     tttcagttgcaagtcttgcacttcttttgttaaactttttccaaatatttattcttctta
     attacataattctcataattaataaaattctgaaattttcttaatttcatttttgTGGCC
     ATGCACTttaaaacttgccttttaacaagacttccagatgattcttgtgaacattaagtt
     tgtgaagtgctTCTCTATTGACAAACTGAACTCACGTGACATTCCACACAGCACTGGCAA
     ATTCTGTCCCGTAACTCCGCTAGCTCTCCAACAAGAAGTGACTTGACGCAGCCCAAGGTT
     ACTACTTAACACAATGAATTAAATGTTTTTAAATAAGAGGAAGCAATAAGATTCTAAAGG
     CTTTTCTGTTTTAATTTTCATGCAATGGAAAACTGGTATTAAATATCTATTTAATTAGGA
     GGAAACTACAATGCTGACTTTTGTCTGAATTATGTAGATAAGTGATTCATTTGAAACAAT
     TATTTTGATAATTGTCAATTATCCATTTCATTTTAATGCATTTTTTATTCTTTTTTCAAA
     AATAGCAACAATTACAACAGTTAAACCTTATAATGAATATGTTTCCTAAACCCTGTTCTA
     CTttctggttccagatctgacaccaattacctttctgattttggacaaaccacttaatat
     ttgtaacttacaattacttcaactgaataataaaagaattg </pre>

8. **(DIY)** Are the two subsets of sequences in the above identical to each other?
   - Hint 1 : You can use `diff` utility to compare the contents between the two files.
   - Hint 2 : If you want to convert a string to uppercase, a simple `awk` one-liner can do it.
     <pre> $ (previous-command) | awk '{ print toupper($0); }' | (next-command) </pre>
   - Need help or want to check solutions? [CLICK HERE FOR SOLUTIONS](../class-material/day3-answers.html#are-the-two-subsets-of-sequences-in-the-above-identical-to-each-other)
   - Each FASTA file may have slight differences. It is important to understand where the FASTA file I am using is coming from (and you should be able to reproduce the FASTA file on your own).
   - If two FASTA files are based on the same genome build, for the primary chromosomes, the sequences will be identical (except for the upper-lower cases and handling of ambiguous bases).
   