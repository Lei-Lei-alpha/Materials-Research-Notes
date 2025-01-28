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

## Print contents of file

### Print tail of multiple files

```bash
tail ./*/vasp.log
```

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

### Print specific line of file

Use `awk`, the following command prints line 6 of `file` out:

~~~shell
awk "NR==6" file
~~~

Use `sed`:

~~~shell
sed -n "6p" file
~~~

### Delete lines matching pattern
Use `sed`, the following command deletes lines of file contains "pattern"

~~~shell
sed -i '/pattern/d' file
~~~

### Replace specific line of a file

Use `sed`, the following command replaces line 6 of file with "new text":

~~~shell
sed -i "6s/.*/new text/" file
~~~

The above code replace the line in place, if you want to keep the original file and save the replaced file into a new file:

~~~shell
sed "6s/.*/new text/" file > newfile
~~~

#### Replace string matching pattern in line
~~~shell
sed -i "6s/txt_to_replace/new_text/" file
~~~



### Sort file
Sort `csv` file based on the second column:

~~~shell
sort -t, -k2 -nr relaxed_energy.csv
~~~

-t, - field delimiter
-k2, - sort by 4th field only
-n - sort numerically (use -nr to sort in reverse order)

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
Cancel all pending jobs
~~~shell
scancel --state=PENDING --user=$USER
~~~

Cancel all jobs in `compute` partition

~~~shell
scancel -p compute -u $USER
~~~


# Handy bash tricks for input scripts
## String
### Get string before or after character

```bash
$ str="GenFiltEff=7.092200e-01"
$ var_name=${str%=*} # Get content before =
$ value=${str#*=}  # Get content after =
```

