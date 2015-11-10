## Representing a Vector in `numpy`

It helps to begin to think of this in terms of how we might represent a function in `numpy`.
Here, we represent $\vecu$ as an evenly spaced vector from 0 to 1. 

```
In [1]: import numpy

In [2]: u = numpy.linspace(0,1,11)

In [3]: u
Out[3]: array([ 0. ,  0.1,  0.2,  0.3,  0.4,  0.5,  0.6,  0.7,  0.8,  0.9,  1. ])
```
