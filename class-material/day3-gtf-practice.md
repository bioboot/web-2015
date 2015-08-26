---
layout: page
---

### III-3. Representing genes and transcripts using GTF and genePred format

In this part, we will learn how to GENCODE GTF and UCSC genePred format for representing genes and transcripts.

1. UCSC Genome Bioinformatics sites provides useful resources for understanding human genomes. For protein-coding genes, it provides a 'refFlat' file containing the coordinates or transcripts and coding sequences in a succint way.
   - Download the refFlat file from UCSC FTP site
     <pre>
     $ wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/database/refFlat.txt.gz </pre>
   - Have a peek of the file contents
     <pre>
     $ zless refFlat.txt.gz
     DRD4    NM_000797       chr11   +       637304  640705  637304  640603  4       637304,639432,639647,640400,    637589,639545,640306,640705,
     GABRA2  NM_000807       chr4    -       46251580        46392056        46252324        46390723        10      46251580,46263942,46305476,46307584,46312189,46314512,46334631,46388090,46390652,46391751,      46252621,46264145,46305629,46307728,46312272,46314733,46334699,46388206,46390733,46392056,
     BHLHE23 NM_080606       chr20   -       61637330        61638387        61637400        61638126        1       61637330,       61638387,
     POR     NM_000941       chr7    +       75544419        75616173        75583310        75615799        16      75544419,75583306,75601730,75608768,75609656,75610365,75610834,75611541,75612837,75613055,75614094,75614375,75614896,75615240,75615476,75615654,        75544497,75583498,75601779,75608897,75609806,75610490,75610924,75611640,75612954,75613174,75614276,75614525,75615167,75615386,75615559,75616173,
     ... </pre>
    - What does each column mean? See [HERE](http://hgdownload-test.cse.ucsc.edu/goldenPath/ce3/database/refFlat.sql) if you understand what the SQL table is. Otherwise, see below.
      1. *Canonical* gene name (or gene symbol). This is the most widely used representation of gene names in human-readable form.
      2. mRNA transcript ID from NCBI. This is less ambiguous than canonical gene name, when multiple transcript encodes the same gene, but harder to be understood by human.
      3. Chromosome name where the gene is located on.
      4. Strand of transcriptional direction.
      5. The leftmost base position of gene for all exons.
      6. The rightmost base position of gene for all exons.
      7. The leftmost base position of coding sequences (excluding UTRs).
      8. The rightmost base position of coding sequences (excluding UTRs).
      9. The list of start position for each coding sequence.
      10. The list of end position for each coding sequence.
      11. The number of exons.
      12. The list of start position for each exon.
      13. The list of end position for each exon.

2. **(DIY)** Now let's try to answer to simple questions about the genes using the refFlat database.
   - **How many total refFlat entries exist? What is the unique number of canonical gene names and transcript IDs that exist in the refFlat database?**
     - The answers are `54196`, `26397`, and `50354`, respectively.
     - You can obtain these answers by combining `zcat`, `cut`, `sort`, `uniq`, and `wc` commands.
     - If you need more help, [CLICK HERE](../class-material/day3-answers.html#a-how-many-total-refflat-entries-exist-what-is-the-unique-number-of-canonical-gene-names-and-transcript-ids-that-exist-in-the-refflat-database)
   - **How many different transcript exist for *LDLR* gene? How are they similar and different?**
     - The answer is `5`
     - You can obtain these answers by combining `zcat`, `grep`, and `wc` commands. `-w` option is needed for `grep` to obtain the correct answer.
     - If you need more help, [CLICK HERE](../class-material/day3-answers.html#b-how-many-different-transcript-exist-for-ldlr-gene-how-are-they-similar-and-different)
   - **For multiple transcripts with the same canonical gene names, are they always located in the same chromosome or not?**
     - This could be a little more tricky.
     - You can obtain the answers by combining `zcat`, `cut`, `sort`, `uniq`, and `wc` commands.
     - If you need help or wants to check the answer, [CLICK HERE](../class-material/day3-answers.html#c-for-multiple-transcripts-with-the-same-canonical-gene-names-are-they-always-located-in-the-same-chromosome-or-not)

3. [GENCODE](http://www.gencodegenes.org/) is a part of ENCODE project to comprehensively identify all genes and their features. While UCSC refFlat database provides information of well known genes, GENCODE resources are more up-to-date to encompass recently identified genes and transcripts. GENCODE uses the Gene Transfer Format (GTF) to represent their contents.
   - Have a peek of downloaded contents of GTF file (GENCODE v19)
     <pre>
     $ zless data/gencode.v19.annotation.gtf.gz
     ##description: evidence-based annotation of the human genome (GRCh37), version 19 (Ensembl 74)
     ##provider: GENCODE
     ##contact: gencode@sanger.ac.uk
     ##format: gtf
     ##date: 2013-12-05
     chr1    HAVANA  gene    11869   14412   .       +       .       gene_id "ENSG00000223972.4"; transcript_id "ENSG00000223972.4"; gene_type "pseudogene"; gene_status "KNOWN"; gene_name "DDX11L1"; transcript_type "pseudogene"; transcript_status "KNOWN"; transcript_name "DDX11L1"; level 2; havana_gene "OTTHUMG00000000961.2";
     chr1    HAVANA  transcript      11869   14409   .       +       .       gene_id "ENSG00000223972.4"; transcript_id "ENST00000456328.2"; gene_type "pseudogene"; gene_status "KNOWN"; gene_name "DDX11L1"; transcript_type "processed_transcript"; transcript_status "KNOWN"; transcript_name "DDX11L1-002"; level 2; tag "basic"; havana_gene "OTTHUMG00000000961.2"; havana_transcript "OTTHUMT00000362751.1";
     chr1    HAVANA  exon    11869   12227   .       +       .       gene_id "ENSG00000223972.4"; transcript_id "ENST00000456328.2"; gene_type "pseudogene"; gene_status "KNOWN"; gene_name "DDX11L1"; transcript_type "processed_transcript"; transcript_status "KNOWN"; transcript_name "DDX11L1-002"; exon_number 1;  exon_id "ENSE00002234944.1";  level 2; tag "basic"; havana_gene "OTTHUMG00000000961.2"; havana_transcript "OTTHUMT00000362751.1";     
     ... </pre>
   - If you want to understand what each column represent [CLICK HERE](http://useast.ensembl.org/info/website/upload/gff.html?redirect=no)
   - What is your impression to GTF format? What are the advantages and disadvantages of GTF format compared to refFlat format?

4. **(DIY)** Now let's try to answer to simple questions about the GENCODE databases
   - **How many genes, transcripts, exons, and CDS exists?**
     - The answers are `57,820`, `196,520`, `1,196,293` and `723,784`, respectively.
     - You can obtain these answers by combining `zcat`, `grep`, `cut`, `sort`, `uniq`  commands.
     - If you need more help, [CLICK HERE](../class-material/day3-answers.html#a-how-many-genes-transcripts-exons-and-cds-exists-in-gencode-v19-gtf)     
   - **See more comprehensive summary statistics [HERE](http://www.gencodegenes.org/stats/archive.html#a19). Do you think you can reproduce these results?**

Congratulations! Well done in understanding GTF and genePred (or refFlat) formats.
<br>
Now let's go to next step
[Working with BED files](../class-material/day3-bed-practice.html)
, or go back to [Day 3 Overview](../day3).