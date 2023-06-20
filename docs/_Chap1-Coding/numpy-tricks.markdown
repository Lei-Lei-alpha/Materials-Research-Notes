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


