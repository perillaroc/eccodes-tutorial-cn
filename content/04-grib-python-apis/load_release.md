---
title: 加载/释放 GRIB 消息
weight: 3
---

需要使用如下函数：

- `codes_grib_new_from_file`
- `codes_any_new_from_file`
- `codes_new_from_file`
- `codes_new_from_samples`
- `codes_new_from_message`
- `codes_release`

```py
gid = codes_grib_new_from_file (file, headers_only=False) 
codes_any_new_from_file (file, headers_only=False) 
codes_new_from_file (file, product_kind, headers_only)
```

`product_kind` 接受如下值：

- `CODES_PRODUCT_GRIB`
- `CODES_PRODUCT_BUFR`
- `CODES_PRODUCT_ANY`

上述的 `codes_grib_new_from_file` 函数返回 GRIB 消息的句柄。
输入文件必须是 Python 的文件对象。现在不推荐使用 `headers_only` 参数。

## codes_new_from_samples

```py
gid = codes_new_from_samples (samplename)
```

返回样例目录的文件中消息的句柄。

## codes_new_from_message

```py
gid = codes_new_from_message (message)
```

返回内存中消息的句柄。

## codes_release

```py
codes_release (gid)
```

释放句柄。

## 示例：读取 GRIB 文件

```py
from __future__ import print_function
import sys

import click
import eccodes


@click.command()
@click.argument('file_path')
def cli(file_path):
    with open(file_path, 'rb') as f:
        handle = eccodes.codes_grib_new_from_file(f, headers_only=False)
        if handle is None:
            print("ERROR: unable to create handle from file " + file_path)
            sys.exit(-1)

        eccodes.codes_release(handle)


if __name__ == "__main__":
    cli()
```
