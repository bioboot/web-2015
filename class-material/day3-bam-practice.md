---
layout: page
---

### III-2. Accessing aligned sequence reads in SAM/BAM format

In this part, we will briefly learn how to access SAM/BAM format files for genomic analysis.

1. Use `samtools` to view a BAM file within a specific region
   <pre>
   $ samtools view data/HG00096.chrom20.ILLUMINA.bwa.GBR.exome.20120522.bam 20:10000000 | less
   $ samtools view data/HG00096.chrom20.ILLUMINA.bwa.GBR.exome.20120522.bam 20:10000000 | less -S </pre>
   - What is the difference? Which way is more convenient to you?
   - Can you interpret what you see on the screen?

2. Understand how to read the BAM file header.
   <pre>
   $ samtools view -H data/HG00096.chrom20.ILLUMINA.bwa.GBR.exome.20120522.bam | less -S </pre>
   - Which version of reference genome is it aligned to?
   - What is the sample ID for the sequence reads? In which center and by which platform was it sequenced?
   - Which software tools are used to produce the data?

3. Examine sequence reads at a potential variant site
   - From the 1000 Genomes VCF, see where the individual have protein-altering non-reference genotypes.
     <pre>
     $ tabix data/ALL.chr20.phase3_shapeit2_mvncall_integrated_v5.20130502.genotypes.annotation.vcf.gz 20:10000000-11000000 | cut -f 1,2,4,5,8,10 | grep -v "0|0" | grep missense
     20	10603750	G	A	AC=328;AF=0.0654952;AN=5008;NS=2504;DP=19050;EAS_AF=0.004;AMR_AF=0.1066;AFR_AF=0.0121;EUR_AF=0.1322;SAS_AF=0.1033;AA=G|||;CSQ=A|ENSG00000149346|ENST00000334534|Transcript|missense_variant|1130|950|317|R/Q|cGa/cAa|||1|tolerated(0.38)|benign(0.008)||||;GENCODE=CDS_chr20:10603307-10604024;ERB=A||proximal_78873|Regulatory_Feature|proximal_enhancer	0|1 </pre>
   - Use `samtools tview` to visualize the sequence reads in the position
     <pre>
     $ samtools tview data/HG00096.chrom20.ILLUMINA.bwa.GBR.exome.20120522.bam data/hs37d5.fa </pre>
   - Press `g` to enter the position to navigate to
   - Type `20:10603750` to jump to the variant site
   - Does it look like a variant site or not?
   - Press `?` for more options.
   - [IGV](http://www.broadinstitute.org/igv) is more comprehensive tools to visualize BAM and sequence data.
   - [UCSC Genome Browser](http://genome.ucsc.edu) also provides an interface to load aligned sequence reads.

4. Create a pileup of sequence reads to process the sequence data by genomic coordinate
   - Generate the pileup of sequence reads around the variant site examined above
     <pre>
     $ samtools mpileup -r 20:10603745-10603755 -f data/hs37d5.fa data/HG00096.chrom20.ILLUMINA.bwa.GBR.exome.20120522.bam
     [mpileup] 1 samples in 1 input files
     <mpileup> Set max per-file depth to 8000
     20	10603745	T	170	,$,...,.....,,,.,,,,.,.,.,,.,,,,...,,,,,.....,.,,,,,,,,,,,,,,,.,,..,,,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,,.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,^],	3LBBHOACGEGPPJBPOPNHPIPDPNFQ@PPG@GPNLPPE@EEAPiPPHQPPJOPPPJQQLAQKEHQGKQLJLPGDHQQQQGQQLKIPLDGBKKMMFBIQIJQEPJG@NJDQFKQQDIMKEFJGBDQOJEKMOFMGJKHHDMHJJLNKJNGGMNJNMIFMB<KJEIJKFE
     20	10603746	G	174	,...,.....,,,.,,,,.,.,.,,.,,,,...,,,,,.....,.,,,,,,,,,,,,,,,.,,..,,$,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,,.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,,^].^].^].^],^],	LFFHPHHHFFPKPHQPQNJKNQGQNHQ;QQHFIQNQQQJGDHFQlQQFQQQQPQQKQQQQGQQOSQ4QQLQLQIFQKIQLRQRSQLQRRRHSRMRRHLQRRQKQQ<DMNQRLRFRJRKRK?JQRJQRMMFLMRKDKFQIIMICNELFQR?ILQQQIKDK2GNMCFHNCC323CC
     20	10603747	A	175	,...,.....,,,.,,,,.,.,.,,.,,,,...,,,,,.....,.,,,$,,,,,,,,,,,,.,,..,,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,,.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,,...,,^],^],	JIGHNHGHFHCOOEONOLGOJO?PLFOEOOGDGJ?OOOHHHJGOlPK4POOPNPOOOPOOCOOMNOOOKPKONFODOKPMPPPPPOPELEMPLQLGLPPPJDOP:CLHKPLLLPIIKPJADKGJKOIJHHCKLELHKLILIBLFLHOOGDINKNLKGK?OFJCFHLEEL:>FDBB
     20	10603748	T	174	,$...,.....,,,.,,,,.,.,.,,.,,,,...,,,,,.....,.,,,,,,,,,,,,,,.,,..,,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,,.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,,...,,,,	3DADNFEG@FKHN0OHJLHOHICOLCO?JKD<BO;OOOHDEIBJqFJPOJONOOFOPFE?OILFOOOLPLKL?OOOKPJPLLOOKPCLCPELMKEMOPPLIJO@:MMPPKKPPIMLPIDIOPIOOLLJGPPECLHOIHLEKIJJJKOHILOOOMBFLBOMJEJ@MGIL7>IFED
     20	10603749	C	172	...,.....,,,.,,,,.,.,.,,.,,,,...,,,,.....,.,,,,,,,,,,,,,,.,,..,,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,,$.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,,...,,,,	;DEGCCD@E@DL=QGH<IRJQ=R>EI=MQI0DHIHQI?DIBJnRLRRQFQIHQHIBQ=QRHHIIREIDIRARLRL>MIERRIQRDMALRMNLGJBIRRCMI2BFNIRLQMJIEFQLB>HQH?Q@FAMQIL@DBQGDJHKNFHGBK@:KKQPMK8E?KOJ:BDO>FN5>?@ED
     20	10603750	G	174	.AA,A..AA,,aAa,,,$Aa.a.aA,,aaA..,,,aA.AA.aAaaaaaaa,,a,,,aAaaA.,,a,,,,..aaaa,.,,Aa,aa..AAAaa.A,,,AaAa,.,a,a.Aa,.,,AAa,,AA,A,,,a..a,,,AaAaaaaaaaA.a,a..AaA,,a.AA,,,A,a.AA,,aaa^].^],^],	@8INIA@=JPQNBONL4HOEO>OGQ?OOF@@QQQOJ?GK=OsPOPOOPNQPNPQQO?OPI=QQPLQKQJ;OOPPQFQQQOQOP@IGQQKPIKRQJQOKOQ<RLMPIQBRBMMMMADPQFQPKFKLIIGFILPGJLPLMFHHPIGJMHIPMLEL>HOPIIMO<EB@>EFED?3CE
     20	10603751	A	174	..,.....,,,.,,,.,.,.,.,,,,...,,,,.....,.,,,,,,,,,,,,,,.,,..,,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,,...,,,,,..,,^],	8FLEIIEDNONFOLJKOJOJOCO0OOGCEOPOOGHEDFOmPOPOOPNOONOPPO@OOLHOOOKPJOGEOOOOPCPPMOKOPEHAMQLPKKPPPMPKIODJQPPJLPPIPQPLDFHLBOPLLIMOLIHKIPIJMIILHMGPOEFKOOIOKDL8OOPHILOBFD;>GFED;/LGGE
     20	10603752	T	174	.$.,.....,,,.,,,.,.,.,.,,,,...,,,,.....,.,,,,,,,,,,,,,,.,,..,,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,,...,,,,,..,,,	YDLFgDAFOPDFPGEGPIHGPJO9PPHA@PPOOIGGG@PqPPPPOPNPKIHQLO0PPLFLPPLQKPHDOPKJPLPPPJPPJEJEPPLQLJLLQPQCPPDQMMMIPLQEMLPKCAOPIPPJLLMPPEFIMOIHKIEKHIICOCGLOOOKJEMDONJGHMN0DNDCDI@HALLIHH
     20	10603753	T	176	.,.....,,,.,,,$.,.,.,.,,,,...,,,,.....,.,,,,,,,,,,,,,,.,,..,,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,,...,,,,,..,,,,^].^].	EKDcFEEMNMAID3GNHNGNJJ9NNG;ANNNNJHHJENkOOONNOLNIMMOOF?NJKENOOIOJNICNJOOOGOOPGONOHKFKLJOKKKOKKOJDNBOKPIKPOPPOKPB@FNP@OPILEJPPHFJFOBJCFHGDBCOP:JDOOOGJBJ=OHODEGN4BN;OEFBE@LMDF>D33
     20	10603754	A	176	.$,$.....,,,.,,$.,.,.,.,,,...,,,,.....,.,,,,,,,,,,,,,,.,,..,,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,,...,,,,,..,,,,..^].^],	C3DkDD?PQPdR3GRHRERHMQQB=ERRMQEEEDDQrRRRRQMNRMLJRMQ@RRJFRRRMRMQFDQRRRRLRRJRLRSFKFKPNSKDRRRKRARRASSSSJORSJSSJD@NQJGRONNGOJILIOHOHHRLNOIKIOOINONOHNIHOANNOKIJNDIIANJLGKJHLJKIJKK3E
     20	10603755	G	172	.....,,,.,.,.,.,.,,,...,,,,.....,.,,,,,,,,,$,,,,,.,,..,,,,,,,..,,,,,.,,.,,,,.....,,..,,,.,.,,.,,,,..,,.,,..,,,..,.,,,,..,,,,.,.,,,,,,,..,,,...,.,,,...,,,.,...,,,,,..,,,,...,	GjFFGNONgPJPHPGPMKPPIFEPPPOIGGJEPmPLQPPQMP3OOQPODPPMGPPPLQIOREKQPKQNLLIQJPQHMHMRLQM?QQQRQJPPGRQQMLRLRRRKRGC@OQJQJKAHMRRHHQKQB?HILNCICQI?LNLQLDL?I7QPQCGGPGPBKDG9HAOPKHEDNNLC  </pre>
     - The first four columns represent chromosome, position, reference base, and depth.
     - The fifth column represent the base
       - `,` or `.` represents matching bases to reference genome.
       - `^]` or `$` represents beginning or end of the reads
       - Upper/lowercase alphebets represents mismatch to the refernece genome.
     - Which position appears to have a variant?
     - The pileup format from samtools is explained [HERE](http://samtools.sourceforge.net/pileup.shtml) in detail.

Congratulations! Well done in understanding the basics of SAM/BAM format.
<br>
Now let's go to next step [Representing genes and transcripts using GTF and genePred format](../class-material/day3-gtf-practice.html)
, or go back to [Day 3 Overview](../day3).