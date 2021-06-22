---
title: "Essential Libraries for Science"
subtitle: ""
author: "Juan Nunez-Iglesios & Nelle Varoquaux"
jupytext:
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---


# Essential Libraries For Science

A core part of programming is having a vague understanding of what exists
out there in terms of functionality, tools, libraries, … You do not need to
know how to do a task, but that this task is possible using this or that
library. In this chapter, we provide a somewhat opiniated list of tools and
libraries that you should know about when writing code for science. We will
not teach _how_ to use those libraries, as there are plenty of
available resources for that. Instead, we only provide a short
introduction of what those libraries _do_.

We have divided the list of libraries into 5 categories: (1) core libraries,
ie libraries that you cannot code for science without; (2) libraries and tools
you should not work without; (3) visualizationg and plotting libraries; (4)
things that will help you speed up your code; (5) domain specific libraries.

## Core libraries: numpy, scipy, pandas.

About 95% of scientific Python code starts with the following lines:

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
  on how to use `numpy` (see {doc}`numpy` for details)!

- **`scipy`** is the core toolbox for scientific and technical computing for
  Python. It contains many submodules, some related to complex data structures
  (for sparse matrices for example) or operations on `ndarray` (linear
  algebra, fourier transforms, IO), others domain specific (signal processing,
  image processing, clustering). View `scipy` as the Swiss army knife of
  scientific computing in Python.

- **`pandas`** is a software library for the manipulation of data frames (an
  object to store tabular data, as you might find in Excel or relational
  databases). This differs from NumPy arrays in being 'column-oriented': the
  values in each column have to be of the same type, but one row can contain
  many different types of objects." Just as NumPy does for arrays,
  `pandas` provides fast and easy operations on dataframes, such as pivoting,
  merging, joining, data filtering, column-wise operations. We also have a
  full chapter on how to use `pandas` (see {doc}`pandas` for details)!

## "Things you cannot (or should not) work without"

- A good text editor or an IDE (See {doc}`editors-and-ides` for details)!

- `ipython` is a command shell for Python. Think `python`, but a hundred times
  better! It has syntax highlighting, auto-completion, and many magic
  functions (`%debug`, `%run`, etc) to ease the process of programming, data
  exploration, and debugging.

- `pdb`, `ipdb`, `pdb++`, and `pudb` are four python debuggers. Knowing how to use a
  Python debugger is a must! If you don't know how to use one, read
  {doc}`debugging`) without further delay!


- Whether you use an IDE or a text editor, configure it with a static linter
  to check your code as you write it. There are different options available. A
  widely used static linter is `flake8`: it combines three tools into one
  (`pyflakes`, `pycodestyle`, and `mccabe`). Another popular one is `pylint`.


## Visualization libraries

- **`Matplotlib`** is the most widely used visualization library in Python.
  While the API takes a while to get used to, there is nothing better to
  create high quality scientific figures for publication! Literally everything
  can be tweaked, tuned, and modified.

- **`seaborn`** is a thin layer on top of Matplotlib: you get the flexibility
  of `Matplotlib` with an easy-to-use interface.

- **`bokeh`**, **`plotly`**, and **`altair`** are three excellent choices of
  plotting libraries with interactive outputs. All of these play very well
  with notebooks, such as myst-nb or jupyter notebooks. Note that `plotly` can
  be used to create interactive tables as well. Because we can't resist
  showing off cool features, here's a small code snippet we shamelessly stole
  from the myst-nb documentation on creating an interactive table and plot.


Here's an interactive table with `plotly`:

```{code-cell} python
import plotly.express as px
data = px.data.iris()
data.head()
```

And now a small graph of the same data with `Altair`:
```{code-cell} python
import altair as alt
alt.Chart(data=data).mark_point().encode(
    x="sepal_width",
    y="sepal_length",
    color="species",
    size='sepal_length'
)
```


## Speed ups

Once you have a performing piece of code, it may come the time where you have
to speed your code up. Keep in mind that "Premature optimization is the root
of all evil," so don't look too closely at the list of packages in this
section unless you are sure that (1) your code works; (2) you really need to
speed things up.

- The first step of speeding up your code is to profile it: no need to spend
  time optimizating the data analysis part if 90% of the time is spend on
  loading the data. In order to do this, profile your code! There are many
  profilers out there, but our favorite ones are **`line_profiler`** and
  **`kernprof`**. `line_profiler` allows to do line-by-line profiling of
  functions, while `kernprof` is a convenient script for running a
  line-profiler (either `line_profiler` or `cProfile`, which is part of Python
  standard library). If you are interested in profiling memory, we recommend
  **`memprofiler`**. We invite you to read our chapter {doc}`profiling` if you
  are at this step of the speed-up process!

- **`cython`** is sort of a programming language, in between Python and C that
  has two purposes: (1) easily interface Python code and C code; (2) a
  Python/Cython to C compiler. The goal of `cython` is to give C-like
  performance with the ease of readability and flexibility of Python. See
  {doc}`get-it-fast` for more details!

- **`numba`** is a JIT compiler for Python, as well as a parallelization
  toolkit. It uses LLVM to generate machine code from Python code.
  Specifically designed for scientific purposes, it can yield 100x speed ups
  to a function by simply adding a decorator to it. We also talk about
  `numba` in {doc}`get-it-fast`.

- **`joblib`** provides a set of tools to create lightweight pipelining in
  Python. Precisely, it has three main features: (1) fast disk-caching
  capabilities; (2) embarassingly parallel helper functions; (3) and fast
  compressed persistence for large data. The power of this library comes from
  how lightweight it is and non-intrusive. You can use many of `joblib`'s
  feature with barely any changes to the original code.

- **`dask`** is a very powerful, parallelization libraries for data analytics.
  It integrates very nicely with existing projects, such as `numpy`, `pandas`, and
  `scikit-learn`. We won't go into much details here, as we once again have a
  full chapter on this library! See {doc}`parallel-python` for more details.

## Domain specific

The problem when it comes to domain specific libraries is that, well… They are
domain specific. As a result, only specialists of each domain know which
library to use when. Yet, unlike some communities (such as R), there are still
widely used and maintained packages for some domain. For example, the go-to
library to do astronomy data analysis in Python is `astropy`: none of us,
authors, are astronomers, yet we know that it is the go-to library (but,
please, don't ask us more about `astropy`…).

Here, we provide a list of packages, with a small description, and sometimes a
bit more detail about when to use which package.

- **`statsmodel`**: if you need to do standard statistics in Python, and
  `scipy.stats` doesn't cover it, there's a good chance it can be done with
  `statsmodel`.  It covers generalized least squares, quantile regression,
  linear mixed-effects model (no, you do not have to use R to do those!!).
- **`scikit-learn`** is the go-to libary for machine learning that isn't deep:
  SVM, regression models, random forests, cross-validation, and many metrics
  are included in this package. But if you need a p-value out of your model,
  you should check out `statsmodels` and not `scikit-learn`. If you want to do
  deep learning, then check out our next element in the list.
- **`tensorflow`** is the go-to library for doing deep-learning & neural networks
  in Python.
- **`scikit-image`** complements `scipy.ndimage` for image processing algorithms.
- **`networkx`** is meant for network analysis in Python
- **`biopython`** provides support for biological data analysis in Python
  (sequencing, alignment, population genetics algorithm, and structure
  bioinformatrics).[^DE_analysis]
- **`astropy`** is, as mentioned, a toolbox for astronomy data analysis.

[^DE_analysis]: Unfortunately, if you are looking for a tool to do
    differential expression analysis, nothing much exists and the standard is
    still to use R and bioconductor…

