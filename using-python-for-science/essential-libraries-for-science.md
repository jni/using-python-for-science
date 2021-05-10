# Essential Libraries For Science

A core part of programming is having a vague understanding of what exists
out there in terms of functionality, tools, libraries, … You do not need to
know how to do a task, but that this task is possible using this or that
library. In this chapter, we provide a somewhat opiniated list of tools and
libraries that you should know about when writing code for science. We will
not teach how _how_ to use those libraries, but we only provide a short
introduction of what those libraries _do_.

We have divided the list of libraries into 5 categories: (1) core libraries,
ie libraries that you cannot code for science without; (2) libraries and tools
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

- **`scipy`** is the core toolbox for scientific and technical computing for
  Python. It contains many submodules, some related to complex data structures
  (for sparse matrices for example) or operations on `ndarray` (linear
  algebra, fourier transforms, IO), others domain specific (signal processing,
  image processing, clustering). View `scipy` as the Swiss army knife of
  scientific computing in Python.

- **`pandas`** is a software library for the manipulation of data frames (2D
  arrays, with homogeously typed columns and fast indexing capabilities). If
  you need an Excel equivalent in Python, use `pandas`. Similarly as `numpy`,
  `pandas` provides fast and easy operations on dataframes, such as pivoting,
  merging, joining, data filtration, column-wise operations. We also have a
  full chapter on how to use `pandas` (see XXX for details [**Editorial
  note:** this chapter has not yet been written])!

## "Things you cannot (or should not) work without"

- A good text editor or an IDE (See XXX for details [**Editorial
  note:** this chapter has not yet been written])!

- `ipython` is a command shell for Python. Think `python`, but a hundred times
  better! It has syntax highlighting, auto-completion, and many magic
  functions (`%debug`, `%run`, etc) to ease the process of programming and
  debugging.

- `pdb`, `ipdb`, `pdb++` are three python debuggers. Knowing how to use a
  Python debugger is a must! **FIXME Do we have a course on this?**

- Whether you use an IDE or a text editor, configure it with a static linter
  to check your code as you write it. There are different options available. A
  widely used static linter is `flake8`: it combines three tools into one
  (`pyflakes`, `pycodestyle`, and `mccabe`). Another popular one is `pylint`.


## Visualization libraries

- matplotlib
- seaborn
- bokeh
- plotly
- Jake's thing

## Speed ups

- profilers
- cython
- numba
- joblib
- dask

## Domain specific

- statsmodel
- scikit-learn
- scikit-image
- networkx
- biopython

