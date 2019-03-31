# 信息工具

## codes_info

关于 ecCodes 的安装信息。

通用的 `codes_info` 工具提供 ecCodes 软件包使用的基本信息，包括：

- ecCodes 版本
- 定义文件的路径，`ECCODES_DEFINITION_PATH`
- 样例文件的路径，`ECCODES_SAMPLE_PATH`

命令输出如下：

```
$ ./codes_info

ecCodes Version 2.6.0

Default definition files path is used: /Users/wangdp/lib/nwpc/ecmwf/eccodes/install/share/eccodes/definitions
Definition files path can be changed setting ECCODES_DEFINITION_PATH environment variable

Default SAMPLES path is used: /Users/wangdp/lib/nwpc/ecmwf/eccodes/install/share/eccodes/samples
SAMPLES path can be changed setting ECCODES_SAMPLES_PATH environment variable
```

## grib_count

统计 GRIB 消息个数

快速统计文件列表中的 GRIB 消息个数。

语法如下（接受通配符）：

```
grib_count grib_file1 [gri_file2 ...]
```