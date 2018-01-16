# 编码 GRIB 文件

## 编码已加载的 GRIB 消息

尽可能少进行“编码”。永远不要“编码”**整个 GRIB 消息**。

进行“编码”的主要函数：

```cpp
int codes_set_long(codes_handle* h, const char* key, long val);
int codes_set_double(codes_handle* h, const char* key, double val);
int codes_set_string(codes_handle* h, const char*  key , const char* mesg, size_t *length);
int codes_set_double_array(codes_handle* h, const char* key, const double* vals, size_t length);
```

将 GRIB 消息写入文件：

```cpp
int codes_write_message(codes_handle* h,const char* file,const char* mode);
```

注意，使用 `codes_write_message` 写入的 GRIB 消息会保证**消息格式正确**，但无法保证**语义正确**。


## 创建 GRIB 消息

可以根据样例文件创建新的 GRIB 消息。

样例文件是保存在 sample 文件夹中的示例 GRIB 消息文件。默认的 sample 目录可以使用 `codes_info` 查看。
样例文件以 `.tmpl` 结尾，用户可以创建自己的样例文件，修改/添加环境变量 `ECCODES_SAMPLES_PATH`，指向这些目录。

从样例文件创建 GRIB 消息。

```cpp
codes_handle* codes_grib_handle_new_from_samples (codes_context* c, const char* sample_name);
```

从其它消息克隆并拷贝一个新的消息：

```cpp
codes_handle* codes_handle_clone(codes_handle* h);
```

## 示例：codes_set

```cpp
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

    const char* sample_filename = "regular_ll_pl_grib2";
    int err = 0;
    codes_handle *h = codes_grib_handle_new_from_samples(nullptr,  sample_filename);
    if(h == nullptr)
    {
        cout<<"ERROR: unable to create handle from file "<<file_path<<endl;
        return 1;
    }

    size_t value_size;
    codes_get_size(h, "values", &value_size);

    auto values = new double[value_size];
    double v = 0;
    for(auto i=0; i<value_size; i++)
    {
        values[i] = v;
        v++;
    }
    codes_set_long(h, "bitsPerValue", 16);
    codes_set_double_array(h, "values", values, value_size);

    codes_write_message(h, file_path, "wb");

    delete [] values;
    codes_handle_delete(h);

    return 0;
}
```

## 修改网格定义和打包类型

可以通过修改 key `gridType` 和 `packingType` 来修改网格定义和打包类型。例如：

```cpp
codes_get_string(h, "gridType", "ploar_stereographic", &grid_type_size);
```

将为 GRIB 消息 h 定义 “Polar Stereographic Projection Grid“ 网格。

```cpp
codes_get_string(h, "packingType", "grid_simple", &packing_type_size);
```

将使用简单打包方式打包数据。

网格定义和打包类型可以查看：

[https://software.ecmwf.int/wiki/display/ECC/GRIB%3A+Keys](https://software.ecmwf.int/wiki/display/ECC/GRIB%3A+Keys)

## 使用不同的打包类型

GRIB 数据可以使用多种方式打包，例如 simple packing，second order packing，...

GRIB 1 和 GRIB 2 支持的打包方式略有不同。

格点数据和谱数据由打包类型。

打包类型影响 GRIB 消息的大小。例如 second order packing 打包的数据大小是 simple packing 的两倍。

打包类型影响打包、解包的时间。例如 second order packing 明显比 simple packing 慢。

打包不会丢失信息。

更多说明请看练习章节。

