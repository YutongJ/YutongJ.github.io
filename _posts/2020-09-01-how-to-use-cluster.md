---
title: 'How to use RSPH cluster'
date: 2020-09-01
<!-- permalink: /posts/2012/08/blog-post-4/ -->
tags:
  - command line
  - HPC
  - slurm
---
<!-- This post will show up by default. To disable scheduling of future posts, edit `config.yml` and set `future: false`.  -->

![cluster_logo](/images/posts_related/rsph_cluster.PNG)

## Prerequisite
If you want to access the culster from outside the Emory (e.g. using Emory wifi), you need to connect to [Emory VPN](https://it.emory.edu/security/vpn.html) first.


## A useful toolkit combo
- Windows User: 
	1. Windows PowerShell
	2. [WinSCP](https://winscp.net/eng/index.php)

- Mac user: 
	1. terminal
	2. [Cyberduck](https://cyberduck.io/) or 
	3. Remote mounting using sshfs (details of this method can be found below.)


## Login to the cluster

Open terminal/PowerShell on your computer, and then login using `ssh` with command:
```
ssh yjin85@clogin01.sph.emory.edu
```



**Details about mounting remote directory to local computer:**
After following the following steps, you don't need to use any SFTP client to move your file back and forth. There is one caveat: beware of deleting files in the mounted folder if you only have one copy here.

1. Install [sshfs](https://github.com/osxfuse/osxfuse/wiki/SSHFS) and [macFUSE](https://osxfuse.github.io/).
*Note2: If you encountered any issues in installing these for M1 chip, please see [this page](https://github.com/daiqile96/Tools/blob/main/M1ChipIssues/README.md).*

2. Create a local directory (destination on your own computer).
```
mkdir /Users/yutong/local_HPC_folder
``` 
3. Mount cluster directory to your local directory. Here is an example of mount remote folder `/home/yjin85` to local folder `/Users/yutong/yjin85/`:
```
sshfs yjin85@clogin01.sph.emory.edu:/home/yjin85 /Users/yutong/Firmiana/yjin85/ -o auto_cache -o follow_symlinks
```
4. If you mount with error in step 3 and want to re-mount, you should unmount first with the following:
```
sudo umount /Users/yutong/yjin85/ 
```
*Note1: If you encountered any issues in installing these for Apple Mac with M1 chip, please see [this page](https://github.com/daiqile96/Tools/blob/main/M1ChipIssues/README.md).*




## Additional Resources

1. Some useful tips provided by the RSPH IT:
	- The [HPC Getting Started Guide](/files/posts_related/HPCGuide.pdf). 
	- website for BIOS at RSPH: [HPC Documentation](https://scholarblogs.emory.edu/rsph-hpc/). 
2. [SLURM quick start](https://slurm.schedmd.com/quickstart.html).
3. Dr. Wu's [post](http://www.haowulab.org//computing/RSPHcluster/rsph_newcluster.html) and Dr. Yang's [post](https://yanglab-emory.github.io/assets/ComputationSlides/RSPH_HPC_StartGuide_Yang.html).





