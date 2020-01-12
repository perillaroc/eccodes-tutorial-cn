---
title: "Namespaces"
weight: 5
---

ecCodes 通过版本无关 key 提供 GRIB 消息的高级访问方式。

强烈建议尽可能使用这些版本无关 key，可以透明地处理 GRIB 1 和 GRIB 2 数据。

为了方便使用，某些版本无关 key 被放到 namespace 中。

下面介绍几个最重要的 namespace。
更多信息请访问[Edition independent ecCodes keys](https://apps.ecmwf.int/codes/grib/format/edition-independent/)

## parameter

Key name | Example value
---------|--------------
paramId  | 151
shortName | msl
centre | ecmf (or 98)
name | Mean sea level pressure
unit | Pa

## time

起报时间

Key name | Example value
---------|--------------
dataDate | 20160224(YYYYMMDD)
dataTime | 0, 600, 1200, 1800

预报时间点

Key name | Example value
---------|--------------
stepType | instant, accum, avg, max, min, ...
stepUnits | s, m, h, 3h, 6h, 12h, D, M, Y, 10Y, 30Y, C
startStep | 0, 3, ...
endStep (= step) | 0, 3, ...
stepRange | 3-6, 6 ("startStep-endStep", "endStep)

预报时长

Key name | Example value
---------|--------------
validityDate | 20160224(YYYYMMDD)
validityTime | 0, 300, 1200, 1800

## geography

Key name | Example value
---------|--------------
gridType | reduced_gg, regular_ll, sh, ...
latitudeOfFirstGridPointInDegrees | 90.0, 55.5, ...
longitudeOfFirstGridPointInDegrees | 0.0, 250.0, ...
latitudeOfLastGridPointInDegrees | -90.0, 35.0, ...
longitudeOfLastGridPointInDegrees | 360.0, 50.0, ...
iDirectionIncrementInDegrees | 0.5, ...
jDirectionIncrementInDegrees | 0.5, ...
N | 640, 320, ...
... | ...

## vertical

Key name | Example value
---------|--------------
typeOfLevel | hybrid, surface, depthBelowLandLayer, isobaricInhPa, ...
level | 0, 1, 137, 1000, 850, ...

## mars

Key name | Example value
---------|--------------
date | 20160224 (YYYYMMDD)
time | 0000, 0600, 1200, 1800
step | 3, 6, 9, 12, ...
class | od, ...
stream | oper, enfo, ...
expver | 0001
type | an, fc, cf, pf, ...
levtype | sfc, pl, ml
levelist | 500, 850, ...
param | 151.128
