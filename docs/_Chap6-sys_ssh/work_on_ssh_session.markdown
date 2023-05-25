---
title: Bash, Git & SSH tricks
author: Lei Lei
category: notes
layout: post
---

# SSH
## Set up

### List of Linux HPCs and workstations

### Start session

### Specify port
If you would like to specify the port so that you can tunneling to this session (_e.g._ when using jupyter notebook):

~~~ ssh
ssh -L 8888:127.0.0.1:8888 llei@10.149.120.35
~~~

# Jupyter lab


# Files
## Download files from server

~~~ shell
scp -r llei@10.149.120.35:/home/llei/YNi3/archer2_AB3.tar.gz ./
~~~