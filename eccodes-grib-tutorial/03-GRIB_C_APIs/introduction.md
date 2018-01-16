# ecCodes GRIB 编程接口

使用编程接口前首先要思考一个问题：

是否真的需要编写新的程序来解码 GRIB 或 BUFR 数据？

在修改或编写代码（Fortran，C 或 Python）前，应该先评估是否可以使用 ecCodes 提供的工具直接完成任务。

不过，有些情况下仍需要编写程序，例如：

- 在气象模式中编码和解码 GRIB 消息
- 后处理过程中编码和解码 GRIB 消息（例如 MAGICS 代码）

编写程序通常比使用工具或脚本完成同样的任务更有效率。

## 在 ECMWF 的使用情况

ecCodes 库已部署在 ECMWF 的所有平台中。

一个库可以同时处理单精度和双精度情况。在库内部，以双精度处理数据。
Fortran 90 中，ecCodes 将返回/使用定义在程序中的数据变量精度。

提供三种用户接口：Fortran（90），C 和 Python。

- Fortran 90 接口：`use eccodes`
- Python 接口：`from eccodes import *`

ECMWF 定义了两个环境变量 `ECCODES_INCLUDE` 和 `ECCODES_LIB` 简化 ecCodes 的使用。

在 ECMWF 的平台中加载 ecCodes 前需要卸载 GRIB API 库

```
$ module swap grib_api eccodes
```

编译使用 ecCodes 的程序

```
ecgate$ gfortran myprogram.f90 $ECCODES_INCLUDE $ECCODES_LIB
cca/ccb$ ftn myprogram.f90
```

更改版本

```
$ module switch eccodes/<version>
```

查看更新记录

[https://software.ecmwf.int/wiki/display/ECC/Latest+news](https://software.ecmwf.int/wiki/display/ECC/Latest+news)


