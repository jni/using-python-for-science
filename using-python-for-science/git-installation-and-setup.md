# Git installation and setup

To work with this tutorial, you're going to need a few things:

- **Git**, of course. Install this by going to the git homepage,
  [git-scm.com](http://git-scm.com). On Linux, you probably already have git,
  or you can install it with `sudo apt install git-all` or
  `sudo yum install git`.
- **A graphical git client or browser**. This lets you visualise your git
  history more easily, and understand the concepts behind git better. For a
  full list of clients, see [here](http://git-scm.com/downloads/guis). On Mac,
  we recommend [Git Tower](https://www.git-tower.com), which is paid software,
  but free for students and academics. The cross-platform
  [GitKraken](https://www.gitkraken.com) works on Mac, Windows, and Linux and
  is free for local use and for use with public repositories.
- **A text editor**. We recommend Microsoft Visual Studio
  Code](https://code.visualstudio.com/). To set up code as your
  default git text editor, type
  `git config --global core.editor "code --wait --disable-extensions"` into
  your terminal. Note: programs like Microsoft Word or TextEdit are *not*
  valid text editors here because they don't produce plain text files, but
  rather more elaborate file formats that include text formatting information.
  For more on text editors, see {doc}`editors-and-ides`.
- **A GitHub account**. Create an account by going to
  [github.com](https://github.com). You can alternatively use
  [gitlab.com](https://gitlab.com), though the screenshots and exact buttons
  won't match. But the concepts and workflows are the same.
- **SSH keys to access GitHub**. Without these, you will need to type your
  GitHub password every time you try to do read from or write to your
  GitHub account. (Which will be many, many times! ;) Follow the instructions
  [here](https://help.github.com/articles/generating-ssh-keys/), making sure
  that you are seeing the instructions for your OS (Mac, Windows, or Linux).

## Notes

When typing a passphrase, it might seem that the keyboard isn't working.
However, this is just a security feature (similar to the `*`s you might see
when typing a password on the web). Just go ahead and type the passphrase,
then repeat it as requested.

**For Windows users**: Windows does not have an ssh agent running in the
background by default. If you see the error:

```console
ssh-add ~/.ssh/id_rsa
Could not open a connection to your authentication agent.
```

you will need to use this command to start the ssh-agent:

```console
eval `ssh-agent -s`
```

(Be careful to use the proper backtick symbol, usually just above the "Tab"
key on most keyboards; NOT the single quote/apostrophe character.)

Then type:

```console
ssh-add ~/.ssh/id_rsa
```

(You might need to change the filename from `id_rsa` to the whatever you used.)
See [this StackOverflow answer](http://stackoverflow.com/a/17848593) for more
info.

You need to keep the window on which you launched the ssh-agent open.

## Setup

Additionally, you'll want to set up git so that it knows your full name and
email address. Fire up a console/terminal, and type:

```console
git config --global user.name "Your Name"
git config --global user.email your.name@email.com
```

(Use the same email you used for your GitHub account.)

The following command also lets you see a rudimentary graphic of your history
without needing a GUI git client:

```console
git config --global alias.lsd "log --graph --decorate --pretty=oneline --abbrev-commit --all"
```

Then you can get a nice history *within your terminal* by typing:

```console
git lsd
```

---

Whew! That's quite a lot of stuff! But I hope by the end of the tutorial you'll
find it all useful and worth getting! (Plus: free stuff!)
