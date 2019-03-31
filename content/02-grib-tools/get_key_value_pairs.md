---
title: 获取键值对
weight: 4
---

## grib_get

> 获取键值对

使用`grib_get`从一个或多个文件中获取一个或多个key值，与`grib_ls`类似。

默认情况下，如果有错误发生（例如没有找到key），`grib_get`则会运行失败，返回非0退出值。

- 适合在脚本中获取 GRIB 消息的键值。
- 可以强制`grib_get`在出错时不会运行失败。

支持获取所有的 MARS key 和特定名称空间中的所有 key。可以添加其他 key 到默认集合。

浮点类型数据的输出支持 C 风格的格式化描述。

可以用于寻找某个经纬度坐标点的最近邻点，与`grib_ls`工作方式相同。

## 用法

```
grib_get [options] grib_file grib_file ...
```

## 选项

选项 | 含义
---|---
-p key1,key2,... | Keys to get
-P key1,key2,... | Additional keys to get with –m, -n
-w key1=val1,key2!=val2,... | Where option
-s key1=val1,key2=val2,... | Keys to set (temporary for printing) 
-m | Get all MARS keys
-n namespace | Get all keys for namespace 
-l lat,lon[,MODE,FILE] | Value(s) nearest to lat-lon point 
-F format | Format for floating point values
-f | Do not fail on error
... | ...


## 使用 where 选项

where 选项 -w 可以用于所有的 GRIB 工具，具有相同的格式。

约束条件可以表示为如下几种形式：

- `key=value`
- `key!=value`
- `key=value1|value2|value3`

示例

```
-w key1=value1,key2:i!=value2,key3:s=value3
```

只有满足所有约束条件的 GRIB 消息才会被处理

```
# IS
> grib_get–w level=100–p step file.grib1
…

# NOT
> grib_get–w level!=100–p step file.grib1“NOT”
…

# AND
> grib_get–w level=100,step=3 –p step file.grib1
…

# OR
> grib_get–w level=100/200/300 –p step file.grib1
…

## 指定 key 的类型

ecCodes 的所有 key 都有默认类型，例如字符型（string），整型（integer），浮点型（float point）。

key 的类型可以用下面的方式指定：

- `key` -> 原始类型
- `key:i` -> 整形
- `key:s` -> 字符型
- `key:d` -> 双精度浮点型


```
$ grib_get -w count=1 -p centre:i,dataDate,shortName,paramId,typeOfLevel,level ne_gmf.gra.2017050100000.grb2
38 20170501 t 130 surface 0
```

### 示例

获取文件中第一个 GRIB 消息的 centre（使用字符型和整形）

```powershell
$ grib_get -w count=1 -p centre ne_gmf.gra.2017050100000.grb2
babj

$ grib_get -w count=1 -p centre:i ne_gmf.gra.2017050100000.grb2
38
```

如果有错误，`grib_get`会出错退出。

```powershell
$ grib_get -w count=1 -p mykey ne_gmf.gra.2017050100000.grb2
ECCODES ERROR   :  Key/value not found

$ echo $?
False
```

获取所有的 MARS key，附加打印 shortName

```
$ grib_get -w count=1 -m ne_gmf.gra.2017050100000.grb2
20170501 0000 0 sfc 130

$ grib_get -w count=1 -m -P shortName ne_gmf.gra.2017050100000.grb2
t 20170501 0000 0 sfc 130
```

`grib_get -m`是`grib_get -n mars`的简写。

获取所有属于 statistics 命名空间的 key

```
$.\grib_get -w count=1 -n statistics ne_gmf.gra.2017050100000.grb2
317.949 250.369 284.773 15.9011 -0.0924059 -1.26255 0
```


### 设置输出格式

浮点数的输出格式可以通过 -F 选项使用 C 风格的输出语法设置。

- `-F "%.4f"` - 小数格式，带4位小数（1.2345）
- `-F "%.4e"` - 指数格式，带4位小数（1.2345E-03）

```
$ grib_get -w count=1 -p maximum -F "%.6f" ne_gmf.gra.2017050100000.grb2
317.949199

$ grib_get -w count=1 -p maximum -F "%.4e" ne_gmf.gra.2017050100000.grb2
3.1795e+02
````

默认格式是 `-F ".10e"`。

### stepRange 和 stepUnits

step 总是使用整型打印。

默认打印的 step 单位是小时。

获取其他单位的 `step`，需要使用`-s`选项设置 `stepUnits`。

```
$ grib_get -w count=1 -p stepRange ne_gmf.gra.2017050100012.grb2
0-12

$ grib_get -w count=1 -p stepRange -s stepUnits=m ne_gmf.gra.2017050100012.grb2
0-720
```

stepUnits 可以设置为 `s, m, h, 3h, 6h, 12h, D, M, Y, 10Y, 30Y, C`

### 使用`grib_get`寻找最近邻点

GRIB 场中某个经纬度坐标点的最近邻点可以用`grib_get`找到，与`grib_ls`工作方式相同。

寻找北京1000百帕的温度：

```
$ grib_get -w count=51 -l 39.92,116.46 ne_gmf.gra.2017050100012.grb2
295.684 294.634 294.064 295.044

$ grib_get -w count=51 -F "%.5f" -P stepRange -l 39.92,116.46 ne_gmf.gra.2017050100012.grb2
12 295.68381 294.63381 294.06381 295.04381
```

选择 GRIB 文件必须包含格点数据。

### 获取某个格点的数据值

输出 GRIB 要素场中特定个点的数值可以使用`grib_get`的`-i`选项。

例如，使用`grib_ls`寻找最近邻的 index，再使用`grib_get`获取该点的一系列值。

```
$ grib_get -i 2159 -p shortName,dummy:s ne_gmf.gra.2017050100012.grb2
acpcp 10
ncpcp 10.00806427
```

`dummy:s`用于在 shortName 和 value 间强制输出一个空格。

非格点数据也会返回值。
