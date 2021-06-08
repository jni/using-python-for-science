# Understanding Python errors

When something goes wrong with Python code, Python *raises* an *exception*. The
logic behind this term is that it should be rare in good code for actual errors
to occur. ;) Of course, in the real world, Python exceptions are as common as
the air we breathe. It is the *norm* to spend most of your time programming
with code that doesn't work.

Most tutorials written by experienced programmers have perfect flow, sweeping
under the rug the endless iterations it took to get the code to even run. (This
book's authors are as guilty as any of this --- but we are now trying to
rectify things!) As a result, many beginners try to modify the code slightly to
suit their data or use case, break it in the process (this is normal!), and are
not prepared to understand how to get it working again.

In fact, the very act of programming involves a fast loop of writing, running,
*seeing what's still broken*, and rewriting. With practice, you can get much,
much faster at that third step, and thus speed up your work dramatically. In
our view, what distinguishes experienced programmers from beginners is less the
ability to get things right from the get-go, but the ability to quickly recover
from errors.

Let's see how Python represents errors, by typing in some obviously-wrong code.

```python
empty_list = []
empty_list[5]  # attempt to access the 6th element
```

Running these lines in your Python terminal will cause the following to be printed to your screen:

```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

So we see that when we try to *index* something incorrectly (this can be a
list, but also a tuple, a NumPy array, or many other custom objects that
support indexing), Python raises an IndexError. This is one of the most common
errors you'll see. Typically, it's caused because the list/tuple/etc that you
are indexing is shorter than expected, and you need to figure out why that is.

Similarly, if you try to access a *dictionary* item that doesn't exist, you get
a similar error called a KeyError:

```python
empty_dict = {}
empty_dict['hello']
```

raises:

```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'hello'
```

### A fast tour of the most common Python errors

Writing code that is not correct Python (all the above was correct Python, just with unexpected inputs) will raise a SyntaxError. For example, the program control keywords in Python (if, for, def, and others) are reserved and cannot be assigned to, since otherwise programs would not work after the point at which these keywords are redefined. Therefore, it is a SyntaxError to try to redefine these keywords:

```python
if = 5
```

raises

```pytb
  File "<stdin>", line 1
    if = 5
       ^
SyntaxError: invalid syntax
```

One particularly tricky form of SyntaxError occurs when you forget to close a
parenthesis. That's because it's valid Python to open a parenthesis on one line
and continue whatever goes inside the parenthesis along the continuing lines.
It's only when you write something that *couldn't* go in the parentheses that
you get the syntax error. So, if you get a SyntaxError and you can't for the
life of you figure out why the printed line is wrong, look up: you might have
forgotten to close a parenthesis a bit higher up.

```python
array_data = (
    (1, 0, 20),
    (0, 1, 10),
    (0, 0,  1),

def transform_data(image, tf_matrix):
    pass
```

which raises:

```pytb
  File "<stdin>", line 6
    def transform_data(image, tf_matrix):
    ^
SyntaxError: invalid syntax
```

There is nothing wrong with that transform_data definition --- but we did
forget to close the second parenthesis on the array_data definition! So again,
if you're completely baffled about why Python is giving you a SyntaxError,
don't forget to look up!

```{note}
Starting with Python 3.10, [Python will be
smarter](https://twitter.com/pyblogsal/status/1385587441323716611) in
highlighting the exact bit of code that contains the SyntaxError, so you'll be
able to forget the above advice! :tada:
```

```{note}
Modern code editors have integrated tools called "linters" that can tell you
about these errors before you even run the code. See {doc}`editors-and-ides`
and {doc}`essential-libraries-for-science` for more details about how to use
them!
```

Because Python does not do type checking ahead of running the code, it's common
for programs to run into TypeErrors, which happen when a function expects
something of a particular type, say, a number, but gets something else
entirely, such as a string, or a list. For example:

```python
import math
math.sqrt('5')
```

raises:

```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: must be real number, not str
```

A perhaps more cryptic type error occurs when a function is called with the
wrong number of arguments:

```python
def add(a, b):
    return a + b

add(5)
```

raises:

```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: add() missing 1 required positional argument: 'b'
```

That's because functions themselves have a type: `add`'s type is a *function*
taking a number, another number, and returning a third number. So, when it is
called with only a single number, we get a type error, because we gave the
wrong type of thing to the function.

Possibly the most common error you'll encounter, especially when using other
people's software, is the ValueError. This is different from the TypeError in
that the type of the thing is right, but it might be a *value* that doesn't
make sense. For example, `math.sqrt` takes in ints and floats, but it will
raise a ValueError if it receives a negative number:

```python
math.sqrt(-5)
```

raises:

```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: math domain error
```

The final very common errors are ImportError, and its more specific cousin,
ModuleNotFoundError. The latter occurs when you try to import a bit of software
that isn't in your Python installation (see the chapter on environments for
details):

```python
import package_that_doesnt_exist
```

raises:

```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'package_that_doesnt_exist'
```

By contrast, ImportError is raised when a module is found, but a specific name
in that module cannot be loaded. For example:

```python
from math import matrix
```

will raise something like:

```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: cannot import name 'matrix' from 'math' (/home/jni/miniconda3/envs/all/lib/python3.8/lib-dynload/math.cpython-38-x86_64-linux-gnu.so)
```

(Though the file path in the parentheses will be different and depend on your
operating system and Python installation.)

Speaking of things not being found: if you try to access a method on an object
or a function in a module that doesn't exist, you get an AttributeError:

```python
a_list = []
a_list.fill(5, 5)
```

(Note: lists don't have a fill method!)

raises:

```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'fill'
```

A common source of attribute errors is objects being of a type that you don't
expect. Here's an example. Suppose you are working with a file format that
contains some freeform text, followed by a table, marked by a delimiter line.
One way to handle it might be as follows:

```python
import os

def get_lines(filename):
    """Return a list of strings containing the lines in an input text file.

    Parameters
    ----------
    filename : str or pathlib.Path
        The input file.

    Returns
    -------
    lines : list of str
        The output lines. If the file does not exist, the output is None.
    """
    if os.path.exists(filename) and os.path.isfile(filename):
        with open(filename, mode='r') as fin:
            return fin.readlines()

def header_line_number(lines):
    """Find the line number of a custom table file.

    The start of the table is marked by the line '# -- header --'.

    Parameters
    ----------
    lines : list of str
        The input lines.

    Returns
    -------
    header_line : int
        The line at which the table starts.
    """
    return lines.find('# -- header --') + 1
```

Now, we can try:

```python
filename = 'filenme-with-typo-in-it.txt'
lines = get_lines(filename)
print(f'The header for {filename} is in line {header_line_number(lines)}')
```

This will raise the error:

```python
AttributeError: 'NoneType' object has no attribute 'find'
```

That's because the file was never found, so `lines` is not a list as we expect,
but None. When we try `lines.find`, we get a nasty error. It's very common for
functions to return None to indicate that they weren't able to do the requested
task, so it's very useful to be aware of this kind of error.

If you try to access a *variable* that doesn't exist (because you haven't
created it or imported it), you get a NameError:

```python
x = y + 7
```

raises:

```pytb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'y' is not defined
```

Finally, closely related to the SyntaxError is the IndentationError, which is
raised  when code is not indented correctly. Remember that indentation is
significant in Python, and indenting code when it should match the previous
line, or *not* indenting code when an indented block is expected, are both
errors:

```python
import os

filename = 'file.txt'

if not os.path.exists(filename):
print('file not found: ', filename)
```

will raise an IndentationError because you should always indent code after an
if-statement. Generally, if you have a statement that ends in a colon (`:`),
the next line should be indented by four spaces.

Similarly,

```python
file = open(filename, 'w')
    file.write('operation successful.')
```

will, in fact, not be successful, due to the incorrect indentation of the
`file.write` line:

```pytb
  File "<stdin>", line 1
    file.write('operation successful.')
    ^
IndentationError: unexpected indent
```


### Deep in the bowels of the beast: understanding tracebacks

Viewed in isolation, the above error messages seem easy and friendly. Couldn't
be easier, right? But if you've played with Python even a little bit, you'll
know that Python error messages can get a lot bigger and a lot hairier. There's
a good chance you've come across this gem:

```python
import pandas as pd
data = pd.DataFrame(
    {'name': ['Alice', 'Bob', 'Eve'],
     'faction': ['Friendly', 'Friendly', 'Adversary']}
)
all_factions = set(data['Faction'])  # notice the typo
```

which raises:

```pytb
KeyError                                  Traceback (most recent call last)
~/miniconda3/envs/all/lib/python3.8/site-packages/pandas/core/indexes/base.py in get_loc(self, key, method, tolerance)
   2645             try:
-> 2646                 return self._engine.get_loc(key)
   2647             except KeyError:

pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()

pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()

pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()

pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()

KeyError: 'Faction'

During handling of the above exception, another exception occurred:

KeyError                                  Traceback (most recent call last)
<ipython-input-8-99ed6b8a31c2> in <module>
----> 1 all_factions = set(data['Faction'])

~/miniconda3/envs/all/lib/python3.8/site-packages/pandas/core/frame.py in __getitem__(self, key)
   2798             if self.columns.nlevels > 1:
   2799                 return self._getitem_multilevel(key)
-> 2800             indexer = self.columns.get_loc(key)
   2801             if is_integer(indexer):
   2802                 indexer = [indexer]

~/miniconda3/envs/all/lib/python3.8/site-packages/pandas/core/indexes/base.py in get_loc(self, key, method, tolerance)
   2646                 return self._engine.get_loc(key)
   2647             except KeyError:
-> 2648                 return self._engine.get_loc(self._maybe_cast_indexer(key))
   2649         indexer = self.get_indexer([key], method=method, tolerance=tolerance)
   2650         if indexer.ndim > 1 or indexer.size > 1:

pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()

pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()

pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()

pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()

KeyError: 'Faction'
```

Good God! All that just to tell us that we used a column name that doesn't
exist!

That, my good friends, is called a *traceback*, and it attempts to capture all
the events that led to a particular error. In fact, all of the errors we
generated above were also tracebacks, but they just happened to be skin deep:
the error was right on the line that we typed. Tracebacks are Python's way of
surfacing errors that happen deep in your program. A less technical term might
be "breadcrumb trail".

Let's look at what happens when we build a slightly less complicated traceback
so we can describe it in more detail.

```python
def average(input_list):
    """Compute the average of a list of numbers.

    Parameters
    ----------
    input_list : list of integers or floats
        The list for which to compute the average.

    Returns
    -------
    avg : float
        The resulting average value.
    """
    added = sum(input_list)
    avg = divide_by(added, len(input_list))
    return avg


def divide_by(n1, n2):
    return n1 / n2
```

Write the above into a file called `pyavg.py`.

Now we try:

```
from pyavg import average

average([7, 6, 2]
```
```
5.0
```

So far so good. Now try:

```python
print(average([]))
```

This results in the following small traceback:

```pytb
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-11-57501266e9cc> in <module>
----> 1 average([])

~/projects/using-python-for-science/pyavg.py in average(input_list)
     13     """
     14     added = sum(input_list)
---> 15     avg = divide_by(added, len(input_list))
     16     return avg
     17

~/projects/using-python-for-science/pyavg.py in divide_by(n1, n2)
     18
     19 def divide_by(n1, n2):
---> 20     return n1 / n2

ZeroDivisionError: division by zero
```

In this case, we wrote all the code executed above so it's easy to see what
happened, laid out from top to bottom: First, we called `average([])`. The sum
of that list was computed (0), and then we called `divide_by`. Within that
function, we attempted to divide by the length of the list, which was 0,
causing the error.

You can read tracebacks top to bottom or bottom to top --- both make sense
depending on context. You can interpret the top to bottom approach as "what was
the series of events that eventually resulted in an error?" Meanwhile, the
bottom to top approach is more like: there's been an error; I understand the
detail of the error (there was a division by zero on this exact line), but what
actually caused the error to happen? What was the context? In a long traceback,
it might be easier to understand the error this way (a bit of context around
the details) than by going all the way to the beginning --- which might be very
far removed from the actual error.

Either way, the traceback contains the information you need to do the detective
work to understand why an error happened, and, probably, what you can do about
it.

Typically, somewhere along the traceback, there is an interface betweene code
you wrote and code from a library you've installed, or from the Python standard
library. In many cases, the error happens from *defined* behavior in thta
library, and you can in a sense stop looking once you get to that interface:
you cannot control that code (except through pull requests! See REFERENCE for
more!). Instead, your goal is to figure out how not to raise that error in the
future by not giving that same input to that library.

### Exercises

1. Rework the above code to remove our definition of divide_by with the built
in `from operator import divide`. Then, call the error producing code and
identify the interface where you should catch the error before attempting the
division by zero.

2. Now try to compute the average of the list `['1', '8', '9']`. What is the
error now? How would you avoid it?

3. Go back to the pandas traceback above. Try to understand the chain of events
that led to a deep KeyError inside of that library. (ie, which function called
which? Can you find those bits in the [pandas source
code](https://github.com/pandas-dev/pandas)?

4. Run the following bits of code, find and understand the error, and suggest a
fix:

a.

```python
import tempfile

import numpy as np

arr = np.random.random((5, 5))
with tempfile.NamedTemporaryFile(suffix='.txt') as f_output:
    np.savetxt(arr, f_output)
```

b.

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([4, 5])

c = np.concatenate(a, b)
```

c.

```python
from skimage import data, filters, color


image = color.rgb2gray(data.astronaut())
result = filters.gaussian(image, sigma=(1, 1, 0))
```
