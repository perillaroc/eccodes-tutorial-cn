---
title: 选项
weight: 2
---

## 选项

参数 | 含义
----|----
`-p key[:{s|i|d}],...` | 需要输出的 key 列表
`-P key[:{s|i|d}],...` | 附加输出的 key 列表 
`-w key[:{s|i|d}]{=|!=}value,...` | where 子句
`-B "key asc, key desc..."` | 排序: "step asc, centre desc"
`-n namespace` | 打印命名空间中的所有key（ls, parameter, statistics, geography, time, mars, vertical)
`-m` | 打印 MARS keys （-n mars的缩写）
`-W width` | 最小列宽，默认为10
... | ...

### -p

```
-p key1, key2, ...
```

指定需要打印信息的key列表。默认使用字符串格式打印。

每个key可以指定以不同的格式打印，包括字符串（key:s），双精度浮点数（key:d）和整数（key:i）。
例如 `-p centre:i` 打印数据的 originating centre 的整数值。

下面的命令打印指定的key值：数据生成的中心，日期，时间，数据类型，变量名，层次类型和层次值。

```
$ grib_ls -p centre:i,dataDate,dataTime,dataType,shortName,typeOfLevel,level gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
centre       dataDate     dataTime     dataType     shortName    typeOfLevel  level
38           20200118     0            fc           acpcp        surface      0
38           20200118     0            fc           ncpcp        surface      0
38           20200118     0            fc           unknown      surface      0
...
38           20200118     0            fc           unknown      surface      0
38           20200118     0            fc           unknown      surface      0
837 of 837 messages in gmf.gra.2020011800003.grb2

837 of 837 total messages in 1 files
```

上面的数据由中国气象局数值预报中心生成，centre是38，表示babj。

### -P

```
-P key1,key2,...
```

与`-p`类似，不过指定的key会在默认的列表基础上额外打印。

下面的代码额外输出变量的单位。

```
$ grib_ls -P units gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
units        edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType
kg m**-2     2            babj         20200118     fc           regular_ll   surface      0            0-3          acpcp        grid_jpeg
kg m**-2     2            babj         20200118     fc           regular_ll   surface      0            0-3          ncpcp        grid_jpeg
unknown      2            babj         20200118     fc           regular_ll   surface      0            0-3          unknown      grid_jpeg
......
m            2            babj         20200118     fc           regular_ll   surface      0            3            vis          grid_jpeg
m s**-1      2            babj         20200118     fc           regular_ll   heightAboveGround  10           3            gust         grid_jpeg
unknown      2            babj         20200118     fc           regular_ll   surface      0            3            unknown      grid_jpeg
unknown      2            babj         20200118     fc           regular_ll   surface      0            3            unknown      grid_jpeg
837 of 837 messages in gmf.gra.2020011800003.grb2

837 of 837 total messages in 1 files
```

### -w

```
-w ...
```

指定打印信息的消息需要满足的约束条件。

下面的命令打印700hPa的温度场。

```
$ grib_ls -w level=700,shortName=t gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType
2            babj         20200118     fc           regular_ll   isobaricInhPa  700          3            t            grid_jpeg
1 of 837 messages in gmf.gra.2020011800003.grb2

1 of 837 total messages in 1 files
```

### -B

```
-B "key asc,key desc"
```

指定消息打印的顺序。
例如，`-B "step:i asc,centre desc"` 将按整形排序step值并按降序排序centre值（默认为字符串）。

下面的命令按照level数值从大到小排序要素场。

```
$ grib_ls -B "level:i desc" gmf.gra.2020011800003.grb2
edition      centre       date         dataType     gridType     stepRange    typeOfLevel  level        shortName    packingType
2            babj         20200118     fc           regular_ll   3            heightAboveGroundLayer  6000         vwsh         grid_jpeg
2            babj         20200118     fc           regular_ll   3            heightAboveGroundLayer  3000         vwsh         grid_jpeg
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  1000         q            grid_jpeg
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  1000         v            grid_jpeg
...
2            babj         20200118     fc           regular_ll   3            surface      0            unknown      grid_jpeg
2            babj         20200118     fc           regular_ll   0-3          surface      0            acpcp        grid_jpeg
2            babj         20200118     fc           regular_ll   3            surface      0            unknown      grid_jpeg
```


下面的命令先按照shortName从a到z排序，再按照level数值从小到大排序要素场。

```
$ grib_ls -B "shortName asc,level:i asc" gmf.gra.2020011800003.grb2
edition      centre       date         dataType     gridType     stepRange    typeOfLevel  level        shortName    packingType
2            babj         20200118     fc           regular_ll   3            heightAboveGround  10           10u          grid_jpeg
2            babj         20200118     fc           regular_ll   3            heightAboveGround  10           10v          grid_jpeg
2            babj         20200118     fc           regular_ll   3            heightAboveGround  2            2d           grid_jpeg
2            babj         20200118     fc           regular_ll   3            heightAboveGround  2            2r           grid_jpeg
2            babj         20200118     fc           regular_ll   3            heightAboveGround  2            2t           grid_jpeg
2            babj         20200118     fc           regular_ll   0-3          surface      0            acpcp        grid_jpeg
2            babj         20200118     fc           regular_ll   3            surface      0            al           grid_jpeg
2            babj         20200118     fc           regular_ll   0-3          surface      0            asnow        grid_jpeg
2            babj         20200118     fc           regular_ll   3            surface      0            bli          grid_jpeg
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  10           ccl          grid_jpeg
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  20           ccl          grid_jpeg
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  30           ccl          grid_jpeg
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  50           ccl          grid_jpeg
...
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  925          wz           grid_jpeg
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  950          wz           grid_jpeg
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  975          wz           grid_jpeg
2            babj         20200118     fc           regular_ll   3            isobaricInhPa  1000         wz           grid_jpeg
```

### -n

```
-n namespace
```

打印指定namespace的所有key。例如 `-n parameter` 将打印centre, paramId, units, name 和 shortName。

```
$ grib_ls -n parameter gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
centre      paramId     shortName   units       name
babj        3063        acpcp       kg m**-2    Convective precipitation (water)
babj        260009      ncpcp       kg m**-2    Large scale precipitation (non-convective)
babj        0           unknown     unknown     unknown
babj        260025      asnow       m           Total snowfall
babj        130         t           K           Temperature
...
babj        0           unknown     unknown     unknown
babj        3020        vis         m           Visibility
babj        260065      gust        m s**-1     Wind speed (gust)
babj        0           unknown     unknown     unknown
babj        0           unknown     unknown     unknown
837 of 837 messages in gmf.gra.2020011800003.grb2

837 of 837 total messages in 1 files
```

### -m

```
-m
```

是 `-n mars` 的缩写，将打印MARS namespace的所有key。

```
$ grib_ls -m gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
date        time        levtype     step        param
20200118    0000        sfc         3           3063
20200118    0000        sfc         3           260009
20200118    0000        sfc         3           0
20200118    0000        sfc         3           260025
20200118    0000        sfc         3           130
...
20200118    0000        sfc         3           0
20200118    0000        sfc         3           3020
20200118    0000        sfc         3           260065
20200118    0000        sfc         3           0
20200118    0000        sfc         3           0
837 of 837 messages in gmf.gra.2020011800003.grb2

837 of 837 total messages in 1 files
```

### -i

```
-i index
```

打印给定index对应数据点的值。例如，`-i 345` 将要素场变量值数组中的第345个值。

```
$ grib_ls -i 345 gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType          value(345)
2            babj         20200118     fc           regular_ll   surface      0            0-3          acpcp        grid_jpeg   0
2            babj         20200118     fc           regular_ll   surface      0            0-3          ncpcp        grid_jpeg   0.031586
...
```

### -F

```
-F format
```

使用C风格样式表达式控制输出格式。
例如 `-F "%.4f"` 打印4位小数（1.2345），`-F "%.4e"` 打印以指数形式打印并包含4位小数（1.2345E-03)。
默认格式是 `-F %.10e`。

```
$ grib_ls -F "%.1f" -i 345 -p centre:i,dataDate,dataTime,dataType,shortName,typeOfLevel,level  gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
centre       dataDate     dataTime     dataType     shortName    typeOfLevel  level                value(345)
38           20200118     0            fc           acpcp        surface      0           0.0
38           20200118     0            fc           ncpcp        surface      0           0.0
38           20200118     0            fc           unknown      surface      0           0.0
38           20200118     0            fc           asnow        surface      0           0.0
38           20200118     0            fc           t            surface      0           243.7
38           20200118     0            fc           str          surface      0           1691476.9
```

### -W

```
-W width
```

指定最小的列宽。默认是10。

```
$ grib_ls -W 8 -F "%.1f" -i 345 -p centre:i,dataDate,dataTime,dataType,shortName,typeOfLevel,level  gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
centre       dataDate     dataTime     dataType     shortName    typeOfLevel  level                value(345)
38           20200118     0            fc           acpcp        surface      0           0.0
38           20200118     0            fc           ncpcp        surface      0           0.0
38           20200118     0            fc           unknown      surface      0           0.0
38           20200118     0            fc           asnow        surface      0           0.0
38           20200118     0            fc           t            surface      0           243.7
38           20200118     0            fc           str          surface      0           1691476.9
```

### -l

```
-l lat,lon[,MODE,FILE]
```

打印与给定坐标latitude/longitude最近的点的值。

```
$ grib_ls -l 30,40 -w count=1/2 gmf.gra.2020011800000.grb2
gmf.gra.2020011800000.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType         value1  value2  value3  value4
2            babj         20200118     an           regular_ll   surface      0            0            acpcp        grid_jpeg    0 0 0 0
2            babj         20200118     an           regular_ll   surface      0            0            ncpcp        grid_jpeg    0 0 0 0
2 of 2 messages in gmf.gra.2020011800000.grb2

2 of 2 total messages in 1 files
Input Point: latitude=30.00  longitude=40.00
Grid Point chosen #2 index=345760 latitude=29.88 longitude=40.00 distance=13.90 (Km)
Other grid Points
- 1 - index=345761 latitude=29.88 longitude=40.25 distance=27.81 (Km)
- 2 - index=345760 latitude=29.88 longitude=40.00 distance=13.90 (Km)
- 3 - index=344321 latitude=30.12 longitude=40.25 distance=27.79 (Km)
- 4 - index=344320 latitude=30.12 longitude=40.00 distance=13.90 (Km)
```

### -s

```
-s key=val
```

为了显示目的而临时设置某个key的值。

下面的示例修改stepUnits的值，原始数据单位是小时，使用 `grib_ls` 修改为秒。

```
$ grib_ls -n time -w count=1/2 gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
dataDate      dataTime      stepType      stepUnits     startStep     endStep       stepRange     validityDate  validityTime
20200118      0000          accum         h             0             3             0-3           20200118      300
20200118      0000          accum         h             0             3             0-3           20200118      300
2 of 2 messages in gmf.gra.2020011800003.grb2

2 of 2 total messages in 1 files
```

```
$ grib_ls -s stepUnits=s -n time -w count=1/2 gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
dataDate      dataTime      stepType      stepUnits     startStep     endStep       stepRange     validityDate  validityTime
20200118      0000          accum         s             0             10800         0-10800       20200118      300
20200118      0000          accum         s             0             10800         0-10800       20200118      300
2 of 2 messages in gmf.gra.2020011800003.grb2

2 of 2 total messages in 1 files
```