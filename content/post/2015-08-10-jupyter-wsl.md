---
date: 2016-08-10T00:00:00Z
title: Make jupyter notebook work in WSL 
url: /2016/jupyter-wsl/
---

In case you are playing around with the 
[Windows Subsystem for Linux](https://blogs.msdn.microsoft.com/wsl/) and 
[jupyter](http://jupyter.org/), you might have notice this error:

{{< highlight python >}}
Invalid argument (bundled/zeromq/src/tcp_address.cpp:171)
{{< / highlight >}}

The issue, which arises because Bash on Windows does not currently expose any 
network interfaces, has been fixed by Adam Seering in its 
[WSL](https://launchpad.net/~aseering/+archive/ubuntu/wsl) PPA.

The fix he [proposed], though, only works on when you install jupyter using 
Ubuntu repositories. In case you want to have it working with `pip`, I found
the following to be helpful:

{{< highlight python >}}
pip uninstall pyzmq
sudo add-apt-repository ppa:aseering/wsl
sudo apt-get update
sudo apt-get install libzmq3 libzmq3-dev
export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu
pip install --no-use-wheel -v pyzmq
pip install jupyter
{{< / highlight >}}
 
[proposed]: https://github.com/Microsoft/BashOnWindows/issues/185#issuecomment-225449188
