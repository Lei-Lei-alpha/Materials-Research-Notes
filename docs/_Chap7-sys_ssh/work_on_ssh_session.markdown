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

~~~ bash
ssh -L 8888:127.0.0.1:8888 username@hostname
~~~

# Files
## Download files from server

~~~ bash
scp -r username@hostname:path_of_file local_path
~~~

## Copy file(s) to multiple directory

```bash
$ echo dir1 dir2 dir3 | xargs -n 1 cp -v myfile
'myfile' -> 'dir1/myfile'
'myfile' -> 'dir2/myfile'
'myfile' -> 'dir3/myfile'
```

## Find files

Find files modified within the last 7 days

``` bash
find . -type f -mtime -7 -print
```

Delete files found and print the file name
~~~bash
find . -name slurm*.out -delete -print
~~~

## Print tail of multiple files

```bash
tail ./*/vasp.log
```

## Print contents of file

### Specified lines of a file

If you want lines X to Y inclusive (starting the numbering at 1), use

```shell
tail -n "+$X" /path/to/file | head -n "$((Y-X+1))"
```

### Print entire file

Using cat

~~~shell
cat file.txt
~~~

Using `echo`

~~~shell
echo "$(<file.txt )"
~~~

# Flow control

## For loop

~~~shell
for i in $(seq 1 10);
do
echo $i
done
~~~

# Slurm

## Cancel jobs

~~~shell
scancel --state=PENDING --user=leilei
~~~

# Handy bash tricks for input scripts
## String
### Get string before or after character

```bash
$ str="GenFiltEff=7.092200e-01"
$ var_name=${str%=*} # Get content before =
$ value=${str#*=}  # Get content after =
```

