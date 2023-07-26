---
title: 'How to use RSPH cluster'
date: 2020-09-01
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

## Prerequisite
If you want to access the culster from outside the Emory (e.g. using Emory wifi), you need to connect to [Emory VPN](https://it.emory.edu/vpntools/) first.

<br/>

## A useful toolkit COMBO
- Windows User: 
	1. Windows PowerShell
	2. [WinSCP](https://winscp.net/eng/index.php)

- Mac user: 
	1. terminal
	2. a. [Cyberduck](https://cyberduck.io/) or [FileZilla](https://filezilla-project.org)\
	b. Remote mounting using sshfs (details of this method can be found below.)

<br/>

## Login to the cluster
This is where we interact with cluster, e.g. installing packages, submitting jobs, etc.

**How:**\
Open terminal (or PowerShell) on your computer, and then login using `ssh` with command:
```bash
ssh netid@clogin01.sph.emory.edu
```
*Note: NetID is your official network ID that is used to access all resources throughout Emory.*


<br/>
<br/>


**Details about mounting remote directory to local computer:**\
After following the steps, you don't need to use any SFTP client to move your file back and forth. \
There is one <span style="color:red"> **caveat**</span>: beware of deleting files in the mounted folder if you only have one copy here.

1. Install [sshfs](https://github.com/osxfuse/osxfuse/wiki/SSHFS) and [macFUSE](https://osxfuse.github.io/).\
*Note: If you encountered any issues in installing these for M1 chip, please see [this page](https://github.com/daiqile96/Tools/blob/main/M1ChipIssues/README.md).*

2. Create a local directory (destination on your own computer).
```bash
mkdir /Users/yutong/local_HPC_folder
``` 
3. Mount cluster directory to your local directory. Here is an example of mount remote folder `/home/yjin85` to local folder `/Users/yutong/yjin85/`:
```bash
sshfs yjin85@clogin01.sph.emory.edu:/home/yjin85 /Users/yutong/yjin85/ -o auto_cache -o follow_symlinks
```
4. If you mount with error in step 3 and want to re-mount, you should unmount first with the following:
```bash
sudo umount /Users/yutong/yjin85/ 
```

<br/>
<br/>

<!-- <span style="color:red">some **This is Red Bold.** text</span> -->

## Additional Resources

1. Some useful tips provided by the RSPH IT:
	- The [HPC Getting Started Guide](/files/posts_related/HPCGuide.pdf). 
	- website for BIOS at RSPH: [HPC Documentation](https://scholarblogs.emory.edu/rsph-hpc/). 
2. [SLURM quick start](https://slurm.schedmd.com/quickstart.html).
3. Dr. Wu's [post](http://www.haowulab.org//computing/RSPHcluster/rsph_newcluster.html) and Dr. Yang's [post](https://yanglab-emory.github.io/assets/ComputationSlides/RSPH_HPC_StartGuide_Yang.html).





