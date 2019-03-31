---
title: 索引
weight: 6
---

## codes_index_new_from_file

```py
iid = codes_index_new_from_file (file, keys)
```

返回创建的索引句柄。

## codes_index_add_file

```py
codes_index_add_file (iid, file)
```

添加一个文件到索引。

## codes_index_write

```py
codes_index_write (iid, file)
```

将索引写入文件，以便后续使用。

## codes_index_read

```py
iid = codes_index_read (file)
```

从 `codes_index_write` 输出的文件中加载索引。

## codes_index_release

```py
codes_index_release (iid)
```

释放索引。

## codes_index_get_size

```py
size = codes_index_get_size (iid, key)
```

索引 key 不重复的键值总数

## codes_index_get

```py
values = codes_index_get (iid, key, ktype=str)
```

获取索引 key 的不重复键值列表。

## codes_index_select

```py
codes_index_select (iid, key, value)
```

选择满足 `key==value` 的 GRIB 消息子集。

## codes_new_from_index

与 `codes_grib_new_from_file` 相同，并使用 `codes_release` 释放消息。

## 示例：

```py
from __future__ import print_function
import click
import eccodes


@click.command()
@click.argument('file_path')
def cli(file_path):
    index_id = eccodes.codes_index_new_from_file(str(file_path), ["paramId"])

    eccodes.codes_index_add_file(index_id, str(file_path))

    param_size = eccodes.codes_index_get_size(index_id, "paramId")
    print("param id count:", param_size)

    param_id_list = eccodes.codes_index_get(index_id, "paramId")
    print("param id list:", param_id_list)

    eccodes.codes_index_select(index_id, "paramId", '131')

    handle = eccodes.codes_new_from_index(index_id)
    while handle is not None:
        short_name = eccodes.codes_get(handle, "shortName")
        date = eccodes.codes_get(handle, "dataDate")
        type_of_level = eccodes.codes_get(handle, "typeOfLevel")
        level = eccodes.codes_get(handle, "level")

        print(short_name, date, type_of_level, level)
        eccodes.codes_release(handle)
        handle = eccodes.codes_new_from_index(index_id)


if __name__ == "__main__":
    cli()
```
