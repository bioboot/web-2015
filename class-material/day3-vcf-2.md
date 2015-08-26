---
layout: page
---

### III-2. Subsetting VCF files

- Extract the metadata and header
  
  Rename the VCF file 

  ```
  mv ALL.chr20.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz chr20.vcf.gz
  ```

  Recall that VCF files have metadata lines beginning with "##" and header lines beginning with "#"

  ```
  zcat chr20.vcf.gz | egrep "^##"

  ```

- Take a subset of rows 

  ```
  zcat chr20.vcf.gz | head -n100000 > chr20.first100krows.vcf

  ```

- Examine this file via less

  ```
  less chr20.first100krows.vcf
  ```

- Get the header
  ```
  cat chr20.first100krows.vcf | egrep "^#[^#]"

  ```
- Pick just a selection of columns
  
  ```
  cat chr20.first100krows.vcf | cut -f1-10,20,30 | less

  cat chr20.first100krows.vcf | cut -f1-10,20,30 > chr20.first100krows_3samps.vcf

  ```
- Pick variants on chromosome 20 between positions 70000 and 72000

  ```
  cat chr20.first100krows_3samps.vcf | awk '{OFS="\t"; if ($2 > 70000 && $2 < 71000){ print }}' 
  ```

- What if we wanted to do this for the whole file, somewhere in the middle?  What needs to change?

  ```
  zcat chr20.vcf.gz | awk '{OFS="\t"; if ($2 > 30000000 && $2 < 30001000){ print }}' 
  ```

- This is much too slow - so let's use tabix to build an index to the file to enable fast random access
  ```
  tabix 

  tabix -p vcf chr20.vcf.gz

  # wait ....

  ```
- Now we can quickly retreive individual rows

  ```
  tabix chr20.vcf.gz 20:30000000-30001000 | cut -f1-5

  ```

- How many variants in this region?

  ```
  tabix chr20.vcf.gz 20:30000000-30001000 | cut -f1-5 | wc -l

  ```

- How many are variant in the first sample in this region (column #10, HG00096)?

  ```
  tabix chr20.vcf.gz 20:30000000-30001000 | cut -f10 | wc -l
  tabix chr20.vcf.gz 20:30000000-30001000 | cut -f10 | grep -v "0.0" | wc -l

  ```

- How many are variant in two samples in this region for two individuals, HG00096 (from British in England and Scotland), and NA18506 (from Yoruba in Ibadan, Nigeria)?

  ```
  cat chr20.first100krows.vcf  | cut -f1-10,1775 > chr20.100krows.twoindivs.vcf

  cat chr20.100krows.twoindivs.vcf | grep CHROM | cut -f10 | head 

  cat chr20.100krows.twoindivs.vcf | grep CHROM | cut -f11 | head 

  cat chr20.100krows.twoindivs.vcf | grep -v "^#" | cut -f10 | grep -v "0.0" | wc -l

  cat chr20.100krows.twoindivs.vcf | grep -v "^#" | cut -f11 | grep -v "0.0" | wc -l

  ```

- Newly tabix index this file
  ``` 
  cat chr20.100krows.twoindivs.vcf | bgzip >  chr20.100krows.twoindivs.vcf.gz

  tabix -p vcf chr20.100krows.twoindivs.vcf.gz 
  ```


<br>
Back to [Day 3 Overview](../day3).