---
title: 'Parallel Jobs on HPC'
date: 2022-10-15
<!-- permalink: /posts/2012/08/blog-post-4/ -->
categories: HPC
tags:
  - command line
  - HPC
  - slurm
---
<!-- This post will show up by default. To disable scheduling of future posts, edit `config.yml` and set `future: false`.  -->

<!-- ![cluster_logo](/images/posts_related/rsph_cluster.PNG) -->

<img src="/images/posts_related/rsph_cluster.PNG" width="50%" height="auto" >

## Life is short but jobs are infinite.

Tired of staring at a blank screen, waiting for your simulation results to materialize? Join the club! Learning parallel computing on an HPC is like discovering a secret shortcut to faster, more efficient research. It’s basically like having a personal time machine for your data.

<!--more-->

In the following example, I am trying to conduct 300k simulations using R. However, our HPC only allows 1000 parallel tasks per job-id and 20 jobs at a time. So the general idea for parallel working is to write some codes in .bash that can automatically submit jobs for us and for each job, it can span all available resources to perform simulation simultaneously. To achieve this goal, I will give out a template for running such large-scale simulations.



## For .R file

I usually pass two types of index into the R code:

- read/write path
- simulation related hyper-parameters

So I need the parse both numeric and path-like variables. You can safely copy and paste the following codes at the beginning of your R file:

**new_sim.R**
```r
#!/bin/bash
options(echo=FALSE) # if you want to see commands in .Rout file
args=(commandArgs(TRUE))
print(args)
arguments = matrix(unlist(strsplit(args,"=")),ncol=2,byrow = T)

for (args_i in 1:length(args)) {
  if(args_i %in% grep("/", args, value = FALSE, fixed = TRUE)){
    assign(arguments[args_i,1],arguments[args_i,2])
  }
  else{
    eval(parse(text = args[args_i]))
  }
}
````
<span style="color:red"> **Caveat**</span>:\
If you want to debug your code for certain simulation, you can set `options(echo=TRUE) ` temporately. Please remember to change it back to `FALSE` when it comes back to a large-scale simulation.


<br/>
Next, we can specify all scenarios using `expand.grid()` in R for all possible combinations of parameters, here is an example:

```r
# parameters of all simulations
params <- expand.grid(
  seed = 1:1000,
  n = c(25,50,100,200,400,800),
  arg1 = c(...),
  arg2 = c(...)
)
```


## For .bash file

In our case, we have 300k jobs to submit while HPC only allows 1000 parallel computing at most using "array". We thus need two `.bash` files:

- `new_submit.slurm`: perform 1000 parallel simulations using "array". 
- `new_sbatch.slurm`: successively submit `new_submit.slurm` with different array base number. (I will explain what it means later.)


Here are the example code for both `.bash` file: 

<br/>

**new_sbatch.slurm**
```bash
#!/bin/bash

# in terminal, submit the following code
# sbatch new_sbatch.slurm

path_read="/..../"  # read directory
path_save="/..../"  # write directory

# array base number
for iter in {0..299}
do

sbatch -J sim -o ${path_read}out/o_%A_%a.out -e ${path_read}err/e_%A_%a.err new_submit.slurm $iter $path_read $path_save

done
```

<br/>

**new_submit.slurm**
```bash
#!/bin/bash
#SBATCH --job-name=%J
#SBATCH --partition=preemptable
#SBATCH --array=1-1000

# in terminal, submit the following code (example for array base number 150)
# sbatch new_submit.slurm 150

##################### Change these constants ##############################
path_read="$2"  # read directory
path_save="$3"  # write directory


# pass in the array base number
let my_array_base="$1" 
# calculate set index number: array_base + array_task_id
let sim=${my_array_base}*1000+${SLURM_ARRAY_TASK_ID}


# if path directory doesn't exist, make it
[ ! -d ${path_save} ] && mkdir ${path_save}
[ ! -d ${path_read}out ] && mkdir ${path_read}out
[ ! -d ${path_read}err ] && mkdir ${path_read}err

module purge
module load R/4.0.3 # specify R version

chmod +x ${path_read}new_sim.R
srun Rscript ${path_read}new_sim.R path_read=${path_read} path_save=${path_save} sim=${sim}

exit 0
```

Here's a breakdown of how the array index works for the simulation:\
* **Base Value:** `my_array_base` is a starting point for each simulation. In this example, it ranges from 0 to 299.
* **Task ID:** `SLURM_ARRAY_TASK_ID` is a unique identifier assigned to each individual simulation job. In this case, it starts at 1 and goes up to 1000.
* **Combined Index:** To create a unique identifier for each simulation, we combine the base value and task ID using the formula: `sim = my_array_base * 1000 + SLURM_ARRAY_TASK_ID`.
* **Example:** If `my_array_base` is 5 and `SLURM_ARRAY_TASK_ID` is 999, the combined index (simulation number) would be 5 * 1000 + 999 = 5999. This value is then passed to the `.R` file for further processing.


<span style="color:red"> **Caveat**</span>:
1. turn off echo option in .R file: options(echo=FALSE)
2. change array base number `iter` in `new_sbatch.slurm` file manually (due to cluster capacity, usually 20 can be successfully submitted simultaneously, but we can adjust the range to a much larger number (saying 160..250), with submitting this file to a day-long-cpu partition to avoid kill.) 


<br/>

Well, one <span style="color:skyblue"> **good news**</span> is: if we only need to perform one particular simulation 1000 times for a certain scenario, we can simply revise `new_submit.slurm`:
- specify read/write path directly instead of using arguments passed from `new_sbatch.slurm`
- submit using following code: `sbatch new_submit.slurm 0`







