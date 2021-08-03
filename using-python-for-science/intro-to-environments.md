# Introduction to Python environments

Installing Python is the single most important thing about learning Python.
Yet, it is also one of the most complicated. There's nothing too intrinsically
difficult here; it's just that there's tons of options, they are all evolving,
and they can interfere with each other in weird ways. This guide is somewhat
opinionated, and will only cover one Python installation in detail (miniforge).
We will, however, try to describe the *principles* involved, so that you can
diagnose issues with other Python installations, and understand when things go
wrong.

## Why are there lots of ways to get Python?

A lot of newcomers are confused when starting out with Python, because there
isn't *just* Python --- there's Anaconda, miniconda, Python(x,y), Python.org
--- nowadays you can even get Python from the Windows App Store!

Plus, on most Linux distributions, there is a python command built-in. Is that
all you need?

All of this happens because of Python's great strength: it's open source. This
means that anyone can package their own Python and distribute it as an app, as
long as they think they can do it better, in some way, than what's out there.
(And even if they don't! You can distribute your own version of Python just for
fun!)

Let's untangle it all.

It's best to think of Python not as a program, but as *email*. Just like you
can send and receive email using a variety of programs, such as Gmail, Outlook,
or Apple Mail, so too can all these different versions of Python run Python
programs --- but each with different features, advantages, disadvantages.

And, just like you can have many different email programs installed on your
phone or on your computer, so too can you have lots of different Python
versions on your computer. Probably 95% of installation problems
have to do with people not knowing which Python they are using out of the
multiple versions on their computer.

To help you preempt these issues, here we show you the single most important diagnostic when
troubleshooting your Python installation problems. In your Python program,
whether that program is commands entered into the Python prompt, a .py file, or
a Jupyter notebook, type:

```python
import sys
print(sys.executable)
print(sys.path)
```

The output will look something like this:

```python
>>> import sys
>>> print(sys.executable)
/home/jni/miniconda3/envs/upfs/bin/python
>>> print(sys.path)
['', '/home/jni/miniconda3/envs/upfs/lib/python39.zip', '/home/jni/miniconda3/envs/upfs/lib/python3.9', '/home/jni/miniconda3/envs/upfs/lib/python3.9/lib-dynload', '/home/jni/miniconda3/envs/upfs/lib/python3.9/site-packages']
```

The first print function call prints the location of the *interpreter* running
your code --- in the email analogy, this is the same as the email program with
which you are reading or sending email. Most usually, when you install a Python
package but can't access it, it's because you installed it for a different
Python interpreter.

Similarly, `sys.path` tells you the folders within your computer where Python
is looking for programs or libraries to import. If you install a library with
conda or pip, it *will* end up in one of these folders. If it's not there, or
it's a different version from the one you expect, it usually means, again, that
you've installed it into a different Python installation.

Together, these two variables make up an *environment*. (Well, to a first
approximation; in fact, other information is needed, such as where to look for
*C* libraries that Python depends on. But in most cases, you only need to care
about which interpreter and which Python libraries you're using.)

## Managing environments

Hopefully, the above ideas are enough to keep you oriented in the face of
different versions of Python and different Python environments within your
machine. When in doubt, don't panic and print `sys.executable`! Now that you
have a compass, though, let's see how you can create and navigate environments.
Once you can create, modify, and destroy environments at will, you'll be able
to marshall the entire Scientific Python ecosystem!

In Python, there are two main tools to manage environments: virtualenv, and
conda. Various other tools, such as pipenv and mamba, give you options to
manipulate either of those two types of environments. The concepts for all
these tools are the same, so we will focus on conda and leave you, brave
reader, to figure out the other tools when necessary, armed with the concepts
from this chapter. Indeed, a good exercise when you are done with this chapter
is to uninstall everything, then start from scratch with mamba.

The first thing to do is to get conda. Confusingly, even now we have choices to
make: we can either install miniconda or Anaconda. Miniconda is *just* conda,
and what we will install going forward. Anaconda includes miniconda *as well
as* a whole bunch of Scientific Python packages hand-compiled by the engineers
at Anaconda, Inc, all neatly bundled into a single, massive environment.
Although this can be convenient, in our case we want to create, modify, and
destroy environments at will, so we will skip the One Huge Environment To Rule
Them All.

In fact, we will not download miniconda itself, but rather *miniforge*, a
different version
of miniconda that searches for packages in the *conda-forge* open repository,
rather than the main Anaconda channel, which is proprietary.
As of this writing, you can download miniforge at
https://github.com/conda-forge/miniforge#download, but if that changes, simply
search the web for "download miniforge". Follow the installation instructions
on the site, and launch a command line terminal. This will be different
depending on your operating
system. On Mac and Linux, you will have an application called Terminal. (Though
on Mac we recommend you download and install the excellent **iTerm**, which
fulfills the same function, but better.) On Windows, the picture is more
complicated, so much so that we have given it its own chapter. See "Navigating
Windows Terminals" for details [**Editorial note:** this chapter has not yet
been written.]. We recommend using Anaconda Prompt in this chapter.

If you've correctly installed miniconda, you shoud be able to launch a terminal
and type:

```
conda --help
```

and get a message about conda usage.

### The Prime Directive of conda environments

In a rather meta turn, conda itself lives in its own little conda environment,
called `base`. In what I consider to be a grave mistake, conda allows you to
install other packages into this environment, and generally wreak all sorts of
havoc with it.

I instead recommend that you treat this environment as untouchable, and always
work in other environments. To reiterate: **never install packages into the
base conda environment!** All other environments are disposable, but the base
one is harder to replace, so you should **only** change it to update conda
itself, using `conda update --name base conda`.

### Your first environment

Ok, with that out of the way, let's create our first conda environment. We do
this with the `conda create` command. Let's make a Python 3.9 environment:

```
conda create -n py39 python=3.9
```

Once that's done, you can *activate* the environment, which means setting that
specific installation of Python as the one that subsequent commands will use.
This setting will be active for that specific terminal session until it's
closed or you `conda deactivate`, or you `conda activate` a different
environment. Do this with:

```
conda activate py39
```

```{note}
if the above command fails with an error message similar to "conda
command not found", you need to set up your terminal to correctly find conda.
See the "terminals and shells" chapter for more.
```

### Exercise: switching environments

You now have at least two environments: Your base environment, and the py39
environment. Try to get comfortable with switching between them, and
understanding how they cause you to invoke different versions of Python. Try
typing in the following commands one by one:

```
conda deactivate
which python  # or "where python" in some Windows shells
python -c "import sys; print(sys.executable)"
conda activate py39
which python
python -c "import sys; print(sys.executable)"
which pip
pip install numpy
python -c "import numpy; print(numpy)"
conda deactivate
python -c "import numpy; print(numpy)"
```

The last command should crash with an import error, because we have not
installed NumPy in the base environment, only in the py39 environment.

You now have a Python 3.9 environment, and you have NumPy installed within it,
and you can run Python programs that use NumPy within it.

Now, let's try to create some other environments, and run Python in different
ways. Try the following commands:

```
conda create -y -n jupy38 python=3.8
conda activate jupy38
pip install jupyter notebook
jupyter notebook
```

The last command should open a browser to the launch page of Jupyter Notebook.

[**Editorial note:** insert screenshot]

Try creating a new notebook. Will you be able to import numpy within it? If you
type the following code into a cell and run it, what is the output?

```python
import sys
print(sys.version_info)
print(sys.executable)
```

## Working with IDEs

Integrated Development Environments, or IDEs, are text editors that come with
additional features to help you quickly evaluate and run your code. Examples of
IDEs include Visual Studio Code, PyCharm, and Spyder.

In order to help you run your code, IDEs must use a Python executable. But
we've just seen that you can have many executables on your computer, and that
they won't all have the same libraries and features installed. In fact, without
knowing which Python executable you are working with, the IDEs can't tell you
whether your code is correct, because different Python versions have different
features. The code `print(f'{5+8}')` is valid in Python 3.6 or later, but won't
work in Python 3.5 or earlier.

The solution, as you might expect, is to tell the IDE which actual Python
interpreter you want to work with. All IDEs have a facility to do this. The
purpose of this section is just to tell you about it, so you can code more
effectively with IDEs!

### Exercise: using the correct interpreter

Download and install Visual Studio Code, an open source IDE, and install the
Python extension. (It will recommend it to you on first launch.)

Start a new project with Visual Studio Code. Go to the settings, and set the
interpreter to be the Python returned by `sys.executable` in the jupyter
exercise.

Now, create a new file in your project, and start with:

```python
import numpy as np
```

VSCode should complain that you do not have pylint installed. pylint is a
linter, a computer program that analyses other computer programs to
check them for errors. When you ask VSCode to install it, it will then
highlight the `import numpy` line, because we did not install NumPy in the
jupy38 environment! This is the power of linters: They can tell us our programs
are wrong even befdore we run them. Working with a linter can make you much
more productive.

At this point, we have two options: we can either install NumPy in the jupy38
environment (and indeed VSCode will suggest this), or we can change the
interpreter to the py39 environment, which does have NumPy. (Though we will
then need to install pylint again.) We leave the choice to you.

### The nuclear option

We warned earlier that it's dangerous to install packages to your system
Python. Why wouldn't it be dangerous in an environment? The answer is, it is,
but environments are *disposable*, and you should see them as such. Once you
do, dealing with environments becomes very freeing, as opposed to another
complicated step to get Python to work.

Installing new libraries to an environment can often lead to strange failures,
because libraries often depend on other libraries in version-specific ways, and
they don't always correctly declare this version specificity. For example,
scikit-image 0.14 depended on NumPy 1.10 and higher, but when NumPy 1.15 was
released, it inadvertently broke some functionality in scikit-image. Therefore,
if you installed scikit-image to an environment before NumPy 1.15 was released,
and then subsequently updated NumPy, you would have ended
up with a broken environment for image analysis. Even if you don't consciously
update packages, you might decide to install some other package that depends on
NumPy 1.15 or higher. Since scikit-image did not know ahead of time that a
later NumPy release would break it (though in this particular instance they
should have, because they were using a private NumPy function), there was
nothing to prevent this from happening --- package dependency metadata is fixed
at release time.

Sometimes, these failures are easy to detect and fix. Sometimes, it is very
difficult or impossible. In many cases, the failures are caused by a strange
convergence of events, caused by the exact order and versions of packages
installed.

This works to our advantage. When an environment breaks, and we can't readily
fix it, just nuke it and start again! Let's try this out:

```
conda create -y -n borked python=3.7
conda activate borked
pip install scikit-image==0.14.0 numpy==1.14.3
```

Now, check that this works:

```
python -c "from skimage import util"
```

Now, suppose you want to visualize some 3D image data in your project, so you
now want to install [napari](https://napari.org). Let's try it:

```
pip install napari[qt]
```

Notice how it upgraded NumPy, but not scikit-image. Now, we try the same code:

```
python -c "from skimage import util"
```

Boom. Our environment broke. Now, in this very specific case, upgrading
scikit-image would fix things. But things aren't always that easy, or obvious.
So, an easy way to proceed is to start again!

### Exercise: nuke, rinse, repeat

Delete the `borked` environment, and create a new one with current versions of
NumPy, scikit-image, and napari.

## Conclusion

That's it! We hope this chapter has made you a bit more confident to try out
different versions of Python, different library versions, everything! The
fastest way to learn is to try lots of things, so by making it easier for you
to try things, environments are a key to help you learn Python quickly.

If you've gone through this whole chapter and are still feeling tentative,
remember the nuclear option, and try again! As a final exercise, why don't you
try:

- remove all your conda environments, and try to repeat this whole chapter
  using mambaforge, a from-scratch implementation of miniforge.
- replace all the pip install commands with mamba install commands
- repeat the chapter using Python.org to download Python, and virtualenv to
  create environments
- if you have access to another computer, try everything again on that other
  computer.

Once you have messed with environments enough times, all of this becomes easier
to deal with. And remember: even the experts regularly end up with broken
environments and have to start from scratch! If your environment is broken,
don't panic, conda env remove, and carry on!
