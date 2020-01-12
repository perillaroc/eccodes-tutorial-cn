---
title: "编码和计算key"
weight: 4
---

GRIB 消息中 key 的值并不总是直接编码到文件中。

某些 key 是其他一些 key 的组合，并通过给定的算法提供，或者只是临时的（transient）。

## Coded keys

Coded key 直接对应于 GRIB 消息中的八位字节。
它们的值仅通过解码八位字节获取。

GRIB 消息中的所有 codes keys 可以通过 octet 模式的 grib_dump 获取。

{{< highlight text >}}
***** FILE: gfs.t00z.pgrb2.1p00.f138
#==============   MESSAGE 1 ( length=3801 )                ==============
1-4       identifier = GRIB
5-6       reserved = 0
7         discipline = 0 [Meteorological products (grib2/tables/2/0.0.table) ]
8         editionNumber = 2
9-16      totalLength = 3801
======================   SECTION_1 ( length=21, padding=0 )    ======================
1-4       section1Length = 21
5         numberOfSection = 1
6-7       centre = 7 [US National Weather Service - NCEP  (WMC)  (common/c-11.table) ]
8-9       subCentre = 0
10        tablesVersion = 2 [Version implemented on 4 November 2003 (grib2/tables/1.0.table) ]
11        localTablesVersion = 1 [Unknown code table entry () ]
12        significanceOfReferenceTime = 1 [Start of forecast (grib2/tables/2/1.2.table) ]
13-14     year = 2020
15        month = 1
16        day = 11
17        hour = 0
18        minute = 0
{{< /highlight >}}

## Computed keys

Computed key 是 通过组合其他 key （coded 或 computed）获取的。

它们提供了一种访问消息中信息的方法，可以访问复杂的属性。

设置 computed key 的值会设置所有与其相关的所有key。
例如，设置 `typeOfGrid=regular_ll` 将会将Grid Definition Section中所有相关的key设置为使用经纬度网格。

MARS 关键词以 computed key 的形式访问。

MARS 是 ECMWF 开发的 Meteorological Archival Retrieval System。
向组织和个人提供访问ECMWF大量气象数据的接口。