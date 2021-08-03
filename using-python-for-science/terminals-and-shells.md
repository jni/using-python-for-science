# Terminals And Shells

To work with a programming language, one of the first things you will need
to become familiar with is a *terminal*. This is a program that lets you
type *text commands* to your computer and have the computer respond to those
commands — typically also with text.

When faced with their first terminal, most users are dismayed: given how
slick modern operating systems have become, why are they being forced to
relive 1980s-style text interfaces? Are programmers really such a backward
bunch?

In some sense, we are: just like any other human, most programmers learn
particular workflows and then become stuck in their ways, regardless of
whether their worflow is in some sense "optimal." But the truth is a bit
deeper: although text-based interfaces appear clunky at first glance, they
are much more *flexible* and *scalable* than graphical interfaces. It's
likely, for example, that you've followed intricate instructions to change
some settings on your phone: "Open the Settings app. Then scroll down and
click on Notifications. Finally, find Twitter, and turn the switch to Off."
Typically, this set of instructions is accompanied by screenshots, which
become outdated as soon as a new iOS release comes out. An equivalent
command on the terminal could read something like `defaults write
com.apple.notifications Twitter -bool false`. That's complicated, and you
probably couldn't ever remember the syntax off the top of your head, *but*,
when you find the instructions, doing things is as easy as copy-paste. For
flicking a switch in settings, this is not always a huge deal, though even
then navigating settings menus gets old very quickly. When you try to piece
together complex workflows, using terminal commands, which you can easily
write down, and then replicate with copying and pasting, is essential.

In addition to being more *convenient* when you want to do more than one
thing at a time, it is also more *reproducible* and less *error-prone*,
compared to clicking buttons. Again, when following tutorials for complex
software, or just helping a friend use software, you've probably had the
experience of "Ok, now click on the little 'Plus' icon on the bottom left...
No, the other left... Ok, now click in the third text box, and..." When
using a terminal, commands are precise, and can be read, rather than vaguely
described.

Hopefully those two paragraphs motivated you enough to keep on reading. All
this is not to pile on graphical user interfaces: they are wonderful! But,
when you want to dig deeper, they don't scale up, nor are they versatile
enough, to support all of the tasks that programming requires. Terminals are
not scary once you get used to them. So let's get started!

## Terminology

(Not the study of terminals!)

As mentioned above, a *terminal* is a program that lets you type in text
commands. But which commands are available to you? That is defined by the
*shell* loaded by the terminal. The shell is the *outer layer* of an
operating system (OS): the OS *kernel* is a collection of low-level programs
that directly interact with the computer hardware. As a user, you should
never have to interact with those. On top of these, small utility programs
are built that let you do things in the operating system. For example, on
Unix-like systems (including Linux and macOS), a program called `rm` removes
(deletes) files. The *shell* is one of these programs, and it has the
special ability to call the others.

Within each shell, the *command prompt* is a set of characters printed to
the screen that tells the user, "I'm ready for new commands." (As opposed to
"I'm working on something/some command is currently running.")

Having said this, you will find people on the interwebs mixing up all three
terms, including your authors until recently, when they had to finally
figure out exact definitions to write this chapter... 😅

In {numref}`figure-terms`, you can see an example of a terminal called iTerm2
on macOS running the bash shell.

```{figure} images/terminal-screenshot.png
---
name: terminal-screenshot
---
Terminals from different operating systems, running various shells.
```

On macOS, there is a built-in terminal called, um, Terminal, or you can
download other terminal applications with more features such as iTerm2.
Additionally, IDEs such as VSCode and PyCharm come with a built-in terminal
(see {doc}`editors-and-ides`). All of these terminals can access different
*shells*, such as bash, zsh, and fish. Although recent macOS versions default
to zsh, we recommend sticking to bash, as it is one of the oldest shells
(released in 1989!), and most shell commands you find on the internet are
written for bash rather than zsh or fish. You can do this by opening a Terminal
and typing the command:

```shell
chsh -s /bin/bash
```

On Windows, the built in terminal is Cmd.exe, but it is extremely primitive:
Microsoft has prioritized backwards compatibility above all else, with the
result that Cmd.exe appears stuck in 1995 (if not earlier). Thankfully,
Microsoft have released a fantastic, modern, open source terminal application
called Windows Terminal, which you can download from the Microsoft Store.

Remember that terminal *applications* and *shells* are different concepts. In
this case, Windows Terminal defaults to PowerShell, which is both a shell and a
programming language and in general horribly complicated! We instead want to
make Command Prompt the default: go to the "Startup" tab in your settings, and
select Command Prompt as your default profile.

Once you launch your terminal application, you will be greeted with your
shell's *prompt*, a sequence of characters followed by a cursor.

````{tabbed} Bash prompt
```{figure} images/bash-prompt.png
---
name: bash-prompt
---
Bash prompt on macOS Big Sur.
```
````

````{tabbed} Cmd.exe prompt
```{figure} images/cmd-prompt.png
---
name: cmd-prompt
---
Command Prompt using Cmd.exe on Windows 10
```
````

````{tabbed} Windows Terminal
```{figure} images/cmd-prompt-terminal.png
---
name: cmd-prompt-terminal
---
Command Prompt using Windows Terminal on Windows 10
```
````

Across all operating system, shells operate as a Read, Evaluate, Print Loop, or
REPL. The shell *reads* text input from the user. When the user presses
"Enter" or "Return", the shell *evaluates* the text, checks for commands, runs
them, *prints* any output, and finally *loops* back to the command prompt,
ready for the next command from the user.

It's important to know that the *evaluate* part can take a long time, and
although you can still enter text during this phase, the shell is *not*
listening to you during this time! As an example that you will surely encounter
in your Python journey, when you launch a Jupyter notebook by typing the
command `jupyter notebook`, you launch a long-running process that takes over
your terminal until you quit by pressing "Ctrl-C", which is a near-universal
command for "interrupt" across terminals and operating systems. (Yes, in
Windows and Linux, this conflicts with the "copy" shortcut. This is in fact the
reason that macOS uses "Cmd" instead of "Ctrl" for its UI shortcuts. But, on
Windows and Linux, the precendence of this shortcut means that you need to use
"Ctrl-Shift-C" and "Ctrl-Shift-V" for copy-paste into the terminal.)

## The current working directory

Commands in the command prompt are evaluated in the context of the *present
working directory*, which roughly defines where to look for any files that the
command might need. For example, after five rounds of reviews and finally
acceptance and publication, you might never want to look at that paper again!
Assuming your file is called `paper.docx`, you can delete it with the command:

````{tabbed} bash
```
rm paper.docx
```
````

````{tabbed} Command Prompt
```
del paper.docx
```
````

But for this to work, your *present working directory* needs to be the folder
in which the file is located. You can find your present working directory at
any time with the command:

````{tabbed} bash
```
pwd
```
````

````{tabbed} Command Prompt
```
cd
```
````

**This is one of the most important concepts about shells.** In fact, it is so
important that by default, command prompts will print your present working
directory with each prompt. The `pwd`/`cd` command is there to help you get
oriented, as well as to print the directory within longer scripts and other
situations where the prompt may not be visible.

And you can *change* your working directory with the command `cd` (this is one
of the rare commands that is identical across all shells). For example, when
you first launch a terminal, your working directory is typically set to your
*home* or *user* directory, which contains all your other documents and
personal folders (Documents, Downloads, Desktop, etc). So, after you launch a
terminal, you can enter the command:

```
cd Documents
```

to make Documents your working directory.

To access sub-directories, you can either repeat the command for each
subdirectory, or you can string them together with the *directory delimiter*,
which of course is different between Windows, which uses `\`, and macOS/Linux,
which use '/'. 🤦 So, for example:

````{tabbed} bash
```
cd Documents/ThatPaper
```
````

````{tabbed} Command Prompt
```
cd Documents\ThatPaper
```
````

So, so far we have discussed a couple of commands for navigating around your
file system, as well as a command for deleting files. You are set for some
spring cleaning but little else! What else can you do?

## The path

Shells have a small number of built-in commands, but other than these all they
do is offer access to small bits of software that live in various places on
your computer. For example, `rm` and `del`, which we saw in the previous section,
are small software programs. How does the shell know to offer them as commands?
And what if you have two programs named `rm` in different folders on your
computer?

The shell decides which program to run by looking at a bit of data called the
*Path*. The path is simply a list of directories, separated by `:`
(macOS/Linux) or `;` (Windows). The shell looks inside each of those folders in
order, and returns the first command it finds matching the name you typed. You
can find out where it's finding a particular command with `which` (macOS/Linux)
or `where` (Windows Command Prompt). For example, to find out where the
`rm`/`del` program is, use:

````{tabbed} bash
```
which rm
```
````

````{tabbed} Command Prompt
```
where del
```
````

To find out what your path is, you can use:

````{tabbed} bash
```
echo $PATH
```
````

````{tabbed} Command Prompt
```
set Path
```
````

As with the working directory, **the path is an extremely important concept
about the shell.** A very common error that people encounter is installing a
new utilitiy (for example, [git](https://git-scm.com)), and then having their
shell not recognize it, coming back with the error "Command not found". When
this happens to you, you now know it means one of two things:

- you mistyped the command (shockingly common!)
- the command is installed, but the *folder* where it was installed is not in
  your path.

To ensure the shell finds a command, you have two options:

1. Add a *shortcut* to the command and put it somewhere in your path
2. Add the location of the command to your path

Roughly speaking, the way tools like Anaconda and Miniconda work is by
installing Python somewhere in your home directory, and then adding that
directory to your path.

## Further reading

For a more in-depth look at using the shell, see the Software Carpentry lesson
for [the Unix shell](http://swcarpentry.github.io/shell-novice/). It only
covers bash, not the Windows command shell. However, many of the commands have
equivalents in Windows — see for example [this translation
guide](https://www.lemoda.net/windows/windows2unix/windows2unix.html): if you
are on Windows, you can thus translate the entire lesson. It's also possible to
install a bash shell on Windows (git provides one by default), but we consider
using bash on Windows to be beyond the scope of this guide.
