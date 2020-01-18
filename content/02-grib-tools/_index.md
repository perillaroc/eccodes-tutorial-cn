---
title: "2. GRIB 工具"
weight: 2
---

## ecCodes 命令行工具

### 简介

ecCodes 工具是一系列命令行程序的集合，用于 GRIB 数据的交互处理和批处理。
为大部分通用的 GRIB 数据操作提供方便且经过测试的解决方案，同时支持 GRIB 1 和 GRIB 2 两种格式。

使用这些工具可以节省编写新代码的时间，从而提高效率。

> 考虑使用 ecCodes 工具替代自己编写的程序。

这些工具提供通用的选项集，可以很方便地将同样的参数应用到不同的工具上。

> 建议尽可能使用命令行工具。

### 通用 ecCodes 工具

获取 ecCodes 安装信息：

{{< highlight bash >}}
codes_info
{{< /highlight >}}

命令输出如下

统计 GRIB 和 BUFR 消息：

{{< highlight bash >}}
codes_count
{{< /highlight >}}

命令输出结果：

```
$ codes_count gmf.gra.2018010300000.grb2 
761
```

## GRIB 工具简介


提供如下的工具：

- 统计 GRIB 文件中的消息数
    - `grib_count`
- 查看 GRIB 文件内容和比较 GRIB 文件内容
    - `grib_ls`
    - `grib_dump`
    - `grib_get`
    - `grib_get_data`
    - `grib_compare`
- 拷贝消息
    - `grib_copy`
- 修改 GRIB 消息内容
    - `grib_set`
    - `grib_filter`
- 将 GRIB 文件转码为 netCDF 格式
    - `grib_to_netcdf`

## GRIB 工具语法

所有的工具使用通用的语法

```
grib_<tool> [options] grib_file [grib_file] ... [output_grib]
```

## 获取帮助信息

### 工具的帮助信息

每个工具都提供 UNIX man 风格的帮助信息，通过运行不带任务参数和输入文件的命令显示。

例如 grib_dump 的帮助信息如下：

```
NAME 	grib_dump

DESCRIPTION
	Dump the content of a grib file in different formats.

USAGE 
	grib_dump [options] grib_file grib_file ...

OPTIONS
	-O 	Octet mode. WMO documentation style dump.
	-D 	Debug mode.
	-d 	Print all data values.
	-j 	JSON mode (JavaScript Object Notation).
...
```

### ecCodes 文档

ecCodes 手册：

[https://software.ecmwf.int/wiki/display/ECC/ecCodes+Home](https://software.ecmwf.int/wiki/display/ECC/ecCodes+Home)

GRIB 工具文档：

[https://software.ecmwf.int/wiki/display/ECC/GRIB+tools](https://software.ecmwf.int/wiki/display/ECC/GRIB+tools)

包括如何使用工具的例子。

ecCodes 软件下载地址：

[https://software.ecmwf.int/wiki/display/ECC/Releases](https://software.ecmwf.int/wiki/display/ECC/Releases)
