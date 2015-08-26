---
layout: page
---

### III-2. Subsetting VCF files

- Extract the metadata and header
  
  Rename the VCF file 

  <pre>
  mv ALL.chr20.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz chr20.vcf.gz
  </pre>

  Recall that VCF files have metadata lines beginning with "##" and header lines beginning with "#"

  <pre>
  zcat chr20.vcf.gz | egrep "^##"
  </pre>

- Take a subset of rows 

  <pre>
  zcat chr20.vcf.gz | head -n100000 > chr20.first100krows.vcf
  </pre>

- Examine this file via less

  <pre>
  less chr20.first100krows.vcf
  </pre>

- Get the header
  
  <pre>
  cat chr20.first100krows.vcf | egrep "^#[^#]"
  </pre>

- Pick just a selection of columns
  
  <pre>
  cat chr20.first100krows.vcf | cut -f1-10,20,30 | less

  cat chr20.first100krows.vcf | cut -f1-10,20,30 > chr20.first100krows_3samps.vcf
  </pre>

- Pick variants on chromosome 20 between positions 70000 and 72000

  <pre>
  cat chr20.first100krows_3samps.vcf | awk '{OFS="\t"; if ($2 > 70000 && $2 < 71000){ print }}' 
  </pre>

- What if we wanted to do this for the whole file, somewhere in the middle?  What needs to change?

  <pre>
  zcat chr20.vcf.gz | awk '{OFS="\t"; if ($2 > 30000000 && $2 < 30001000){ print }}' 
  </pre>

- This is much too slow - so let's use tabix to build an index to the file to enable fast random access
  
  <pre>
  tabix 

  tabix -p vcf chr20.vcf.gz

  # wait ....
  </pre>

- Now we can quickly retreive individual rows

  <pre>
  tabix chr20.vcf.gz 20:30000000-30001000 | cut -f1-5
  </pre>

- How many variants in this region?

  <pre>
  tabix chr20.vcf.gz 20:30000000-30001000 | cut -f1-5 | wc -l
  </pre>

- How many are variant in the first sample in this region (column #10, HG00096)?

  <pre>
  tabix chr20.vcf.gz 20:30000000-30001000 | cut -f10 | wc -l
  tabix chr20.vcf.gz 20:30000000-30001000 | cut -f10 | grep -v "0.0" | wc -l
  </pre>

- How many are variant in two samples in this region for two individuals, HG00096 (from British in England and Scotland), and NA18506 (from Yoruba in Ibadan, Nigeria)?

  <pre>
  cat chr20.first100krows.vcf  | cut -f1-10,1775 > chr20.100krows.twoindivs.vcf

  cat chr20.100krows.twoindivs.vcf | grep CHROM | cut -f10 | head 

  cat chr20.100krows.twoindivs.vcf | grep CHROM | cut -f11 | head 

  cat chr20.100krows.twoindivs.vcf | grep -v "^#" | cut -f10 | grep -v "0.0" | wc -l

  cat chr20.100krows.twoindivs.vcf | grep -v "^#" | cut -f11 | grep -v "0.0" | wc -l
  </pre>

- Newly tabix index this file
  
  <pre>
  cat chr20.100krows.twoindivs.vcf | bgzip >  chr20.100krows.twoindivs.vcf.gz

  tabix -p vcf chr20.100krows.twoindivs.vcf.gz 
  </pre>


<br>
Back to [Day 3 Overview](../day3).