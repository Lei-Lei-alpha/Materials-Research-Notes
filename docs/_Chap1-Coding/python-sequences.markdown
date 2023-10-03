---
title: Python - sequences
author: Lei Lei
category: notes
layout: post
---

## Overview

### Types in terms of structure

<ul>
<li><em>container sequences</em></li>
<div style="margin-left: 15px;">Can hold items of different types, including nested containers. <i>E.g.</i> <code>list</code>, <code>tuple</code>, and <code>collections.deque</code>.</div>
<li><em>flat sequences</em></li>
<div style="margin-left: 15px;">Hold items of one simple type. <i>E.g.</i> <code>str</code>, <code>bytes</code>, and <code>array.array</code>.</div>
</ul>

<div style="border-style:solid; border-width: 1.5px 1.5px 1.5px 5px; border-color: #858585 #858585 #858585 #9a172f; border-radius:5px;">
    <div style="padding-left: 25px; padding-right: 10px;">
        <p>
            A <em>container sequence</em> holds references to the objects it contains which may be of any type.
        </p>
        <p>
            A <em>flat sequence</em> stores the value of its contents in its own memory space, not as distinct Python objects.
        </p>
    </div>
</div>


<div style="margin-top: 20px; border-width: 1px; border-style:solid; border-color: #b2182b; border-radius:3px; background-color: #FFEFEF;">
    <div style="padding-left: 25px; padding-right: 10px;">
        <p style="color: #9a172f; font-weight: 550; font-size: 105%; text-align: center;">Note</p>
        <p>Every Python object in memory has a header with metadata. <i>E.g.</i> the simplest object <code>float</code>
        <ul>
            <li><code>ob_refcnt</code>: metadata, reference count</li>
            <li><code>ob_type</code>: metadata, a pointer to the object's type</li>
            <li><code>ob_fval</code>: value, a <code>C double</code> holding the value of the <code>float</code></li>
        </ul>
        </p>
    </div>
</div>

### Types in terms of mutability

<ul>
<li><em>mutable sequences</em></li>
<div style="margin-left: 15px;"><i>E.g.</i> <code>list</code>, <code>bytearray</code>, and <code>collections.deque</code>.</div>
<li><em>immutable sequences</em></li>
<div style="margin-left: 15px;"><i>E.g.</i> <code>tuple</code>, <code>str</code> and <code>bytes</code>.</div>
</ul>

```python
>>> from collections import abc
>>> issubclass(tuple, abc.Sequence)
True
>>> issubclass(list, abc.MutableSequence)
True
```

