---
title: "Environment variables in a Python virtual environment on WSL"
date: 2020-03-22T12:00:00+01:00
draft: false
tags: [Python, WSL, Windows Subsystem for Linux, Virtual Environment, Environment Variables]
categories: []
---

With my python projects on a Windows computer I prefer to use virtual environments inside the Windows Subsystem for Linux (WSL). You can read [here](/posts/python-virtual-environment-wsl/) how to set-up a virtual environment inside WSL. With environment variables outside the a virtual environment you can use the `EXPORT` function on Linux and `SET` on windows. However, everytime you close the virtual environment, the environment variables will be removed.

So, to automatically use the export command with a given variable every time you activate your virtual environment add the code below to the `activate` file in your virtual environment folder. if your virtual environment folder is called _venv_ and your project is called <i>python_project</i> the file would be at the following path: <i>venv/python_project/bin/activate</i>.

```bash
# Set environment variable
environment_variable="test_environment_variable"
export environment_variable
```

Accesing environment variables from your python code can be done with the [os](https://docs.python.org/3.8/library/os.html) module using the code below.

```python
# Accessing an environment variable
import os
environment_variable = os.environ["environment_variable"]
```
