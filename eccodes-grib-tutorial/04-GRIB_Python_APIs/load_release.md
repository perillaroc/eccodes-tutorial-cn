# 加载/释放 GRIB 消息

需要使用如下函数：

- `codes_grib_new_from_file`
- `codes_any_new_from_file`
- `codes_new_from_file`
- `codes_new_from_samples`
- `codes_new_from_message`
- `codes_release`

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
