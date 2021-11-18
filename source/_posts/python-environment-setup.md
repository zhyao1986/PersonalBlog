---
title: The first step of development with Python
date: 2021-11-18 17:28:49
tags:
- guide
- environment
categories:
- Python
---

## Python Install
### Windows
1. Download install file from [Python.org](https://www.python.org/downloads/windows/)
2. Install the file
3. Set system environment with install path

### Linux
```shell
wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz
apt-get install wget build-essential libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev liblzma-dev -y
tar xzf Python-3.10.0.tgz
cd Python-3.10.0 && ./configure --enable-optimizations
sudo make altinstall
ls /usr/local/bin/python*
update-alternatives --install /usr/bin/python python /usr/local/bin/python3.10
/usr/local/bin/python3.10 -m pip install --upgrade pip
python -V && pip -V
```

## Setup Virtual Environment
```shell
python -m venv /path/to/new/virtual/environment
``` 