---
title: "参数数据库"
weight: 3
---

无论何时想要寻找 GRIB 1 和 GRIB 2 的编码参数，参数数据库都是非常棒的工具。

ECMWF 的 GRIB 参数数据库请访问：

[The parameter database](https://apps.ecmwf.int/codes/grib/param-db)

网页如下图所示：

{{< figure src="./asset/parameter-database.png" 
    title="The parameter database" >}}

## 参数示例

以海平面气温为例说明如何使用参数数据库。

[Sea surface temperature](https://apps.ecmwf.int/codes/grib/param-db?id=34)

该页面包含参数的序号、名称、简称、单位、描述等等。

最后列出怎样在GRIB中使用该参数。

GRIB 1

Key | Value
---|---
table2Version | 128
indicatorOfParameter | 34

GRIB 2

Key | Value 
---|---
discipline | 10
parameterCategory | 3
parameterNumber | 0
typeOfFirstFixedSurface | 1

译者注：其实 GRIB 2 与 GRIB 1 的关键区别在于 GRIB 2 可以更方便地描述更多的参数类型。

## ecCodes keys - paramter

GRIB 1 和 GRIB 2 中气象参数的定义有很大的区别。
例如

GRIB 1 keys | GRIB 2 keys
------------|------------
centre | discipline
table2Version | parameterCategory
indicatorOfParameter | parameterNumber
levelType | typeOfFirstFixedSurface
level | scaleFactorOfFirstFixedSurface
...  | scaledValuesOfFirstFixedSurface
|    | typeOfSecondFirstFixedSurface
|    | scaleFactorOfSecondFixedSurface
|    | scaledValuesOfSecondFixedSurface
|    | ...