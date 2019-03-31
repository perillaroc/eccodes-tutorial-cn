#  Python API

## 细节

导入包：

```py
# grib api
import gribapi
from gribapi import *

# eccodes
import eccodes
from eccodes import *
```

提供底层，过程式的接口。

提供几乎与 C 接口一一对应的接口。

使用 NumPy 库处理数据值。

## 加载/释放 GRIB 消息

`codes_grib_new_from_file`
`codes_any_new_from_file`
`codes_new_from_file`

`codes_new_from_samples`

`codes_new_from_message`

`codes_release`

## 解码

`codes_get`

`codes_get_array`

`codes_get_values`

`CodesInternalError`

## 工具

`codes_grib_find_nearest`

`codes_grib_iterator_new`

`codes_grib_iterator_next`

`codes_grib_iterator_delete`


## 索引

`codes_index_new_from_file`

`codes_index_add_file`

`codes_index_write`

`codes_index_read`

`codes_index_release`

`codes_index_get_size`

`codes_index_get`

`codes_index_select`

`codes_new_from_index`

## 编码

`codes_get`

`codes_set_array`

`codes_set_values`

`codes_clone`
