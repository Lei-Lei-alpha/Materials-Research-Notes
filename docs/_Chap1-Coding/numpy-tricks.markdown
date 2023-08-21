---
title: Numpy - practical tricks and code snipets
author: Lei Lei
category: notes
layout: post
---

## Boolean

### Invert

The ~ operator can be used as a shorthand for np.invert on ndarrays.

```python
x1 = np.array([True, False])
~x1
array([False,  True])
```

### np.histogram and np.digitize

The output of np.histogram actually has 10 bins; the last (right-most) bin includes the greatest element because its right edge is inclusive (unlike for other bins).

The np.digitize method doesn't make such an exception (since its purpose is different) so the largest element(s) of the list get placed into an extra bin. To get the bin assignments that are consistent with histogram, just clamp the output of digitize by the number of bins, using fmin.

```python
A = range(1,94)
bin_count = 10
hist = np.histogram(A, bins=bin_count)
np.fmin(np.digitize(A, hist[1]), bin_count)
```

```
Output:

array([ 1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  2,  2,  2,  2,  2,  2,  2,
        2,  2,  3,  3,  3,  3,  3,  3,  3,  3,  3,  4,  4,  4,  4,  4,  4,
        4,  4,  4,  5,  5,  5,  5,  5,  5,  5,  5,  5,  6,  6,  6,  6,  6,
        6,  6,  6,  6,  6,  7,  7,  7,  7,  7,  7,  7,  7,  7,  8,  8,  8,
        8,  8,  8,  8,  8,  8,  9,  9,  9,  9,  9,  9,  9,  9,  9, 10, 10,
       10, 10, 10, 10, 10, 10, 10, 10])
```
