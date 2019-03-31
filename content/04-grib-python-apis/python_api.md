---
title: Python API
weight: 2
---

## ecCodes 和 GRIB API 中的 Python

从 GRIB API 1.9.5 版本开始提供 Python 接口。

支持 Python 2.5 及以上版本，不支持 Python 3。

底层，过程式接口。

提供几乎与 C 接口一一对应的函数。

使用 NumPy 模块处理数据值。

在 ECMWF 中可以使用 module 系统加载。

## 使用前准备

编译 ecCodes 时需要开启 Python 选项。

```
cmake -DENABLE_PYTHON ...
```

在 `make install` 后，Python API 相关文件会安装在如下的目录：

```
{prefix}/lib/pythonX.X/site-packages/eccodes
{prefix}/lib/pythonX.X/site-packages/gribapi
```

可以设置 PYTHONPATH 环境变量或在 Python 中链接这些文件。

注意：需要在 LD_LIBRARY_PATH 目录中添加 `{prefix}/lib`，否则载入模块会失败。

加载模块：

```py
import eccodes
# or
import gribapi
```
