---
title: "Windows 下编译 ecCodes"
---

请参阅 ecCodes 官网对 windows 下编译的说明《[Microsoft Windows (experimental)](https://software.ecmwf.int/wiki/pages/viewpage.action?pageId=70949236)》。

ecCodes 在 windows/msvc 目录中提供 Visual Studio 的解决方案文件，用于编译静态库和一些命令行工具，但不支持 Fortran 和 Python。

为了使用命令行工具，必须设置全局环境变量，定位定义文件和样例文件。环境变量包括：

- ECCODES_DEFINITION_PATH
- ECCODES_SAMPLES_PATH

例如

```bash
ECCODES_DEFINITION_PATH=c:/eccodes/definitions
```

注意如果需要设置多个目录，则需要使用分号(;)分隔各个目录，而不是冒号(:)。

## 使用 JASPER 编译 ecCodes

2.X 版本的 JASPER 支持 CMake 编译，直接使用 CMake 编译 JASPER。

需要在 grib_api_lib 中设置如下的预定义变量：

```
HAVE_JPEG
USE_JPEG2000
JAS_WIN_MSVC_BUILD
HAVE_LIBJASPER
```

## 使用 openJPEG 编译 ecCodes

openJPEG 提供 windows 的预编译包，直接下载就可以使用。

需要在 grib_api_lib 中设置如下的预定义变量：

```
HAVE_JPEG
USE_JPEG2000
HAVE_LIBOPENJPEG
```

## 使用 ecCodes

> 要使用 rb 模式打开 GRIB 文件，否则解析 GRIB 消息会发生错误。
