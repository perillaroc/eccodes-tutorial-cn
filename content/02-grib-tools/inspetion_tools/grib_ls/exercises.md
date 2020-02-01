---
title: 练习
weight: 20
---

## t2m.grib1 vs t2m.grib2

GRIB 1 和 GRIB 2 的输出略有不同，在处理 grib_ls 的输出时候需要注意。

选择正确的选项。

- [ ] typeOfLevel 的值不同
- [ ] 没有不同
- [ ] 两者打印不同的默认key
- [ ] 两者打印相同的默认key
- [ ] 列顺序不一样

## 列出默认key

我们刚刚发现两个GRIB版本打印的默认key相同。
请写出默认key的名称。

## 列出要素场

对于某个 GRIB 2 文件，里面有哪些要素场？

## 打印MARS keys

打印 MARS keys，使用 t2m.grib2 文件。

## 打印 shortName

与 MARS keys 一同打印 shortName，使用 t2m.grib1 文件。

## 降序排列输出

下面的哪个命令用于降序打印step。

- [ ] grib_ls -B "step desc" t2m.grib1
- [ ] grib_ls -B step:i desc t2m.grib1
- [ ] grib_ls "step:i desc" t2m.grib1
- [ ] grib_ls -B "step:i desc" t2m.grib1

## 重新指定输出

使用 `grib_ls` 按顺序打印 centre, dataDate, stepRange, typeOfLevel 和 shortName，只打印 forecast step 6。使用 t2m.grib1 文件。

## 打印整数和字符串

使用 `grib_ls` 按顺序打印 centre（同时打印整型和字符串）, dataDate, stepRange, typeOfLevel 和 shortName，只打印 forecast step 6。使用 t2m.grib1 文件。