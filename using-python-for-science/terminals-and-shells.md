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

In {numref}`figure-terms`, you can see different terminal programs for
different operating systems running different shells.

```{figure} images/terminal-screenshots.png
---
name: terminal-screenshots
---
Terminals from different operating systems, running various shells.
```
