# Table of Contents

* **[Python](#python)**
  * [Timing Python](#timing-python)
  * [Advanced formatting with f-strings](#advanced-formatting-with-f-strings)
  * [classproperty](#classproperty)
  * [Arrow vs. CISO8601](#arrow-vs-ciso8601)
  * [Creating an object that supports `with` statement](#creating-an-object-that-supports-with-statement)
  * [Autoreload with iPython](#autoreload-with-ipython)
  * [Lambda Cheat Sheet](#lambda-cheat-sheet)
* **[Git](#git)**
  * [Revert a committed change](#revert-a-committed-change)
  * [Overwrite local branch with remote branch](#overwrite-local-branch-with-remote-branch)
  * [Squashing commits (before pushing)](#squashing-commits-before-pushing)
  * [Setting up git-lint](#setting-up-git-lint)
  * [Debugging .gitignore](#debugging-gitignore)
* **[Docker](#docker)**
  * [Start Docker from the command line](#start-docker-from-the-command-line)
* **[Rapid Grep](#rapid-grep)**
  * [Exclude a directory from search](#exclude-a-directory-from-search)
* **[Mac](#mac)**
  * [Install Xcode](#install-xcode)
  * [Change Interactive Shell](#change-interactive-shell)
* **[Additional Resources](#additional-resources)**

# Python
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

## classproperty
In order to create a combined [`@classmethod`+`@property` a la Django](https://docs.djangoproject.com/en/3.1/_modules/django/utils/functional/#classproperty):
```python
class classproperty:
    """
    Decorator that converts a method with a single cls argument into a property
    that can be accessed directly from the class.
    """
    def __init__(self, method=None):
        self.fget = method

    def __get__(self, instance, cls=None):
        return self.fget(cls)

    def getter(self, method):
        self.fget = method
        return self
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
`%autoreload 2` is the easiest entry and will likely get you what you need because it reloads everything before executing the Python.
```python
In [1]: %load_ext autoreload

In [2]: %autoreload 2

In [3]: mine = module.MyClass()

In [4]: mine.output()
Out[5]: 0

# ... open up my_package/directory/module.py and make a change ...

In [5]: mine.output()
Out[5]: 1

```

If you want to only reload specific modules, use `%autoreload 1`.
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
See this gist on [How to use autoreload](https://gist.github.com/jbwhit/38c1035c48cdb1714fc8d47fa163bfae) for more details.

## Lambda Cheat Sheet
Lambda functions do not require a `return` statement, but they are also anonymous, so you will usually want to assign them to a variable so you can refer to them later.

In fact, `return`, `pass`, `assert`, and `raise` are not allowed in lambda definitions.

```python
# regular definition
def add_one(x):
    return x + 1

add_one(4)
>>> 5

# lambda defintion
add_one = lambda x: x + 1
#                ^-- bound variable

add_one(5)
>>> 6
```

Using multiple arguments:

```python
full_name = lambda first, last: f'Full name: {first.title()} {last.title()}'

full_name('brad', 'pitt')
>>> 'Full name: Brad Pitt'
```

Example of an Immediately Invoked Function Expression (IIFE, pronounced "iffy"):
```python
(lambda x, y: x + y)(2, 3) = 5
```

# Git

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

# Docker

## Start Docker from the command line
```shell
$ systemctl start docker
```

# Rapid Grep

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
