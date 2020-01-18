---
title: where选项
weight: 3
---

where选项 `-w` 用于打印符合约束条件的消息。
约束条件有如下的格式：

## IS

```
key=value
```

下面命令打印层次值等于100的要素场。

```
$ grib_ls -w level=100 gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType
2            babj         20200118     fc           regular_ll   isobaricInhPa  100          3            gh           grid_jpeg
...
2            babj         20200118     fc           regular_ll   isobaricInhPa  100          3            unknown      grid_jpeg
24 of 837 messages in gmf.gra.2020011800003.grb2

24 of 837 total messages in 1 files
```

## IS NOT

```
key!=value
```

下面命令打印层次类型不等于surface的要素场。

```
$ grib_ls -w typeOfLevel!=surface gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType
2            babj         20200118     fc           regular_ll   nominalTop   0            3            ulwrf        grid_jpeg
2            babj         20200118     fc           regular_ll   nominalTop   0            3            unknown      grid_jpeg
2            babj         20200118     fc           regular_ll   nominalTop   0            0-3          nswrf        grid_jpeg
2            babj         20200118     fc           regular_ll   nominalTop   0            3            uswrf        grid_jpeg
2            babj         20200118     fc           regular_ll   nominalTop   0            3            nswrfcs      grid_jpeg
2            babj         20200118     fc           regular_ll   nominalTop   0            3            uswrf_cs     grid_jpeg
2            babj         20200118     fc           regular_ll   heightAboveGround  2            3            q            grid_jpeg
...
```

## AND

```
key1=value1,key2=value2,key3=value3
```

下面代码打印层次值为100的温度场。

```
$ grib_ls -w level=100,shortName=t gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType
2            babj         20200118     fc           regular_ll   isobaricInhPa  100          3            t            grid_jpeg
1 of 837 messages in gmf.gra.2020011800003.grb2

1 of 837 total messages in 1 files
```

## OR

```
key=value1/value2/value3
```

下面代码打印层次值为100或200或300或500的温度场

```
$ grib_ls -w level=100/200/300/500,shortName=t gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType
2            babj         20200118     fc           regular_ll   isobaricInhPa  500          3            t            grid_jpeg
2            babj         20200118     fc           regular_ll   isobaricInhPa  300          3            t            grid_jpeg
2            babj         20200118     fc           regular_ll   isobaricInhPa  200          3            t            grid_jpeg
2            babj         20200118     fc           regular_ll   isobaricInhPa  100          3            t            grid_jpeg
4 of 837 messages in gmf.gra.2020011800003.grb2

4 of 837 total messages in 1 files
```

使用 `-w` 参数意味着只有满足所有约束条件的消息才会被处理。
