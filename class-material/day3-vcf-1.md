---
layout: page
---

### III-1. Working with VCF files.

Here we will download a file of variant calls and genotypes from the 1000 Genomes project.  


- Log in to flux 
  
  <pre>
  ssh USERNAME@flux-login.engin.umich.edu
  </pre>

- Update your PATH to point to programs we will use
  

  <pre>
  which samtools   # this won't be able to find samtools

  export PATH=${PATH}:/scratch/biobootcamp_fluxod/kitzmanj/samtools-1.1/:/scratch/biobootcamp_fluxod/kitzmanj/samtools-1.1/htslib-1.1/ 

  which samtools   # this won't be able to find samtools
  </pre>

  - You should now be able to find and run samtools

    <pre>
    kitzmanj@flux-login3:/scratch/biobootcamp_fluxod/kitzmanj$ which samtools
    /scratch/biobootcamp_fluxod/kitzmanj/samtools-1.1/samtools

    kitzmanj@flux-login3:/scratch/biobootcamp_fluxod/kitzmanj$ samtools

    Program: samtools (Tools for alignments in the SAM format)
    Version: 1.1 (using htslib 1.1)

    Usage:   samtools <command> [options]
    ... 
    </pre>

- Make a new directory to work in 

  <pre>
  cd /scratch/biobootcamp_fluxod/$( whoami )/
  mkdir day3_vcf
  cd day3_vcf
  export BASE_DIR=/scratch/biobootcamp_fluxod/$( whoami )/day3_vcf
  </pre>

- Navigate in your web browser to this address: [ftp://ftp-trace.ncbi.nih.gov/1000genomes/ftp/release/20130502/]

- Download variant calls and genotypes for chr20. 

  - Right-click the link to ALL.chr20.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz and select "Copy Link Address"

  - Back in Flux, type "wget " and paste this link:

  <pre>
  wget ftp://ftp-trace.ncbi.nih.gov/1000genomes/ftp/release/20130502/ALL.chr20.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz
  </pre>

 - QC check #1: make sure the size of the downloaded file is right:

 <pre>
 ls -l ALL.chr20.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz
 </pre>

 - QC check #2: a "hash" of the file is a characteristic 'fingerprint' of its contents. Damage or truncation to the file will change the value of this hash. Often a file will be provided with an accompanying hash (e.g., an md5sum) which can be checked. 

 <pre>
 md5sum ALL.chr20.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz
 </pre>

<br>
Back to [Day 3 Overview](../day3).