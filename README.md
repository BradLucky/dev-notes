# Table of Contents

* **[Python Notes](#python-notes)**
  * [Timing Python](#timing-python)
  * [Advanced formatting with f-strings](#advanced-formatting-with-f-strings)
  * [Arrow vs. CISO8601](#arrow-vs-ciso8601)
  * [Creating an object that supports `with` statement](#creating-an-object-that-supports-with-statement)
  * [Autoreload with iPython](#autoreload-with-ipython)
* **[Git Notes](#git-notes)**
  * [Revert a committed change](#revert-a-committed-change)
  * [Overwrite local branch with remote branch](#overwrite-local-branch-with-remote-branch)
  * [Squashing commits (before pushing)](#squashing-commits-before-pushing)
  * [Setting up git-lint](#setting-up-git-lint)
  * [Debugging .gitignore](#debugging-gitignore)
* **[Rapid Grep Notes](#rapid-grep-notes)**
  * [Exclude a directory from search](#exclude-a-directory-from-search)
* **[Mac](#mac)**
  * [Install Xcode](#install-xcode)
  * [Change Interactive Shell](#change-interactive-shell)
* **[Additional Resources](#additional-resources)**

# Python Notes
A cheat sheet for less-used Python tools and methods

## Timing Python
```python
import timeit

timeit.timeit("""
    name = 'Brad'
    age = 90
    f'{name} is {age} years old.'
""")

>>> 0.15161066000291612

timeit.timeit("""
    name = 'Brad'
    age = 90
    '{} is {} years old'.format(name, age)
""")

>>> 0.3390180559945293
```

## Advanced formatting with f-strings
```python
name = 'Brad'

f'{name.center(10, "*"):~^20}'
>>> '~~~~~***Brad***~~~~~'

f'{name.center(10, "*"):~>20}'
>>> '~~~~~~~~~~***Brad***'

f'{name.center(10, "*"):~<20}'
>>> '***Brad***~~~~~~~~~~'

line_width = 24
f'{name.center(len(name) + 2, " "):~^{line_width}}'  # use another set of curly braces
>>> '~~~~~~~~~ Brad ~~~~~~~~~'
```

## Arrow vs. CISO8601
For reference CISO8601 is considerably faster, but...
```python
ciso8601.parse_datetime(<DateTime>) == arrow.get(<DateTime>).datetime
```

## Creating an object that supports `with` statement
```python
from contextlib import contextmanager

@contextmanager
def tag(name):
    print(f'<{name}>')
    yield
    print(f'</{name}>')
    
with tag('title'):
    print('This is the title')
```
```html
<title>
This is the title
</title>
```

## Autoreload with iPython
```bash
$ ipython
```
```python
In [1]: %load_ext autoreload

In [2]: %autoreload 1

In [3]: %aimport my_package.directory.module

In [4]: mine = module.MyClass()

In [5]: mine.output()
Out[5]: 0

# ... open up my_package/directory/module.py and make a change ...

In [6]: mine.output()
Out[6]: 1
```
See this gist on [How to user autoreload](https://gist.github.com/jbwhit/38c1035c48cdb1714fc8d47fa163bfae) for more details.

# Git Notes

## Revert a committed change
Use this with caution. It will erase all history of commits after the hash you use.
```bash
$ git reset --hard <hash>
```

## Overwrite local branch with remote branch
Use this with caution, obviously.
```bash
# git reset --hard <remote_branch>
$ git reset --hard origin/development
```

## Squashing commits (before pushing)
```bash
$ git rebase -i development  # branch name
```
In the editor that comes up, change "most" lines to `squash` and save.

Update the commit message as desired.
```bash
$ git push --force
```

## Setting up git-lint
1. Delete git-lint cache
2. Remove the `REPO_HOME` line

## Debugging .gitignore
https://blog.kennard.dev/2019-06-19-git-debug-gitignore/

# Rapid Grep Notes

## Exclude a directory from search
```bash
$ rg template -g '!frontend/*'
```

# Mac

## Install Xcode
Every now and then (especially when upgrading MacOS), ugly errors will arise on the command line. For instance:
```bash
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```
To solve this, (re)install Xcode:
```bash
$ xcode-select --install
```

## Change Interactive Shell
The default since Catalina is zsh. To decide which you want:
```bash
$ chsh -s /bin/bash
# ... or ...
$ chsh -s /bin/zsh
# more options can be found in /bin/
```

# Additional Resources
1. [Python Tips and Trick, You Haven't Already Seen](https://martinheinz.dev/blog/1)
2. [PostgreSQL JSON cheatsheet](https://devhints.io/postgresql-json)
