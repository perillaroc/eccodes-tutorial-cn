# 设置键值对

## grib_set

> 设置键值对

`grib_set` 工具可用于：

- 设置输入 GRIB 文件中的键值对；
- 对输入 GRIB 文件中的键值对做简单修改。

每个 GRIB 消息都会写入到输出文件中。

- 默认包括那些没有修改键值对的消息。
- 使用 `-s` 选项（strict）仅拷贝哪些满足 where 选项中所有约束条件的消息。

可以设置选项重新打包数据。有时在修改与打包算法相关的属性键值后，需要重新打包数据。

`grib_set` 在遇到错误时会异常退出。例如找不到某个 key 时。

### 用法

```
grib_set[options] grib_filegrib_file… out_grib_file
```

选项

选项 | 含义
---|---
-s key1=val1,key2=val2,… | 需要修改的键值对列表
-p key1,key2,,… | 打印的 key 列表，与 `-v` 选项一同使用
-w key1=val1,key2!=val2,… | Where 选项
-d value | 设置所有的数据值为 value
-f | 出错后不退出
-v | 显示详细信息
-S | 严格模式
-r | 重新打包数据
... | ...

### 示例

将所有变量的名称修改为 10 米风速，简称 10si。

```
$ grib_set -s shortName=10si out_10u.grb2 out_10si.grb2
$ grib_ls out_10si.grb2 
out_10si.grb2
edition      centre       date         dataType     gridType     stepRange    typeOfLevel  level        shortName    packingType  
2            babj         20180103     an           regular_ll   0            heightAboveGround  10           10si         grid_jpeg   
1 of 1 messages in out_10si.grb2

1 of 1 total messages in 1 files
```

上述操作将修改多项键值，例如：

- `shortName` 设为 10si
- `paramId` 设为 207
- `name`/`parameterName` 设为 ‘10 米风速’
- `units`/`parameterUnits` 设为 `m s ** -1`
- `indicatorOfParameter` 设为 207，与 GRIB 版本无关
- `marsParam` 设为 207.128

某些 key 是只读的，不能直接修改。

```
$ grib_set -s name="10 meter wind speed" out_10u.grb2 out_10si.grb2
ECCODES ERROR   :  concept: no match for name=10 meter wind speed
ECCODES ERROR   :  grib_set_values[0] name (3) failed: Concept no match
```

只读 key 可以通过设置其它 key 来修改，例如

- `shortName=10si`
- `paramId=207`
- `indicatorOfParameter=207`，GRIB 版本无关

### 设置 key 为缺失值

如果某个 key 不被使用，则该键值所有位都应该设为 1，表示该值是缺失值。

不同的 key 有不同的长度，所以需要编码的缺失值不完全相同。

为了将某个 key 设为缺失值，`grib_set` 接受字符串 `missing` 或 `MISSING`。

```
$ grib_set -s Ni=missing out_10u.grb2 out.grb2
$ grib_dump -O out.grb2
***** FILE: out.grb2 
#==============   MESSAGE 1 ( length=1138347 )             ==============
...
======================   SECTION_3 ( length=72, padding=0 )    ======================
1-4       section3Length = 72
...
31-34     Ni = MISSING
...
```

注意某些值无法被设为缺失值。

```
$ grib_set -s dataDate=missing out_10u.grb2 out.grb2
ECCODES ERROR   :  unable to set dataDate=missing (Value cannot be missing)
ECCODES ERROR   :  grib_set_values[0] dataDate (7) failed: Value cannot be missing
```

### 修改小数精度

为了使用1位小数表示温度，我们可以设置 `changeDecimalPrecision=1`

注意：这与设置有效位数不一样。

```
$ grib_set -s changeDecimalPrecision=1 t.grb2 t2.grb2
```

使用 `grib_compare` 比较数据。

```
$ grib_compare -c data:n t.grb2 t2.grb2 

-- GRIB #1 -- shortName=t paramId=130 stepRange=0 levelType=sfc level=0 packingType=grid_jpeg gridType=regular_ll --
double [packedValues]: 1036800 out of 1036800 different
 max absolute diff. = 4.9997558593815938e-02, relative diff. = 0.000162694
	max diff. element 462366: 3.07260292968749979536e+02 3.07310290527343795475e+02
	tolerance=0.0000000000000000e+00
	values max= [313.09]  [313.11]         min= [202.71] [202.71]
```

### 修改数据值

可以使用 `offsetValuesBy`为 GRIB 消息中的所有数据添加偏移量。

```
$ grib_get -F "%.5f" -p max,min,average t.grb2
313.09029 202.71029 277.65163

$ grib_set -s offsetValuesBy=-273.15 t.grb2 tc.grb2

$ grib_get -F "%.5f" -p max,min,average tc.grb2
39.94029 -70.43971 4.50163
```

可以设置 `scaleValuesBy` 将 GRIB 消息中的所有数据乘以一个系数。

```
$ grib_get -F "%.2f" -p max,min,average u1.grb2
27.27 -27.69 -0.03

$ grib_set -s scaleValuesBy=10 u1.grb2 u2.grb2

$ grib_get -F "%.2f" -p max,min,average u2.grb2
272.71 -276.87 -0.33
```

### 输出文件中使用键值

键值可以用于设置输出文件名。

```
$ grib_set -s time=0180 gmf.gra.2018010300003.grb2 "out_[shortName].grb2"
$ ls out_*
out_10u.grb2    out_asnow.grb2     out_grle.grb2   out_mcc.grb2
```

记住：请使用引号保护 `[]`。

### `grib_set` 无法完成的任务

`grib_get` 无法用于对数据的表示方式做变换。比如无法将数据从谱格式转变为格点格式。

`grib_get` 无法将数据从一种格点格式转换为另一种格点格式。比如不能将数据从 regular 或 reduced 高斯网格转为经纬度网格。

`grib_get` 无法用于选择数据的子区域。可以修改类似 `latitudeOfFirstGridPointInDegrees` 等 key 的值，但数据仍然定义在原有的网格中。

> GRIB 工具不能对数据进行插值。

