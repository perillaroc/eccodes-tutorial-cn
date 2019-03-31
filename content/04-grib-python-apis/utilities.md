---
title: 工具
weight: 5
---

## 最近邻点

```py
points = codes_grib_find_nearest (gid, inlat, inlon, is_lsm=False, npoints=1)
point = points[0]
# point : {
#    lat: lat, lon: lon, index: index, value: value, distance: distance
# }
```

寻找给定经纬度点的最近邻点的 tuple。当 `npoints=4` 时将返回 4 个最近邻点的tuple。

### 示例

下面的示例寻找北京（39.92,116.46）的 1 个最近邻点。

```py
from __future__ import print_function
import click
import eccodes


@click.command()
@click.argument('file_path')
def cli(file_path):
    with open(file_path, 'rb') as f:
        handle = eccodes.codes_grib_new_from_file(f, headers_only=False)
        while handle is not None:
            date = eccodes.codes_get(handle, "dataDate")
            type_of_level = eccodes.codes_get(handle, "typeOfLevel")
            level = eccodes.codes_get(handle, "level")

            points = eccodes.codes_grib_find_nearest(handle, 39.92, 116.46, False, 1)
            point = points[0]

            print(date, type_of_level, level, " :", point.lat, point.lon, point.value, point.distance)

            eccodes.codes_release(handle)
            handle = eccodes.codes_grib_new_from_file(f, headers_only=False)


if __name__ == "__main__":
    cli()
```

部分输出结果：

```
20180113 isobaricInhPa 10  : 39.9375 116.4375 226.705515625 2.73275631598
20180113 isobaricInhPa 20  : 39.9375 116.4375 226.627828125 2.73275631598
20180113 isobaricInhPa 30  : 39.9375 116.4375 220.17278125 2.73275631598
...
```

## 迭代器

ecCodes 的迭代器用于读取数据值，每次调用返回经纬度坐标和数据值。

创建迭代器

```py
iter_id = codes_grib_iterator_new (gid,mode)
```

获取数据值

```py
[lat,lon,value] = codes_grib_iterator_next (iterid)
```

释放迭代器

```py
codes_grib_iterator_delete (iter_id)
```

### 示例

```py
from __future__ import print_function
import click
import eccodes


@click.command()
@click.argument('file_path')
def cli(file_path):
    with open(file_path, 'rb') as f:
        handle = eccodes.codes_grib_new_from_file(f, headers_only=False)
        date = eccodes.codes_get(handle, "dataDate")
        type_of_level = eccodes.codes_get(handle, "typeOfLevel")
        level = eccodes.codes_get(handle, "level")

        iter_id = eccodes.codes_grib_iterator_new(handle, 0)

        while 1:
            result = eccodes.codes_grib_iterator_next(iter_id)
            if not result:
                break

            [lat, lon, value] = result
            print(lat, lon, value)

        eccodes.codes_grib_iterator_delete(iter_id)
        eccodes.codes_release(handle)


if __name__ == "__main__":
    cli()
```
