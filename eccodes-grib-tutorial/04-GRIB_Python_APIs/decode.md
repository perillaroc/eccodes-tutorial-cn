# Python API：解码

## 示例：

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
            values = eccodes.codes_get_values(handle, "values")
            value = values[-1]

            print(date, type_of_level, level, value)

            eccodes.codes_release(handle)
            handle = eccodes.codes_grib_new_from_file(f, headers_only=False)


if __name__ == "__main__":
    cli()
```