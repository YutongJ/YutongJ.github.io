---
title: 'Some Useful Bash Commands'
date: 2022-09-15
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
This is a summary of some useful commands. Be careful about "space" usage in command line.

<br/>

**Example: Show current time:**
```bash
date +"%T"
````

**Example: count the number of files in certain folder:**
```bash
# go to certain folder
cd /folder_of_interest/
# count the number of files
ls | wc -l
````

**Example: Bash arithmetic expansion:**
The builtin shell expansion allows you to use the parentheses ((...)) to do math calculations.
```bash
# initiate a value for x and output it with 'echo'
x=3 && echo $x
# calculation using ((...))
echo $((x+2)) 
````




