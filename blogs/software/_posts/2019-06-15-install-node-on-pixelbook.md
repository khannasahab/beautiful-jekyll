---
layout: post
title: Installing node on Pixelbook
categories: [software]
tags: [software, engineering]
share_request: true
subscribe_email: true
---

If you are a Pixelbook owner, you must be aware that Google is putting a lot of efforts in making it the developer machine. I recently tried installing `node` on my Pixelbook and honestly it was not a smooth experience. On mac, it it was a matter of running one command and on Pixelbook, it was a struggle for sometime.

There are many articles on web helping you how to install linux apps on chromebook. A simple Google search will do the trick. However I was getting a simple error and it took me sometime to figure out the solution.

> I followed some article and it concluded to install node with nvm.

[You can follow this](https://github.com/nvm-sh/nvm)
and you will try something like this:

`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`

The error I got was:

`libssl.so.1.0.0: cannot open shared object file`

Or something similar for another lib.

## Solution
I had to do two things to solve the problem

1. Add lib and lib64 in path (LD_LIBRARY_PATH)
2. Install libsssl.so.1.0.0

### Update the LD_LIBRARY_PATH
```
sudo vi /etc/bash.bashrc
append at last:
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib:/usr/local/lib64
save and exit
```
### Install  libsssl.so.1.0.0
I did google search "Download libsssl.so.1.0.0" and found a deb file
`https://packages.debian.org/jessie/amd64/libssl1.0.0-dbg/download`

Download, Right Click on the file and run with Linux. It solved all of my issues.

Now you can install nvm with
`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`


Hope, it helps someone.
