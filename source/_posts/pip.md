---
layout: python
title: pip
date: 2017-08-20 11:30:21
tags: python
---

# pip 摘要 #
pip is already installed if you are using python 2 >=2.7.9 or python 3 >= 3.4 binaries downloaded from python.org.
or you can install pip through [get-pip.py](https://bootstrap.pypa.io/get-pip.py).

get-pip.py will also install setuptools and wheel, if they are not available in the operating system.**setuptools** is required to install *source distributions*. Both is required to be able to build a *Wheel Cache*, although neither are required to install **pre-built wheels**.

	pip install SomePackage
	pip install Some-1.0-py2-py3-none-any.whl
	pip show --file SomePackage
	pip list --outdated
	pip install --upgrade SomePackage
	pip uninstall SomePackage

## pip install ##
1.install SomePackage and its dependencies from PyPI 
	
		pip install SomePackage # latest version
		pip install SomePackage==1.1.1 #specific ver
		pip install SomePackage>=1.11 # minimum ver

2.install a list of requirements specified in a file
		
		pip install -r requirements.txt

3.upgrade an already installed Somepackage
	
		pip install --upgrade SomePackage

4.intall a local project in "editable" mode

		pip install -e path/to/project

5.install a source archive file
	
		pip isntall ./download-tar.gz
		pip install http://my.rep/Somapack.zip

6.install from alterantivate package repositories

		pip install --index-url http://example.com/simple SomePackage
		pip install --no-index --find-links=file:///local/dir SomePackage
		pip install --no-index --find-links=relative/dir SomePackage

## pip uninstall ##
using pip to uninstall package

	pip uninstall [options] <package> # -r requirements.txt -y --yes do not ask for confirmation
## pip freeze ##
output installed packages in requirements format

		pip freeze [options] # -r --requirement <file>
## pip wheel ##
build wheel archives for your requirements and dependencies

		pip wheel --wheel-dir=/tmp/wheelhouse SomePackage

## 参考 ##

[https://pip.pypa.io/en/stable/](https://pip.pypa.io/en/stable/ "pip 文档")
<br>
