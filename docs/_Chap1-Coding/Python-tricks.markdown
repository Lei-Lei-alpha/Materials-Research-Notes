---
title: Python practical tricks
author: Lei Lei
category: notes
layout: post
---

## Processing Files

### Copy files

In Python, you can copy the files using

*   **[`shutil`](https://docs.python.org/3/library/shutil.html)** module
*   **[`os`](https://docs.python.org/3/library/os.html)** module
*   **[`subprocess`](https://docs.python.org/3/library/subprocess.html)** module

* * *

~~~python
import os
import shutil
import subprocess 
~~~

* * *

#### Copying files using [`shutil`](https://docs.python.org/3/library/shutil.html) module

**[`shutil.copyfile`](https://docs.python.org/3/library/shutil.html#shutil.copyfile)** signature

~~~python
shutil.copyfile(src_file, dest_file, *, follow_symlinks=True)

# example    
shutil.copyfile('source.txt', 'destination.txt') 
~~~

* * *

**[`shutil.copy`](https://docs.python.org/3/library/shutil.html#shutil.copy)** signature

~~~python
shutil.copy(src_file, dest_file, *, follow_symlinks=True)

# example
shutil.copy('source.txt', 'destination.txt') 
~~~

* * *

**[`shutil.copy2`](https://docs.python.org/3/library/shutil.html#shutil.copy2)** signature

~~~python
shutil.copy2(src_file, dest_file, *, follow_symlinks=True)

# example
shutil.copy2('source.txt', 'destination.txt') 
~~~

* * *

**[`shutil.copyfileobj`](https://docs.python.org/3/library/shutil.html#shutil.copyfileobj)** signature

~~~python
shutil.copyfileobj(src_file_object, dest_file_object[, length])

# example
file_src = 'source.txt'  
f_src = open(file_src, 'rb')

file_dest = 'destination.txt'  
f_dest = open(file_dest, 'wb')

shutil.copyfileobj(f_src, f_dest) 
~~~

* * *

**Clarification on `shutil` module**

<table class="s-table">
    <thead>
        <tr>
            <th>Function</th>
            <th>
                Copies<br />
                metadata
            </th>
            <th class="">
                Copies<br />
                permissions
            </th>
            <th>Uses file object</th>
            <th>
                Destination<br />
                may be directory
            </th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="https://docs.python.org/3/library/shutil.html#shutil.copy" rel="noreferrer">shutil.copy</a></td>
            <td>No</td>
            <td class="">Yes</td>
            <td>No</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td><a href="https://docs.python.org/3/library/shutil.html#shutil.copyfile" rel="noreferrer">shutil.copyfile</a></td>
            <td>No</td>
            <td class="">No</td>
            <td>No</td>
            <td>No</td>
        </tr>
        <tr>
            <td><a href="https://docs.python.org/3/library/shutil.html#shutil.copy2" rel="noreferrer">shutil.copy2</a></td>
            <td>Yes</td>
            <td class="">Yes</td>
            <td class="">No</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td><a href="https://docs.python.org/3/library/shutil.html#shutil.copyfileobj" rel="noreferrer">shutil.copyfileobj</a></td>
            <td>No</td>
            <td>No</td>
            <td class="">Yes</td>
            <td>No</td>
        </tr>
    </tbody>
</table>


*   9 Note that [even the `shutil.copy2()` function cannot copy all file metadata](https://docs.python.org/3/library/shutil.html). – [wovano](/users/10669875/wovano "4,397 reputation") [Mar 11, 2022 at 10:34](#comment126266541_30359308)

#### Copying files using [`os`](https://docs.python.org/3/library/os.html) module

**[`os.popen`](https://docs.python.org/3/library/os.html#os.popen)** signature

~~~python
os.popen(cmd[, mode[, bufsize]])

# example
# In Unix/Linux
os.popen('cp source.txt destination.txt') 

# In Windows
os.popen('copy source.txt destination.txt') 
~~~

* * *

**[`os.system`](https://docs.python.org/3/library/os.html#os.system)** signature

~~~python
os.system(command)


# In Linux/Unix
os.system('cp source.txt destination.txt')  

# In Windows
os.system('copy source.txt destination.txt') 
~~~

* * *

#### Copying files using [`subprocess`](https://docs.python.org/3/library/subprocess.html) module

**[`subprocess.call`](https://docs.python.org/3/library/subprocess.html#subprocess.call)** signature

~~~python
subprocess.call(args, *, stdin=None, stdout=None, stderr=None, shell=False)

# example (WARNING: setting `shell=True` might be a security-risk)
# In Linux/Unix
status = subprocess.call('cp source.txt destination.txt', shell=True) 

# In Windows
status = subprocess.call('copy source.txt destination.txt', shell=True) 
~~~

* * *

**[`subprocess.check_output`](https://docs.python.org/3/library/subprocess.html#subprocess.check_output)** signature

~~~python
subprocess.check_output(args, *, stdin=None, stderr=None, shell=False, universal_newlines=False)

# example (WARNING: setting `shell=True` might be a security-risk)
# In Linux/Unix
status = subprocess.check_output('cp source.txt destination.txt', shell=True)

# In Windows
status = subprocess.check_output('copy source.txt destination.txt', shell=True) 
~~~

* * *


