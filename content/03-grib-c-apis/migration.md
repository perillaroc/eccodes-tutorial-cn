---
title: 移植
weight: 4
---

## 从 GRIBEX 迁移到 ecCodes

译者注：没用过 GRIBEX，建议使用 ecCodes。

## 从 GRIB API 迁移到 ecCodes

对于 GRIB 数据，GRIB API 和 ecCodes 不同**仅限于**软件名称和图标。

不需要修改原有的 GRIB API 代码，仅需在连接库时连接 ecCodes 的库即可。

但可以将 `grib_` 开头的函数替换成以 `codes_` 开发的函数。

注意新的功能只在 ecCodes 中开发，未来将不再维护 GRIB API。
