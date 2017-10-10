---
layout: post
title: Jupyter in Google Chrome 
date: '2017-10-10T12:00:00.001+06:00'
comments: true
categories: [tips]
---

I like to use Safari for browsing (it is a default browser on my mac), but I have Google Chrome for some tasks for example google docs or jupyter.
But when you run Jupyter from Anaconda it launches the default browser in my case Safari. To run chrome instead you can do following things.
Create a directory .jupyter
```bash
$ mkdir .jupyter
```
then run your fav editor, in this example it would be nano
```bash
$ nano .jupyter/jupyter_notebook_config.py
```
and type the following python code into the file
```python
import webbrowser
webbrowser.register(u'chrome', None, webbrowser.GenericBrowser('/Applications/Google Chrome.app/Contents/MacOS/Google Chrome'))
c.NotebookApp.browser = u'chrome'
```
