---
title: 编码
weight: 7
---

## codes_set

```py
codes_set (gid, key, value)
```

设置 GRIB 消息中的 key 值。

## code_set_array

```py
codes_set_array (gid, key, value)
```

设置 GRIB 消息中的数组 key 值。

输入数组可以是 `numpy.ndarray` 或 Python 序列类型，例如 tuple, list, array, ...

## codes_set_value

```py
codes_set_array (gid, key, value)
```

设置消息中‘数据’值的工具函数。

## codes_clone

```py
clone_id = codes_clone (gid_src)
```

克隆一个消息。

可以使用 `codes_write` 直接写入到文件中。

别忘了调用 `codes_release`。

## 示例

```py
from __future__ import print_function
import click
import eccodes


@click.command()
@click.argument('output_file_path')
def cli(output_file_path):
    handle = eccodes.codes_grib_new_from_samples('regular_ll_pl_grib2')

    value_size = eccodes.codes_get_size(handle, 'values')
    values = []
    for i in range(0, value_size):
        values.append(i)
    eccodes.codes_set_values(handle, values)

    with open(output_file_path, 'wb') as output_file:
        eccodes.codes_write(handle, output_file)

    eccodes.codes_release(handle)


if __name__ == "__main__":
    cli()
```

比较 C 接口和 Python 接口生成的两个文件，完全相同。

```
$ diff output.grib2 output.py.grib2
```
