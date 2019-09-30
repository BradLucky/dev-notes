# Table of Contents

* **[Python Notes](#python-notes)**
  * [Timing Python](#timing-python)
  * [Advanced formatting with f-strings](#advanced-formatting-with-f-strings)
  * [Arrow vs. CISO8601](#arrow-vs-ciso8601)
  * [Creating an object that supports `with` statement](#creating-an-object-that-supports-with-statement)
* **[Git Notes](#git-notes)**
  * [Revert a committed change](#revert-a-committed-change)
  * [Squashing commits (before pushing)](#squashing-commits-before-pushing)
  * [Setting up git-lint](#setting-up-git-lint)
  * [Debugging .gitignore](#debugging-gitignore)
* **[Rapid Grep Notes](#rapid-grep-notes)**
  * [Exclude a directory from search](#exclude-a-directory-from-search)
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

# Git Notes

## Revert a committed change
Use this with caution. It will erase all history of commits after the hash you use.
```bash
$ git reset --hard <hash>
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

1. Delete git-lit cache
2. Remove the `REPO_HOME` line

## Debugging .gitignore
https://blog.kennard.dev/2019-06-19-git-debug-gitignore/

# Rapid Grep Notes

## Exclude a directory from search

```bash
$ rg template -g '!frontend/*'
```

# Additional Resources
1. [Python Tips and Trick, You Haven't Already Seen](https://martinheinz.dev/blog/1)
