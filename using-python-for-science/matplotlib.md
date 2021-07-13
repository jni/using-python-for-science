---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---


# Matplotlib

## Introduction

Matplotlib is probably the most widely used Python package for scientific
plotting. While it can be used to create animated, and interactive images, its
main strength is to provide both very quick and easy way to visualize data
from an interactive Python terminal **and** the utilities to create scientific
publication-quality figures in a wide range of format.

In this chapter, we are going create a couple of publication quality plot and
assemble them into a multi-panel figure using some of Matplotlib's advanced
utilities. 

Before diving into some practical examples, let's investigate a bit what data
visualization is... Our favorite source of information (Wikipedia…) says
"scientific visualization is the use of interactive, sensory representations,
typically visual, of abstract data to reinforce cognition, hypothesis
building, and reasoning." In science, there are two main but very different
goals to data visualization: (1) quickly investigate and understand data or
results; (2) Communicate a message efficiently. The former helps oneself gain
understanding about some new data, some new results. It is a crucial part in
making sure that your data is what you think it is [^QA_and_transcriptomic],
or that your results make sense. The approach is typically to quickly draw
some plots of some statistics to confirm or deny a hypothesis. The latter is a
means to communicate clearly a message through a representation of data. As
such, before plotting, one needs to identify precisely: (1) the message that
should be conveyed by the plot; (2) the target audience; and (3) the support
of the plot (presentation, website, scientific publication, …). 

[^QA_and_transcriptomic]: Data visualization is very widely used in genomics
  to do some quality checks on the data. Biological data, such as RNA-seq (an
  assay used to measure the abundance of thousands of gene expressions as once
  in many samples), is produced through a complicated series of steps, each of
  which can be affected by technical measurements error and human errors. For
  example, it is not uncommon for sample labels to be swapped or misannotated
  during one step of the protocol. Data visualization is thus crucial to
  ensure that samples "make sense." And this is done by using your good
  judgment: if you have a mixture of leaf and root samples, and one leaf
  sample looks a lot more like a root sample than a leaf sample, and one root
  sample looks a lot more like a leaf sample than a root sample, the two
  labels probably have been switched. If you are outraged, and think this
  abnormal and should ever happen, know that errors have been and always be a
  part of a practical implementation of large experiments!

The goal of scientific visualization can be summarized by this excellent quote
from Andrew Tufte, (1983, FIXME) "Excellence in statistical graphics consists
of complex ideas communicated with clarity, precision, and efficiency." Making
good scientific visualization is a skill that needs be learned, practiced, and
sharpened, and this chapter will not teach you this. Here, we aim at teaching
you to how to implement a plot with Matplotlib. We will do this by creating
(or more precisely, reproducing) several published scientific plots, and
assembling them in a multi-panel figure. Precisely, we will be plotting some
results from a time-course transcriptomic study of sorghum plants exposed to
drought.  

```{note} The Data: Time-course transcriptomic of droughted sorghum 
This chapter will used some data and results from a large transcriptomic
analysis of sorghum plants exposed to drought stress
{citet:varoquaux_transcriptomic}. While understanding the specific results of
the study is necessary to understand this chapter, we provide here some
background on the experiment, the data, and the analysis performed.

The overarching (very ambitious) goal of this study is to understand the
molecular response of plants to drought. The authors grew two genotypes of
sorghum plants in the field, and exposed them to two different drought
stresses: (1) pre-flowering drought stress, where no watering is applied to
the plant before flowering (roughly 8 weeks); (2) post-flowering drought
stress, where no water is provided to the plant after flowering (for roughly 8
weeks as well). Root and leaf samples were collected every week for 15 weeks,
thus resulting in a large dataset of nearly 400 transcriptomics. This data can
be loaded as an $N x p$ matrices, where $N$ is the number of genes (or mRNA
transcripts, here roughly 20,000) and $p$ the number of samples (here, roughly
400). Each sample thus corresponds to (a) a genotype (known under the sweet
names RTx430 and BTx642, which is highly unrelevant to us); (b) a sample type
(leaf or root); (c) a treatment condition (control, preflowering drought
stress, postflowering drought stress); (d) a timepoint; (e) a replicate
number.

% FIXME load part of the data, and display a nice plotly table with the
relevant metadata

In this chapter, we will visualize some results performed on analysis of
this transcriptomic dataset.
```

The goal here is to give you the tools in hand to use Matplotlib,
but we will not teach you how to do good visualization.

This chapter is divided as follows:

- Visualing a PCA 2D embedding PCA using scatter plots;
- Using stacked bar plots to visualize the effect of drought stress on
  differential expression;
- Visualizing time-series data with line plots \& error bars;
- Assembling individual plots into a multi-panel figure.

For each of the sections, we are going to start with the most basic Matplotlib
plot and enriched it step by step to obtain a publication quality figure.

### Visualizing a PCA 2D embedding using scatter plots;

Our first plot is going to be visualizing the result of a dimensionality
reduction on the transcriptomic data. The goal of this analysis is to project
the samples into a 2D dimensional space and visualize the resulting
transformation using a scatter plot.

#### The simplest plot

We are first going to load the data. It has been preprocessed such that the
data is already embedded in a 2D space

```{code-cell} python
import numpy as np
import pandas as pd

X = np.loadtxt("data/matplotlib/samples_2D_embedding.txt")
meta = pd.read_csv("data/matplotlib/data/leaf_meta.tsv", delim_whitespace=True)
```

`X` is a NumPy array of FIXME rows and 2 columns. `meta` is a Pandas dataframe
containing FIXME rows and FIXME columns.

```{code-cell} python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.scatter(X[:, 0], X[:, 1])
plt.show()
```

Matplotlib comes with a set of default parameters, that allow to customize
about everything. You can change the figure size, colors and sizes of markers,
line width, the position of the spines, the size, color, and font properties
of tick labels, the position of tick labels, … Matplotlib's default are pretty
good for a quick visualization, but for scientific publication figures, you
will want to modify many of them.

#### Changing the colors and markers

First, let us give some meaningfulness to this plot. Right now, we see that
the samples are divided into two large clusters. We are going to define
default colors and markers to some metadata properties. 

```{code-cell} python
genotype_markers = {"RT430": "o",
		    "BT642": "D"}

week_colors = {
    2: "#00FF00",
    3: "#12BC99",
    4: "#3A8DFF",
    5: "#8E84FF",
    6: "#A461EB",
    7: "#9A32CD",
    8: "#CC13E0",
    9: "black",
    10: "#FB91CA",
    11: "#FF6D73",
    12: "#FF0000",
    13: "#B90000",
    14: "#A22607",
    15: "#E79A1D",
    16: "#FFD916",
    17: "#FFFF00",
}
```

We are now going to use these dictionnaries to annotate the scatter plot.

```{code-cell} python
fig, ax = plt.subplots()

# Create a mask to identify elements from one genotype
mask_genotype = (meta["Genotype"] == "RT430").values

ax.scatter(X[mask_genotype, 0], X[mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[mask_genotype]["Week"]],
	   marker=genotype_markers["RT430"])
ax.scatter(X[~mask_genotype, 0], X[~mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[~mask_genotype]["Week"]],
	   marker=genotype_markers["BT642"])

plt.show()

```

We are starting to see a pattern in our data. The first component of the PCA
separates the two genotypes one from the other. The second component captures
the time-dependency of the data.
