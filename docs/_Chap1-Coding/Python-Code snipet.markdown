---
title: Python - useful snippets for materials science
author: Lei Lei
category: notes
layout: post
---

## Fast distance calculation

### With periodic boundary condition

> [stackoverflow.com](https://stackoverflow.com/questions/52649815/fastest-code-to-calculate-distance-between-points-in-numpy-array-with-cyclic-pe)

These are 8 different solutions I've timed, some of my own and some posted in response to my question, that use 4 broad approaches:

1.  spatial cdist
2.  spatial KDtree
3.  Sklearn BallTree
4.  Kernel approach

This is the code with the 8 test routines:

~~~ python
import numpy as np
from scipy import spatial
from sklearn.neighbors import BallTree

n=500 # size of 2D box
f=200./(n*n) # first number is rough number of target cells...

np.random.seed(1) # to make reproducable
a=np.random.uniform(size=(n,n))
i=np.argwhere(a>-1)  # all points, we want to know distance to nearest point
j=np.argwhere(a>1.0-f) # set of locations to find distance to.

# long array of 3x3 j points:
for xoff in [0,n,-n]:
    for yoff in [0,-n,n]:
        if xoff==0 and yoff==0:
            j9=j.copy()
        else:
            jo=j.copy()
            jo[:,0]+=xoff
            jo[:,1]+=yoff
            j9=np.vstack((j9,jo))

global maxdist
maxdist=10
overlap=5.2
kernel_size=int(np.sqrt(overlap*n**2/j.shape[0])/2)

print("no points",len(j))

# repear cdist over each member of 3x3 block
def dist_v1(i,j):
    dist=[]
    # 3x3 search required for periodic boundaries.
    for xoff in [-n,0,n]:
        for yoff in [-n,0,n]:
            jo=j.copy()
            jo[:,0]+=xoff
            jo[:,1]+=yoff
            dist.append(np.amin(spatial.distance.cdist(i,jo,metric='euclidean'),1))
    dist=np.amin(np.stack(dist),0).reshape([n,n])
    #dmask=np.where(dist<=maxdist,1,0)
    return(dist)

# same as v1, but taking one amin function at the end
def dist_v2(i,j):
    dist=[]
    # 3x3 search required for periodic boundaries.
    for xoff in [-n,0,n]:
        for yoff in [-n,0,n]:
            jo=j.copy()
            jo[:,0]+=xoff
            jo[:,1]+=yoff
            dist.append(spatial.distance.cdist(i,jo,metric='euclidean'))
    dist=np.amin(np.dstack(dist),(1,2)).reshape([n,n])
    #dmask=np.where(dist<=maxdist,1,0)
    return(dist)

# using a KDTree query ball points, looping over j9 points as in online example
def dist_v3(n,j):
    x,y=np.mgrid[0:n,0:n]
    points=np.c_[x.ravel(), y.ravel()]
    tree=spatial.KDTree(points)
    mask=np.zeros([n,n])
    for results in tree.query_ball_point((j), 2.1):
        mask[points[results][:,0],points[results][:,1]]=1
    return(mask)

# using ckdtree query on the j9 long array
def dist_v4(i,j):
    tree=spatial.cKDTree(j)
    dist,minid=tree.query(i)
    return(dist.reshape([n,n]))

# back to using Cdist, but on the long j9 3x3 array, rather than on each element separately
def dist_v5(i,j):
    # 3x3 search required for periodic boundaries.
    dist=np.amin(spatial.distance.cdist(i,j,metric='euclidean'),1)
    #dmask=np.where(dist<=maxdist,1,0)
    return(dist)

def dist_v6(i,j):
    tree = BallTree(j,leaf_size=5,metric='euclidean')
    dist = tree.query(i, k=1, return_distance=True)
    mindist = dist[0].reshape(n,n)
    return(mindist)

def sq_distance(x1, y1, x2, y2, n):
    # computes the pairwise squared distance between 2 sets of points (with periodicity)
    # x1, y1 : coordinates of the first set of points (source)
    # x2, y2 : same
    dx = np.abs((np.subtract.outer(x1, x2) + n//2)%(n) - n//2)
    dy = np.abs((np.subtract.outer(y1, y2) + n//2)%(n) - n//2)
    d  = (dx*dx + dy*dy)
    return d

def apply_kernel1(sources, sqdist, kern_size, n, mask):
    ker_i, ker_j = np.meshgrid(np.arange(-kern_size, kern_size+1), np.arange(-kern_size, kern_size+1), indexing="ij")
    kernel = np.add.outer(np.arange(-kern_size, kern_size+1)**2, np.arange(-kern_size, kern_size+1)**2)
    mask_kernel = kernel > kern_size**2

    for pi, pj in sources:
        ind_i = (pi+ker_i)%n
        ind_j = (pj+ker_j)%n
        sqdist[ind_i,ind_j] = np.minimum(kernel, sqdist[ind_i,ind_j])
        mask[ind_i,ind_j] *= mask_kernel

def apply_kernel2(sources, sqdist, kern_size, n, mask):
    ker_i = np.arange(-kern_size, kern_size+1).reshape((2*kern_size+1,1))
    ker_j = np.arange(-kern_size, kern_size+1).reshape((1,2*kern_size+1))

    kernel = np.add.outer(np.arange(-kern_size, kern_size+1)**2, np.arange(-kern_size, kern_size+1)**2)
    mask_kernel = kernel > kern_size**2

    for pi, pj in sources:

        imin = pi-kern_size
        jmin = pj-kern_size
        imax = pi+kern_size+1
        jmax = pj+kern_size+1
        if imax < n and jmax < n and imin >=0 and jmin >=0: # we are inside
            sqdist[imin:imax,jmin:jmax] = np.minimum(kernel, sqdist[imin:imax,jmin:jmax])
            mask[imin:imax,jmin:jmax] *= mask_kernel

        elif imax < n and imin >=0:
            ind_j = (pj+ker_j.ravel())%n
            sqdist[imin:imax,ind_j] = np.minimum(kernel, sqdist[imin:imax,ind_j])
            mask[imin:imax,ind_j] *= mask_kernel

        elif jmax < n and jmin >=0:
            ind_i = (pi+ker_i.ravel())%n
            sqdist[ind_i,jmin:jmax] = np.minimum(kernel, sqdist[ind_i,jmin:jmax])
            mask[ind_i,jmin:jmax] *= mask_kernel

        else :
            ind_i = (pi+ker_i)%n
            ind_j = (pj+ker_j)%n
            sqdist[ind_i,ind_j] = np.minimum(kernel, sqdist[ind_i,ind_j])
            mask[ind_i,ind_j] *= mask_kernel


def dist_v7(sources, n, kernel_size, method):
    sources = np.asfortranarray(sources) #for memory contiguity
    kernel_size = min(kernel_size, n//2)
    kernel_size = max(kernel_size, 1)
    sqdist = np.full((n,n), 10*n**2, dtype=np.int32) #preallocate with a huge distance (>max**2)
    mask   = np.ones((n,n), dtype=bool)              #which points have not been reached?
    #main code
    if (method == 1):
        apply_kernel1(sources, sqdist, kernel_size, n, mask)
    else:
        apply_kernel2(sources, sqdist, kernel_size, n, mask)
      
    #remaining points
    rem_i, rem_j = np.nonzero(mask)
    if len(rem_i) > 0:
        sq_d = sq_distance(sources[:,0], sources[:,1], rem_i, rem_j, n).min(axis=0)
        sqdist[rem_i, rem_j] = sq_d
    return np.sqrt(sqdist)



from timeit import timeit
nl=10
print ("-----------------------")
print ("Timings for ",nl,"loops")
print ("-----------------------")
print("1. cdist looped amin:",timeit(lambda: dist_v1(i,j),number=nl))
print("2. cdist single amin:",timeit(lambda: dist_v2(i,j),number=nl))
print("3. KDtree ball pt:", timeit(lambda: dist_v3(n,j9),number=nl))
print("4. KDtree query:",timeit(lambda: dist_v4(i,j9),number=nl))
print("5. cdist long list:",timeit(lambda: dist_v5(i,j9),number=nl))
print("6. ball tree:",timeit(lambda: dist_v6(i,j9),number=nl))
print("7. kernel orig:", timeit(lambda: dist_v7(j, n, kernel_size,1), number=nl))
print("8. kernel optimised:", timeit(lambda: dist_v7(j, n, kernel_size,2), number=nl)) 
~~~

The output (timing in seconds) on my linux 12 core desktop (with 48GB RAM) for n=350 and 63 points:

~~~ python
no points 63
-----------------------
Timings for  10 loops
-----------------------
1. cdist looped amin: 3.2488364999881014
2. cdist single amin: 6.494611179979984
3. KDtree ball pt: 5.180531410995172
4. KDtree query: 0.9377906009904109
5. cdist long list: 3.906166430999292
6. ball tree: 3.3540162370190956
7. kernel orig: 0.7813036740117241
8. kernel optimised: 0.17046571199898608 
~~~

and for n=500 and npts=176:

~~~ python
no points 176
-----------------------
Timings for  10 loops
-----------------------
1. cdist looped amin: 16.787221198988846
2. cdist single amin: 40.97849371898337
3. KDtree ball pt: 9.926229109987617
4. KDtree query: 0.8417396580043714
5. cdist long list: 14.345821461000014
6. ball tree: 1.8792325239919592
7. kernel orig: 1.0807358759921044
8. kernel optimised: 0.5650744160229806 
~~~

So in summary I reached the following conclusions:

1.  avoid `cdist` if you have quite a large problem
2.  If your problem is not too computational-time constrained I would recommend the "`KDtree` query" approach as it is just 2 lines without the periodic boundaries and a few more with periodic boundary to set up the j9 array
3.  For maximum performance (e.g. a long integration of a model where this is required each time step as is my case) then the Kernal solution is now by far the fastest.

***

> [stackoverflow.com](https://stackoverflow.com/questions/11108869/optimizing-python-distance-calculation-while-accounting-for-periodic-boundary-co?rq=1)

You should write your `distance()` function in a way that you can vectorise the loop over the 5711 points. The following implementation accepts an array of points as either the `x0` or `x1` parameter:

~~~ python
def distance(x0, x1, dimensions):
    delta = numpy.abs(x0 - x1)
    delta = numpy.where(delta > 0.5 * dimensions, delta - dimensions, delta)
    return numpy.sqrt((delta ** 2).sum(axis=-1)) 
~~~

Example:

~~~ python
>>> dimensions = numpy.array([3.0, 4.0, 5.0])
>>> points = numpy.array([[2.7, 1.5, 4.3], [1.2, 0.3, 4.2]])
>>> distance(points, [1.5, 2.0, 2.5], dimensions)
array([ 2.22036033,  2.42280829]) 
~~~

The result is the array of distances between the points passed as second parameter to `distance()` and each point in `points`.