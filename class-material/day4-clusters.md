---
layout: page
---

### V-1. Download example dataset and notebooks

- On your computer (with anaconda/ipython installed), download day3_plotting_data.tar.gz into a new directory (e.g., day3_plotting/ ) and unpack it
  
  <pre>
  mkdir day3_plotting

  cd day3_plotting
  </pre>

  download the [read count dataset and notebook](../class-material/day3_plotting_data.tar.gz)

  <pre>
  tar zxvf day3_plotting_data.tar.gz

  ipython notebook
  </pre>

  - Navigate to http://localhost:8888 and make a new notebook


- Or, if you'd like to run ipython notebook on flux:
  
  <pre>
  mkdir day3_plotting

  cd day3_plotting
  </pre>

  copy the read count dataset and notebook

  <pre>

  cp /scratch/biobootcamp_fluxod/kitzmanj/biobootcamp/day3_plotting_data.tar.gz ./

  tar zxvf day3_plotting_data.tar.gz

  </pre>

  - Start a notebook server 
  Notes:
    1. you need to know which host you're on.   The command line prompt will show this (e.g., <B>flux-login3</B>)
    2. you need to know which port your instance of ipython listens to.  The first few lines of output from ipython notebook will list this for you.

  <pre>
  kitzmanj@<b>flux-login3</b>:/scratch/biobootcamp_fluxod/kitzmanj/biobootcamp$ ipython notebook --ip=<B>flux-login3</b> --no-browser

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


