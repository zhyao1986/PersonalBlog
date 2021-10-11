---
title: Import in Python
date: 2021-05-17 10:03:50
tags:
- python
categories:
- STARS
---

## Import语句功能
基本的 import 语句（不带 from 子句）会分两步执行:
1. 查找一个模块，如果有必要还会加载并初始化模块。
2. 在局部命名空间中为 import 语句发生位置所处的作用域定义一个或多个名称。

## How to import module
```python
import foo                 # foo imported and bound locally
import foo.bar.baz         # foo.bar.baz imported, foo bound locally
import foo.bar.baz as fbb  # foo.bar.baz imported and bound as fbb
from foo.bar import baz    # foo.bar.baz imported and bound as baz
from foo import attr       # foo imported and foo.attr bound as attr
```