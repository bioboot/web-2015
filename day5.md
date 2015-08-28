---
layout: page
title: Day 5
permalink: /day5/
---

# Day 5.  Unified Analytical Group Projects 
On the final day of this bootcamp, we will group up with the other members of our table and embark on a joint effort to take what we have learned this past week and use it to explore a large-scale biological data set in a collaborative fashion. We will make use of the [Geuvadis Project](http://www.geuvadis.org/web/geuvadis/RNAseq-project) which combines data on genetic variation from the [1000 Genomes Project](http://www.1000genomes.org/) with gene expression measurements derived from RNA-sequencing generated in [Lappalainen et al, Nature 2013](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3918453/) to detect and visualize potential expression quantative trait loci ([eQTL](https://en.wikipedia.org/wiki/Expression_quantitative_trait_loci)). 

Typically, such research projects can take a very long time to generate the data and analyze the results. For the purposes of this bootcamp, we will be using a small subset of these data and will attempt to recreate the published results over these regions. Our goal is to give you a taste of what types of data exploration are now available to you with the simple yet powerful biocomputing tools you have learned and to serve as a foundation for your future research endeavors. 

<br>

### Schedule:

| Session | Time             | Topics                                                 |  
| :-----: |:----------------:| :------------------------------------------------------|  
| I       | 9:00-10:15 AM    | **Introduction to eQTLs and Overview of Project**      |  
|         | 10:15-10:30 AM   | Coffee Break                                           |   
| II      | 10:30-12:00 AM   | **Obtaining, Parsing and Formatting Data**             |   
|         | 12:00-1:00 PM    | Lunch                                                  |  
| III     | 1:00-2:15 PM     | **Parallel Association Testing and Visualization**    |  
|         | 2:15-2:30 PM     | Coffee Break                                           |  
| IV      | 2:30-4:00 PM     | **Group Presentations and Discussion**                 |  

<br>

### Instructors:
Ryan Mills (RM) 
Jacob Kitzman (JK)
Barry Grant (BG)
Hui Jiang (HJ)
<br>

### Class Questionnaire
Please help us improve this course by completing this [questionnaire](http://tinyurl.com/bioboot-questions). 

<br>

### Data Sets:
- Genotype data for 465 individuals
  - [Remote site](ftp://ftp.ebi.ac.uk/pub/databases/microarray/data/experiment/GEUV/E-GEUV-1/genotypes/)
  - Local FLUX directory: /scratch/biobootcamp_fluxod/remills/bioboot/geuvadis/genotypes
- Expression data for 465 individuals
  - [Remote site](ftp://ftp.ebi.ac.uk/pub/databases/microarray/data/experiment/GEUV/E-GEUV-1/analysis_results/)
  - Local FLUX directory: /scratch/biobootcamp_fluxod/remills/bioboot/geuvadis/analysis_results

<br>

### Project Resources
- [eQTL Introduction Slides](https://github.com/bioboot/web-2015/blob/gh-pages/class-material/handout_day5_intro-eqtl.pdf)

<br>

### Analysis notebooks
- [ipython notebook - eQTL exercise](https://github.com/bioboot/web-2015/blob/gh-pages/class-material/Day5.ipynb)
- [ipython notebook - eQTL exercise, with solutions](https://github.com/bioboot/web-2015/blob/gh-pages/class-material/Day5_solution.ipynb)


<br>

- For this exercise, we will need to run ipython notebook on flux. As with Day 4, start a notebook server with the following commands


  - Start a notebook server 
  - Notes:
    1. you need to know which host you're on.   The command line prompt will show this (e.g., <B>flux-login3</B>)
    2. you need to know which port your instance of ipython listens to.  The first few lines of output from ipython notebook will list this for you.

  <pre>
  remills@<b>flux-login3</b>:/scratch/biobootcamp_fluxod/remills/biobootcamp$ ipython notebook --ip=<B>flux-login3</b> --no-browser

  [I 13:26:18.660 NotebookApp] Using MathJax from CDN: https://cdn.mathjax.org/mathjax/latest/MathJax.js
  [I 13:26:19.220 NotebookApp] The port 8888 is already in use, trying another random port.
  [I 13:26:19.473 NotebookApp] Serving notebooks from local directory: /scratch/biobootcamp_fluxod/kitzmanj
  [I 13:26:19.473 NotebookApp] 0 active kernels
  [I 13:26:19.473 NotebookApp] The IPython Notebook is running at: http://flux-login3:<b>8889</b>/
  [I 13:26:19.473 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
  </pre>

  - A word of warning: in general, running on the head node of the flux cluster is not a good practice, because the head node controls the job queue for the entire cluster. Instead you would want to enter the queue, login to a compute node, and do your work there.

  - Now, your own ipython server is running on flux.  But you will need to open a tunnel to get there.  The below command will securely connect port 8889 on the computer flux-login3 to port 9000 on my computer. 

   <pre>
   ssh -L localhost:9000:<b>flux-login3:8889</b> YOURNAME@flux-login.engin.umich.edu
   </pre>


  - Navigate to http://localhost:9000 and make a new notebook

- Ipython notebook reminders:

  - Green box  - marks active cell. You are in EDIT mode.
  - Hit **escape** to exit mode mode and go to COMMAND mode.
  - In command mode:
    - **A** : insert new cell above
    - **B** : insert new cell below
    - **Enter** : edit the currently selected cell
    - **up/down arrow** : navigate up or down
  - In either:
    - **Shift-Enter** : run the current cell

  - Remember, you can run commands out of order in the notebook.


