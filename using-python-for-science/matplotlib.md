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

```{code-cell} python
:tags: ["hide-cell"]
import matplotlib as mpl
mpl.rcParams['figure.dpi'] = 500
```

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


#### Introducing some terminology.

Before we dive into making this figure publication-ready, let's first go other
some Matplotlib terminology.

- The **`Figure`** denotes the whole plot. It can be composed of several
  subplots. Each plot is holde by an `Axes`, an area where points can be
  specificed in terms of data coordinates, i.e. `(x, y)`. The `Figure` is
  typically called `fig`.

- The **`Axes`** is what people typically refer as "a plot." Matplotlib allows
  a single figure to contain many `Axes`. These can be overlapping, but
  usually are not. An `Axes` contains all the typical elements a plot has: an
  x- and y-axis (denoted by `Axis`), x- and y-labels, a title, etc. The `Axes`
  is typically called `ax`.

- The **`Axis`** (note the difference with the `Axes`) are objects that define
  the graph limits, the ticks, tick labels, … There are typically two `Axis`
  per plots/`Axes`: the x-axis and the y-axis. They can be accessed from the
  the `Axes` object called `ax` with: `ax.xaxis` and `ax.yaxis`.

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

#### Setting axes' limits \& scales

Now, we are going to set the scales of the x-axis and y-axis such that they
are identical. The two axis will thus become much more comparable to one
another.  This can be done by setting the `aspect` keyword argument of the
axes to `"equal"`. Here, we show how to set this during construction of the
subplots.

```{code-cell} python
fig, ax = plt.subplots(subplot_kw={"aspect": "equal"})

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

#### Moving the spines

For PCA plots, it's almost always nicer to move the spines so that they cross
at the (0, 0) coordinate. We will first remove the top and right spines, by
setting their linewidth to 0, then move the bottom and left splines such that
they cross at the data coordinate 0.

```{code-cell} python
fig, ax = plt.subplots(subplot_kw={"aspect": "equal"})

# Create a mask to identify elements from one genotype
mask_genotype = (meta["Genotype"] == "RT430").values

ax.scatter(X[mask_genotype, 0], X[mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[mask_genotype]["Week"]],
	   marker=genotype_markers["RT430"])
ax.scatter(X[~mask_genotype, 0], X[~mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[~mask_genotype]["Week"]],
	   marker=genotype_markers["BT642"])

# We effectively start by removing the right and top spines.
ax.spines["right"].set_linewidth(0)
ax.spines["top"].set_linewidth(0)

# Now, we set the position of the spines to be at the 0 of the data.
ax.spines["bottom"].set_position(("data", 0))
ax.spines["left"].set_position(("data", 0))

plt.show()

```

#### Setting the final size of the figure

Before diving in anything text related with Matplotlib, it's good practice to
set the figure size. Indeed, in Matplotlib, the fontsize are in points, and
not relative to the figure size. The figure size is set in inches. Scientific
publications are typically on A4 papers, while posters are often A0 or A1.
Most scientific journals require fontsize no smaller than 8pts.

| Paper	| mm	| inches | 
| --- | --- | --- |
|  A0 |	 841 x 1189 mm | 33.1 x 46.8 inches |
|  A1 |	 594 x 841 mm |	 59.4 x 84.1 cm |  23.4 x 33.1 inches |
|  A2 |	 420 x 594 mm |	 42 x 59.4 cm | 16.5 x 23.4 inches |
| A3 |	297 x 420 mm | 11.7 x 16.5 inches |
| A4 |	210 x 297 mm |	8.3 x 11.7 inches |
| A5 | 148.5 x 210 mm | 5.8 x 8.3 inches |

Let's assume this figure is for a scientific paper, and meant to be displayed
half the page (to fit into one of the the very commonly used two-column text).
Counting for a bit of margin, a figure of 3.5 inches seems thus appropriate.

```{code-cell} python
fig, ax = plt.subplots(subplot_kw={"aspect": "equal"}, figsize=(3.5, 3.5))

# Create a mask to identify elements from one genotype
mask_genotype = (meta["Genotype"] == "RT430").values

ax.scatter(X[mask_genotype, 0], X[mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[mask_genotype]["Week"]],
	   marker=genotype_markers["RT430"])
ax.scatter(X[~mask_genotype, 0], X[~mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[~mask_genotype]["Week"]],
	   marker=genotype_markers["BT642"])

# We effectively start by removing the right and top spines.
ax.spines["right"].set_linewidth(0)
ax.spines["top"].set_linewidth(0)

# Now, we set the position of the spines to be at the 0 of the data.
ax.spines["bottom"].set_position(("data", 0))
ax.spines["left"].set_position(("data", 0))

plt.show()

```

You can see that the figure is smaller than before.

#### Adding x- and y-labels

Let's now set the x-s and y-labels.

```{code-cell} python
fig, ax = plt.subplots(subplot_kw={"aspect": "equal"}, figsize=(3.5, 3.5))

# Create a mask to identify elements from one genotype
mask_genotype = (meta["Genotype"] == "RT430").values

ax.scatter(X[mask_genotype, 0], X[mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[mask_genotype]["Week"]],
	   marker=genotype_markers["RT430"])
ax.scatter(X[~mask_genotype, 0], X[~mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[~mask_genotype]["Week"]],
	   marker=genotype_markers["BT642"])

# We effectively start by removing the right and top spines.
ax.spines["right"].set_linewidth(0)
ax.spines["top"].set_linewidth(0)

# Now, we set the position of the spines to be at the 0 of the data.
ax.spines["bottom"].set_position(("data", 0))
ax.spines["left"].set_position(("data", 0))

# Let's set the x- and y-labels
ax.set_xlabel("1st component")
ax.set_ylabel("1st component")

plt.show()

```

The placement of the x- and y-labels is really not satisfying… Ideally, we
would want them aligned with the spines, but outside of the main figure. There
isn't any easy way to do this with the functions `set_xlabel` and
`set_ylabel`. Instead, we are going to do this "manually," through the `text`
function. First, we retrieve the x- and y-limits, using `get_xlim` and
`get_ylim`. Then, we compute the the coordinates of the text. Last but not
least, we need to align the text using the options `horizontalalignment` and
`verticalalignment` such that they don't overlap at all with the spines.
And voilà!


```{code-cell} python
fig, ax = plt.subplots(subplot_kw={"aspect": "equal"}, figsize=(3.5, 3.5))

# Create a mask to identify elements from one genotype
mask_genotype = (meta["Genotype"] == "RT430").values

ax.scatter(X[mask_genotype, 0], X[mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[mask_genotype]["Week"]],
	   marker=genotype_markers["RT430"])
ax.scatter(X[~mask_genotype, 0], X[~mask_genotype, 1],
	   c=[week_colors[m] for m in meta.loc[~mask_genotype]["Week"]],
	   marker=genotype_markers["BT642"])

# We effectively start by removing the right and top spines.
ax.spines["right"].set_linewidth(0)
ax.spines["top"].set_linewidth(0)

# Now, we set the position of the spines to be at the 0 of the data.
ax.spines["bottom"].set_position(("data", 0))
ax.spines["left"].set_position(("data", 0))

###############################################################################
# We can't just use the xlabel and ylabel functions here. We need to compute
# the coordinates of the text

xlim = ax.get_xlim()
ylim = ax.get_ylim()

ax.text(0, ylim[0], "1st component", horizontalalignment="center",
        verticalalignment="top", fontweight="bold", fontsize="small")
ax.text(xlim[0], 0, "2nd component", verticalalignment="center", rotation=90,
        horizontalalignment="right", fontweight="bold", fontsize=10)

plt.show()

```




#### Final figure

```{code-cell} python
from matplotlib import ticker
fig, ax = plt.subplots(subplot_kw={"aspect": "equal"}, figsize=(3.5, 3.5))

# Create a mask to identify elements from one genotype
mask_genotype = (meta["Genotype"] == "RT430").values

ax.scatter(X[mask_genotype, 0], X[mask_genotype, 1],
           c=[week_colors[m] for m in meta.loc[mask_genotype]["Week"]],
           marker=genotype_markers["RT430"])
ax.scatter(X[~mask_genotype, 0], X[~mask_genotype, 1],
           c=[week_colors[m] for m in meta.loc[~mask_genotype]["Week"]],
           marker=genotype_markers["BT642"])

# We effectively start by removing the right and top spines.
ax.spines["right"].set_linewidth(0)
ax.spines["top"].set_linewidth(0)

# Now, we set the position of the spines to be at the 0 of the data.
ax.spines["bottom"].set_position(("data", 0))
ax.spines["left"].set_position(("data", 0))

# Now set the axis labels & title
ax.set_title("Find a meaningful title", fontweight="bold")

###############################################################################
# We can't just use the xlabel and ylabel functions here. We need to compute
# the coordinates of the text

xlim = ax.get_xlim()
ylim = ax.get_ylim()

ax.text(0, ylim[0], "1st component", horizontalalignment="center",
        verticalalignment="top", fontweight="bold", fontsize="small")
ax.text(xlim[0], 0, "2nd component", verticalalignment="center", rotation=90,
        horizontalalignment="right", fontweight="bold", fontsize="small")
ax.xaxis.set_major_locator(
    ticker.MultipleLocator(40))

ax.tick_params(labelsize="x-small")

for label in ax.get_xticklabels() + ax.get_yticklabels():
    label.set_bbox(dict(facecolor='white', edgecolor='None', alpha=0.65 ))

```

