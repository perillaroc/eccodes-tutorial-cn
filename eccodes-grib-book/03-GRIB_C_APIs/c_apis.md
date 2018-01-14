# ecCodes GRIB C 接口

## 处理 GRIB 数据的 ecCodes 函数名

ecCodes 的所有函数都以 `codes_` 开头，例如

```c
codes_handle_new_from_file(0, in, PRODUCT_GRIB, &err);
```

一些函数可以用于 GRIB 和 BUFR 两种数据，例如

```c
// GRIB
codes_get_long(h, "Ni", &numberOfPointsAlongAParallel)
// BUFR
codes_get_long(h, "blockNumber", &longVal)
```

另一些函数则有所不同，例如

```c
grib_handle * codes_grib_handle_new_from_file(grib_context *c, FILE *f, int *error);
grib_handle * codes_bufr_handle_new_from_file(grib_context *c, FILE *f, int *error);
grib_handle * codes_handle_new_from_file(grib_context *c, FILE *f, ProductKind product, int *error);
```

GRIB API 中以 `grib_` 开头的函数在 ecCodes 依然存在。例如

```c
grib_handle* grib_handle_new_from_file(grib_context* c, FILE* f, int* error);
```

## 通用框架

一个使用 ecCodes 接口（Fortran/C/Python）的代码包括以下步骤

- 打开一个或多个 GRIB 文件，用于读取或写入。
- 加载一个或多个 GRIB 消息到内存
    - 加载 GRIB 消息的函数返回一个唯一的 grib handler，可以用于操控加载的 GRIB 消息。
- 编码或解码加载的 GRIB 消息
    - 只能编码或解码**加载**的 GRIB 消息
    - 使用者应仅加载需要的内容，而不是整个消息
- 将一个或多个 GRIB 消息写入文件（编码时需要的步骤）
- 释放加载的 GRIB 消息
- 关闭打开的 GRIB 文件

以上框架对 BUFR 数据也适用。

## ecCodes 接口细节

所有的函数都以一个可选的参数，用于错误处理。

```c
grib_handle* grib_handle_new_from_file(grib_context* c, FILE* f, int* error);
```

上面代码中的 error 就是错误返回码。

如果没有指定 error 并且函数调用发生错误，整个程序会退出，并返回错误代码。

请使用 error 处理错误，例如，在 MPI 并行程序中需要处理错误，下面用 Fortran 代码说明。

```fortran
call codes_grib_new_from_samples(igrib, samplename, status)
if (status /= 0) then
    call codes_get_error_string(status,err_msg) or grib_get_error_string
    print*,’ECCODES Error: ’,trim(err_msg),’ (err=‘,status,’)’
    call mpi_finalize(ierr)
    stop
end if
```

请在如下网站查询错误代码和它们的含义：

[http://download.ecmwf.int/test-data/eccodes/html/group__errors.html](http://download.ecmwf.int/test-data/eccodes/html/group__errors.html)

## 加载/释放 GRIB 消息

因为 ecCodes 仅能解码/编码加载过的 GRIB 消息，所以首先要加载消息。

有三个主要的函数用于加载一个 GRIB 消息。

- `codes_handle_new_from_file (NULL, f, PRODUCT_GRIB, &error)` 或 `grib_handle_new_from_file`

    从已经存在并使用 `fopen` 已打开的文件中加载 GRIB 消息。

- `codes_grib_handle_new_from_samples(NULL, 'regular_ll_sfc_grib2')` 或 `grib_handler_new_from_samples`

    从一个样例中加载 GRIB 消息，用于编码。后面会详细介绍。

- `codes_handle_new_from_index(index, &error)` 或 `grib_handle_new_from_index`

    从一个 index 中加载 GRIB 消息，index 需要事先创建好。后面会详细介绍。

上面三种加载函数将返回一个唯一的 grib handle，使用该 handle 可以操控已加载的 GRIB 消息。

无法直接访问包含已加载 GRIB 消息的缓存。缓存由 ecCodes 库**内部**处理。

任何 GRIB 消息占用的缓存都保存在内存中。

因此，当某个已加载的 GRIB 消息不再需要时，**总应该**调用 `codes_handle_delete(handle)` 函数释放其占用的空间。

## 示例：从文件中加载 GRIB 消息

```c
#include <iostream>
#include <eccodes.h>

using namespace std;

int main(int argc, char** argv)
{
    if(argc < 2)
    {
        cout<<"Usage: "<<argv[0]<<" grib_file_path";
        return 1;
    }

    const char* file_path = argv[1];

    FILE* in = fopen(file_path, "rb");
    if(!in)
    {
        cout<<"ERROR: unable to open file "<<file_path<<endl;
        return 1;
    }

    int err = 0;
    codes_handle *h = codes_handle_new_from_file(0, in, PRODUCT_GRIB, &err);
    if(h == nullptr)
    {
        cout<<"ERROR: unable to create handle from file "<<file_path<<endl;
        return 1;
    }

    codes_handle_delete(h);
    fclose(in);

    return 0;
}
```

## 解码已加载的 GRIB 消息

目标尽可能地少解码。永远不要解码整个**已加载的 GRIB 消息**。可以使用 `grib_dump -D <grib_file>` 查看到底有多少个 ecCodes key。

用于解码的主要函数：

```c
int codes_get_long (codes_handle *h, const char *key, long *value);
int	codes_get_double (codes_handle *h, const char *key, double *value)
int	codes_get_string (codes_handle *h, const char *key, char *mesg, size_t *length)

int codes_get_long_array (codes_handle *h, const char *key, long *vals, size_t *length)
int	codes_get_double_array (codes_handle *h, const char *key, double *vals, size_t *length)
```

