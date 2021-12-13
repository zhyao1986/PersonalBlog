---
title: pip-guide
date: 2021-12-04 20:49:52
tags:
- guide
- pip
- environment
categories:
- Python
---

## 介绍
pip是python的包管理工具，相关文档可以参考[官方文档](https://pip.pypa.io/en/stable/)

## 安装
大部分情况下，安装python包或者创建virtual environment都会自动安装好pip。
如果没有pip的话可以使用ensurepip来安装。
```shell
python -m ensurepip --upgrade
curl -O get-pip.py https://bootstrap.pypa.io/get-pip.py
python get-pip.py
```
升级pip版本
```shell
python -m pip install --upgrade pip
```

## 常用命令
1. pip list
   列出所有已安装的package
2. pip install
   安装指定package，使用-r参数可以安装requirements里指定的所有package
3. pip uninstall
   卸载指定package，使用-r参数可以卸载requirements里指定的所有package
4. pip freeze
   将所有已安装的package写到requirements文件里
5. pip config
   管理全局或者本地的pip configuration
   * list: List the active configuration (or from the file specified)
   * edit: Edit the configuration file in an editor
   * get: Get the value associated with name
   * set: Set the name=value
   * unset: Unset the value associated with name
   * debug: List the configuration files and values defined under them

## 非官方源
* 命令行
```
pip install --trusted-host [server] --extra-index-url http://[server]/simple [package name]
```
* pip.conf/pip.ini
```
[global]
index-url = https://pypi.org/simple
extra-index-url = [镜像源或者私有源]
trusted-host = 
    [服务器域名或者IP地址]
timeout = 60
```
* .netrc
```
machine [域名或者IP]
login [用户名]
password [密码]
machine [域名或者IP]
login [用户名]
password [密码]
```

## Error and Warning
### Invalid distribution
遇到下面的warning:
```shell
WARNING: Ignoring invalid distribution -ip (c:\python310\lib\site-packages)
```
解决方法：
找到warning中对应的文件夹，删掉以`~`开头的文件夹，这种类型的文件夹是插件安装失败或者中途退出导致
