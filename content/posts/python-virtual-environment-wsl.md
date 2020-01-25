---
title: "How to use a Python virtual environment on WSL"
date: 2020-01-25T12:00:00+01:00
draft: false
tags: [Python, WSL, Windows Subsystem for Linux, Virtual Environment]
categories: []
---

I wrote this down because I keep forgetting it. It might be usefull for others who want to set-up virtual environments for Python on WSL as well. The first step we want to do is update and upgrade our subsystem. Open bash and use the command below.

```bash
root@host: path# sudo apt update && upgrade
```

Once your system is up-to-date you can install the python-virtual environment.

```bash
root@host: path# apt-get install python-virtualenv
```

Windows Subsystem for Linux comes installed with two python versions installed, before we continue you need to decide which Python distribution you want to use for your virtual environment. The commands below show you the location of the two Python distributions installed.

```bash
root@host: path# which python
/usr/bin/python
root@host: path# which python3
/usr/bin/python3
```

If you know which python version you want to use you can run the code below to create a virtual environment. First we create the folder in which the environment will be installed using the ```mkdir``` command. The name of the folder will be _venv_. 

```bash
root@host: path# mkdir venv
root@host: path# virtualenv -p /usr/bin/python3 venv/python_project
```

To start the virtual environment run the code below.

```bash
root@host: path# source venv/python_project/bin/activate
(python_project) root@host: path#
```

To close the virtual environment simply run ```deactivate```.

```bash
(python_project) root@host: path# deactivate
root@host: path#
```