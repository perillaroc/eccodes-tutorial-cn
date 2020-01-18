---
title: grib_ls
weight: 1
---

`grib_ls` 用于列出 GRIB 文件的内容。

不带参数的 `grib_ls` 会按照默认列表输出 key 值。GRIB 1 和 GRIB 2 使用不同的列表。

通过选项可以指定要打印的 key 集合，或者附加到默认列表中的 key。

支持对输出进行排序，例如按照 step 的升序或降序排列。

如果没有找到某个 key，grib_ls 也不会运行失败。

grib_ls 还可以用来寻找最接近给定经纬度点的网格点，并打印（这些）点的数据值。支持获取最近的一个点或四个点。

## 用法

```
grib_ls [options] grib_file grib_file ...
```

## 示例

默认输出

```
$ grib_ls gmf.gra.2018010300000.grb2 
gmf.gra.2018010300000.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType  
2            babj         20180103     an           regular_ll   surface      0            0            acpcp        grid_jpeg   
2            babj         20180103     an           regular_ll   surface      0            0            ncpcp        grid_jpeg 
...
2            babj         20180103     an           regular_ll   surface      0            0            unknown      grid_jpeg   
2            babj         20180103     an           regular_ll   surface      0            0            unknown      grid_jpeg   
761 of 761 messages in gmf.gra.2018010300000.grb2

761 of 761 total messages in 1 files
```


使用`-p`参数指定需要打印的 key。

```
$ grib_ls -p centre:l,dataDate,shortName,paramId,typeofLevel,level gmf.gra.2018010300000.grb2 
gmf.gra.2018010300000.grb2
centre       dataDate     shortName    paramId      typeofLevel  level        
38           20180103     acpcp        3063         not_found    0           
38           20180103     ncpcp        260009       not_found    0  
...
38           20180103     unknown      0            not_found    0           
38           20180103     unknown      0            not_found    0           
761 of 761 messages in gmf.gra.2018010300000.grb2

761 of 761 total messages in 1 files
```

如果 GRIB 文件中不存在某个 key，则该 key 的值由 not found 代替

```
$ grib_ls -p my_key gmf.gra.2018010300000.grb2 
gmf.gra.2018010300000.grb2
my_key      
not_found  
not_found  
...
```

grib_ls 与 grib_get 的行为类似（后面会详细介绍）

- grib_ls 更适合交互使用
- grib_get 更适合在脚本中使用

## 使用 where 选项

所有的 GRIB 工具都支持 where 选项。

约束条件表示为 `key=value` 或 `key!=value`。

```
-w key[:{s|i|d}]=value, key[:{s|i|d}]!=value
```

只有满足所有键值对约束条件的消息才会被处理。

使用`/`分隔的数值代表`OR`条件。

```
> grib_ls –w levelType=pl file.grib1
...
> grib_ls –w step!=6,level=700/850 file.grib1
...
> grib_ls –w count=3 file.grib1
```

### 练习：使用 grib_ls

### 使用 grib_ls 寻找最近邻网格点

GRIB 场中某个经纬度坐标的最近邻点可以使用 grib_ls 找到。

```
grib_ls –l Latitude,Longitude[,MODE,file] grib_file
```

`MODE`可以使用如下数值：

- 4：打印4个最近邻点（默认）
- 1：打印1个最近邻点

`file` 指定一个 GRIB 文件作为掩码，打印最近邻的 land 点（掩码值大于等于 0.5）。

给定的 GRIB 文件必须包含格点数据。

示例：北京(纬度39.92，精度116.46)的最近邻点。

```
$ grib_ls -w count=25 -l 39.92,116.46 gmf.gra.2018010300000.grb2 
gmf.gra.2018010300000.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType         value1  value2  value3  value4 
2            babj         20180103     an           regular_ll   heightAboveGround  2            0            2t           grid_jpeg    267.665 267.695 266.975 266.435 
1 of 25 messages in gmf.gra.2018010300000.grb2

1 of 25 total messages in 1 files
Input Point: latitude=39.92  longitude=116.46
Grid Point chosen #1 index=288466 latitude=39.88 longitude=116.50 distance=6.06 (Km)
Other grid Points
- 1 - index=288466 latitude=39.88 longitude=116.50 distance=6.06 (Km)
- 2 - index=288465 latitude=39.88 longitude=116.25 distance=18.60 (Km)
- 3 - index=287026 latitude=40.12 longitude=116.50 distance=23.05 (Km)
- 4 - index=287025 latitude=40.12 longitude=116.25 distance=28.97 (Km)
```

### 练习：使用 grib_ls -l
