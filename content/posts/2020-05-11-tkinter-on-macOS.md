---
date: 2020-05-11T07:00:00Z
title: Install tkinter on macOS
url: /2020/install-tkinter-macOS
---

If you work with Python on macOS and are trying to let your kids play with things like [turtle]
you will encounter errors such as

```
>>> import turtle
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "~/.pyenv/versions/3.7.4/lib/python3.7/turtle.py", line 107, in <module>
    import tkinter as TK
  File "~/.pyenv/versions/3.7.4/lib/python3.7/tkinter/__init__.py", line 36, in <module>
    import _tkinter # If this fails your Python may not be configured for Tk
ModuleNotFoundError: No module named '_tkinter'
```

If you use [pyenv] and [brew] there's a simple way to fix it:

```
brew install tcl-tk
brew install pyenv  # skip if you already have pyenv installed
export PATH="/usr/local/opt/tcl-tk/bin:$PATH"
export LDFLAGS="-L/usr/local/opt/tcl-tk/lib"
export CPPFLAGS="-I/usr/local/opt/tcl-tk/include"
export PKG_CONFIG_PATH="/usr/local/opt/tcl-tk/lib/pkgconfig"
export PYTHON_CONFIGURE_OPTS="--with-tcltk-includes='-I$(brew --prefix tcl-tk)/include' \
                              --with-tcltk-libs='-L$(brew --prefix tcl-tk)/lib -ltcl8.6 -ltk8.6'"
pyenv uninstall 3.8.2  # substitute here the version you're using or skip if you were not using pyenv
pyenv install $(pyenv install --list | grep -v - | grep -v b | tail -1)
```

After you're done, you can now turtle along:

```python
>>> from turtle import *
>>> color('yellow', 'blue')
>>> begin_fill()
>>> while True:
        forward(200)
        left(220)
        if abs(pos()) < 1:
            break
>>> end_fill()
>>> done()
```

![a turtle](/images/turtle.png)

[turtle]: https://docs.python.org/3/library/turtle.html
[pyenv]: https://github.com/pyenv/pyenv
[brew]: https://brew.sh

