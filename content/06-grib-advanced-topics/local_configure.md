---
title: 本地配置
weight: 7
---

WMO 强烈建议不要在非本地使用和国际交换的消息中使用本地码表。

定义被解码引擎使用的解码规则的外部文本文件叫做定义文件（definition files）。

安装 ecCodes 会有一个默认的码表文件集合。

设置环境变量 `ECCODES_DEFINITION_PATH`，使用本地定义文件替代 ecCodes 提供的定义文件。

为给定中心 `centre` 定义的变量描述包含在 `shortName.def`，`paramId.def`，`units.def`，`name.def` 等文件中。
这些文件保存在如下目录：

```
BASE_DIR/definitions/grib1/localConcepts/[centre:s]
BASE_DIR/definitions/grib2/localConcepts/[centre:s]
```

注意：`centre:s` 是指用字符串表示的 centre，例如 ecmf, kwbc, cnmc 等等。

通用变量描述包含在 `shrotName.def`，`paramId.def`，`units.def`，`name.def` 文件中，这些定义文件保存在如下目录：

```
BASE_DIR/definitions/grib1
BASE_DIR/definitions/grib2
```

## 环境变量 `ECCODES_DEFINITION_PATH`

```
ECCODES_DEFINITION_PATH=/my/definitions:/eccodes/definitions
```

ecCodes 库会首先搜索目录 `/my/definitions` 中的每个需要的定义文件，然后搜索 `/eccodes/definitions` 目录下的文件。

如果在 `/my/definitions` 中找到定义文件，编码引擎就会使用该文件。

用户可以使用自己的定义文件覆盖所有的定义文件。

我们只建议覆盖包含变量信息的定义文件。

## 定义一个本地变量

使用 `codes_info` 获取定义文件的目录。

设置环境变量，将 local_dir 添加到目录列表中。

```
ECCODES_DEFINITION_PATH=local_dir:default_definition_dir
```

创建下面的目录，用于保存本地定义文件：

```
local_dir/grib1/localConcepts/[centre:s]
local_dir/grib2/localConcepts/[centre:s]
```

添加文件 `shortName.def`，`paramId.def`，`name.def` 和 `units.def`。

`paramId.def` 的示例（GRIB 1）

```py
# Direction of wind waves
500072 = {
    table2Version = 112; 
    indicatorOfParameter = 101;
}
```

`shortName.def` 的示例（GRIB 2）

```py
# Total precipitation of at least 10 mm 
'tpg10' = {
    table2Version = 131;
    indicatorOfParameter = 62;
}
```

## 练习

尚未实现，后续添加。
