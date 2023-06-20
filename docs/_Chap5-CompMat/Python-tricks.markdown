---
title: VASP errors and practical tricks
author: Lei Lei
category: notes
layout: post
---

## Inconsistent Bravais lattice types found for crystalline and reciprocal lattice

Suggested SOLUTIONS:
    1. Refine the lattice parameters of your structure,
    2. and/or try changing **SYMPREC**.

**SYMPREC** 

<div style="border-width: 1px; border-style:solid; border-color: #b2182b; border-radius:3px; background-color: #FFEFEF;">
    <div style="padding-left: 25px; padding-right: 10px;">
        <p>
            <span style="color: #b2182b; font-weight: 550;">SYMPREC:</span> determines to which accuracy the positions in the <b>POSCAR</b> file must be specified. Default $10^{-5}$.
        </p>
    </div>
</div>

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

### JSON

#### Load `json` file

``` python
import json

with open('site_occ.json') as file:
    parsed_json = json.load(file)

print(parsed_json)
```

## List, tuple and dict

### Remove all occurences of an element from a list

[Remove all occurrences of a value from a list?](https://stackoverflow.com/questions/1157106/remove-all-occurrences-of-a-value-from-a-list)

Functional approach:

**Python 3.x**

```python
>>> x = [1,2,3,2,2,2,3,4]
>>> list(filter((2).__ne__, x))
[1, 3, 3, 4]
```

### Sort list of lists

> Source [stackoverflow.com](https://stackoverflow.com/questions/2213923/removing-duplicates-from-a-list-of-lists)

```python
>>> k = [[1, 2], [4], [5, 6, 2], [1, 2], [3], [4]]
>>> import itertools
>>> k.sort()
>>> list(k for k,_ in itertools.groupby(k))
[[1, 2], [3], [4], [5, 6, 2]] 
```

[`itertools`](http://docs.python.org/library/itertools.html?highlight=itertools#module-itertools) often offers the fastest and most powerful solutions to this kind of problems, and is **well** worth getting intimately familiar with!

**Edit**: as I mention in a comment, normal optimization efforts are focused on large inputs (the big-O approach) because it's so much easier that it offers good returns on efforts. But sometimes (essentially for "tragically crucial bottlenecks" in deep inner loops of code that's pushing the boundaries of performance limits) one may need to go into much more detail, providing probability distributions, deciding which performance measures to optimize (maybe the upper bound or the 90th centile is more important than an average or median, depending on one's apps), performing possibly-heuristic checks at the start to pick different algorithms depending on input data characteristics, and so forth.

Careful measurements of "point" performance (code A vs code B for a specific input) are a part of this extremely costly process, and standard library module `timeit` helps here. However, it's easier to use it at a shell prompt. For example, here's a short module to showcase the general approach for this problem, save it as `nodup.py`:

```
import itertools

k = [[1, 2], [4], [5, 6, 2], [1, 2], [3], [4]]

def doset(k, map=map, list=list, set=set, tuple=tuple):
  return map(list, set(map(tuple, k)))

def dosort(k, sorted=sorted, xrange=xrange, len=len):
  ks = sorted(k)
  return [ks[i] for i in xrange(len(ks)) if i == 0 or ks[i] != ks[i-1]]

def dogroupby(k, sorted=sorted, groupby=itertools.groupby, list=list):
  ks = sorted(k)
  return [i for i, _ in itertools.groupby(ks)]

def donewk(k):
  newk = []
  for i in k:
    if i not in newk:
      newk.append(i)
  return newk

# sanity check that all functions compute the same result and don't alter k
if __name__ == '__main__':
  savek = list(k)
  for f in doset, dosort, dogroupby, donewk:
    resk = f(k)
    assert k == savek
    print '%10s %s' % (f.__name__, sorted(resk)) 
```

Note the sanity check (performed when you just do `python nodup.py`) and the basic hoisting technique (make constant global names local to each function for speed) to put things on equal footing.

Now we can run checks on the tiny example list:

```
$ python -mtimeit -s'import nodup' 'nodup.doset(nodup.k)'
100000 loops, best of 3: 11.7 usec per loop
$ python -mtimeit -s'import nodup' 'nodup.dosort(nodup.k)'
100000 loops, best of 3: 9.68 usec per loop
$ python -mtimeit -s'import nodup' 'nodup.dogroupby(nodup.k)'
100000 loops, best of 3: 8.74 usec per loop
$ python -mtimeit -s'import nodup' 'nodup.donewk(nodup.k)'
100000 loops, best of 3: 4.44 usec per loop 
```

confirming that the quadratic approach has small-enough constants to make it attractive for tiny lists with few duplicated values. With a short list without duplicates:

```
$ python -mtimeit -s'import nodup' 'nodup.donewk([[i] for i in range(12)])'
10000 loops, best of 3: 25.4 usec per loop
$ python -mtimeit -s'import nodup' 'nodup.dogroupby([[i] for i in range(12)])'
10000 loops, best of 3: 23.7 usec per loop
$ python -mtimeit -s'import nodup' 'nodup.doset([[i] for i in range(12)])'
10000 loops, best of 3: 31.3 usec per loop
$ python -mtimeit -s'import nodup' 'nodup.dosort([[i] for i in range(12)])'
10000 loops, best of 3: 25 usec per loop 
```

the quadratic approach isn't bad, but the sort and groupby ones are better. Etc, etc.

If (as the obsession with performance suggests) this operation is at a core inner loop of your pushing-the-boundaries application, it's worth trying the same set of tests on other representative input samples, possibly detecting some simple measure that could heuristically let you pick one or the other approach (but the measure must be fast, of course).

It's also well worth considering keeping a different representation for `k` -- why does it have to be a list of lists rather than a set of tuples in the first place? If the duplicate removal task is frequent, and profiling shows it to be the program's performance bottleneck, keeping a set of tuples all the time and getting a list of lists from it only if and where needed, might be faster overall, for example.

### Sort list of dicts

#### Sort by values

> Source: [stackoverflow.com](https://stackoverflow.com/questions/72899/how-to-sort-a-list-of-dictionaries-by-a-value-of-the-dictionary-in-python)

The [`sorted()`](https://docs.python.org/library/functions.html#sorted) function takes a `key=` parameter

``` python
newlist = sorted(list_to_be_sorted, key=lambda d: d['name']) 
```

Alternatively, you can use [`operator.itemgetter`](https://docs.python.org/library/operator.html#operator.itemgetter) instead of defining the function yourself

``` python
from operator import itemgetter
newlist = sorted(list_to_be_sorted, key=itemgetter('name')) 
```

For completeness, add `reverse=True` to sort in descending order

``` python
newlist = sorted(list_to_be_sorted, key=itemgetter('name'), reverse=True) 
```

#### Sort by key names

> Source: [stackoverflow.com](https://stackoverflow.com/questions/38644055/sort-list-of-dictionaries-based-on-keys)

Just use `sorted` using a list like `[key1 in dict, key2 in dict, ...]` as the key to sort by. Remember to reverse the result, since `True` (i.e. key is in dict) is sorted after `False`.

```
>>> dicts = [{1:2, 3:4}, {3:4}, {5:6, 7:8}]
>>> keys = [5, 3, 1]
>>> sorted(dicts, key=lambda d: [k in d for k in keys], reverse=True)
[{5: 6, 7: 8}, {1: 2, 3: 4}, {3: 4}] 
```

This is using _all_ the keys to break ties, i.e. in above example, there are two dicts that have the key `3`, but one also has the key `1`, so this one is sorted second.

### Remove a key from a dict

> Source: [stackoverflow.com](https://stackoverflow.com/questions/11277432/how-can-i-remove-a-key-from-a-python-dictionary)

To delete a key regardless of whether it is in the dictionary, use the two-argument form of [`dict.pop()`](http://docs.python.org/library/stdtypes.html#dict.pop):

``` python
my_dict.pop('key', None) 
```

This will return `my_dict[key]` if `key` exists in the dictionary, and `None` otherwise. If the second parameter is not specified (i.e. `my_dict.pop('key')`) and `key` does not exist, a `KeyError` is raised.

To delete a key that is guaranteed to exist, you can also use

``` python
del my_dict['key'] 
```

This will raise a `KeyError` if the key is not in the dictionary.

## Variables

### Create dynamic variable names

#### Using `for` loop

The creation of a dynamic variable name in Python can be achieved with the help of iteration.

Along with the `for` loop, the `globals()` function will also be used in this method.

The `globals()` method in Python provides the output as a dictionary of the current global symbol table.

The following code uses the `for` loop and the `globals()` method to create a dynamic variable name in Python.

```python
for n in range(0, 7):
    globals()['strg%s' % n] = 'Hello'
# strg0 = 'Hello', strg1 = 'Hello' ... strg6 = 'Hello'

for x in range(0, 7):
    globals()[f"variable1{x}"] = f"Hello the variable number {x}!"


print(variable5)
```

Output:

```bash
Hello from variable number 5!
```

#### Using a dictionary

A dictionary is one of the four built-in data-types provided by Python along with tuple, list, and set. It is used to store data in the form of key: value pairs. A dictionary is both ordered (in Python 3.7 and above) and mutable. It is written with curly brackets `{}`. In addition to this, dictionaries cannot have any duplicates.

A dictionary has both a key and value, so it is easy to create a dynamic variable name using dictionaries.

The following code uses a dictionary to create a dynamic variable name in python.

```python
var = "a"
val = 4
dict1 = {var: val}
print(dict1["a"])
```

Although the creation of a dynamic variable name is possible in Python, It is needless and unnecessary as data in Python is created dynamically. Python references the objects in the code. If the reference of the object exists, then the object itself exists.

Creating a variable in this way is not recommended.

