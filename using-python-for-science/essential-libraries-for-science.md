# Essential Libraries For Science

A core part of programming is having a vague understanding of what exists
out there in terms of functionality, tools, libraries, … You do not need to
know how to do a task, but that this task is possible using this or that
library. In this chapter, we provide a somewhat opiniated list of tools and
libraries that you should know about when writing code for science. We will
not teach how _how_ to use those libraries, but we only provide a short
introduction of what those libraries _do_.

We have divided the list of libraries into 5 categories: (1) core libraries,
libraries that you cannot code for science without; (2) libraries and tools
you cannot (or should not) work without; (3) visualizationg & plotting
libraries; (4) things that will help you speed up your code; (5) domain
specific libraries.

## Core libraries: numpy, scipy, pandas.

About 95% of scientific Python code start with the following lines:

```python
import numpy as np
from scipy import XXX
```

- **`numpy`**'s core functionality is to provide an easy-to-use and efficient
  n-dimensional array data structure called `ndarray`. Any numerical analysis
  code in Python heavily relies on the `ndarray` data structure. The main
  difference with Python's list is that arrays are almost always homogeneously
  typed (all elements of the array are of the same type). `numpy` also
  provides fast and easy operations such as sum, element-wise product, matrix
  product, indexing, slicing, broadcasting, … Note that we have a full chapter
  on how to use `numpy` (see XXX for details [**Editorial note:** this chapter
  has not yet been written])! 

- **`scipy`**


## "Things you cannot work without": ipython, linters, debuggers


## Visualization libraries: matplotlib, seaborn, bokeh, plotly

## Speed ups: cython, numba

## Domain specific: scikit-learn, statsmodel, scikit-image, networkx

