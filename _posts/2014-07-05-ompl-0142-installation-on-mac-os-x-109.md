---
layout: post
title: OMPL 0.14.2 installation on Mac OS X 10.9 Mavericks
date: '2014-07-05T11:38:00.001+06:00'
tags:
modified_time: '2014-07-05T20:58:27.719+06:00'
blogger_id: tag:blogger.com,1999:blog-6020186563520251140.post-5657795903152199951
blogger_orig_url: http://o1zhas.blogspot.com/2014/07/ompl-0142-installation-on-mac-os-x-109.html
---
<!--more-->
[OMPL](http://ompl.kavrakilab.org/) is a path and motion planning library.
To install it please follow the next steps

1. Install [Homebrew](http://brew.sh/)
```bash
$ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```
2. Install build prerequisites:
```bash
$ sudo easy_install pip
$ brew install boost cmake assimp pyqt ode
$ pip install PyOpenGL PyOpenGL-accelerate
```
If you want Python bindings
```bash
$ brew tap homebrew/dupes
$ brew install apple-gcc42
$ ln -s /usr/local/bin/gcc-4.2 /usr/local/bin/llvm-gcc-4.2
$ ln -s /usr/local/bin/g++-4.2 /usr/local/bin/llvm-g++-4.2
$ brew reinstall --with-c++11 --build-from-source --with-python boost
```
If you want to install documentation as well run the following lines.
```bash
$ brew install doxygen graphviz
```
3. Download OMPL.app and unzip it.   https://bitbucket.org/ompl/ompl/downloads/omplapp-0.14.2-Source.zip
```bash
$ cd omplapp-0.14.2-Source/
$ mkdir -p build/Release
$ cd build/Release
$ cmake ../..
```
if you want Python bindings
```bash
$ make installpyplusplus && cmake .
$ make update_bindings # this option update_bindings will not be available if your boost does not support python
```
Finally to install the OMPL
```bash
$ make
$ sudo make install
```
4. Enjoy playing with path/motion planning algorithms. =)
