# 加载/释放 GRIB 消息

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