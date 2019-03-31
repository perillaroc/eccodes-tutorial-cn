---
title: 解码已加载的 GRIB 消息
weight: 3
---

目标尽可能地少解码。永远不要解码整个**已加载的 GRIB 消息**。可以使用 `grib_dump -D <grib_file>` 查看到底有多少个 ecCodes key。

用于解码的主要函数：

```c
int codes_get_long (codes_handle *h, const char *key, long *value);
int	codes_get_double (codes_handle *h, const char *key, double *value)
int	codes_get_string (codes_handle *h, const char *key, char *mesg, size_t *length)

int codes_get_long_array (codes_handle *h, const char *key, long *vals, size_t *length)
int	codes_get_double_array (codes_handle *h, const char *key, double *vals, size_t *length)
```

## 帮助函数

获取一个（数组）数据的大小：

```c
int codes_get_size(codes_handle* h, const char* key,size_t *size);
int grib_get_size(grib_handle* h, const char* key,size_t *size);
```

详细输出 grib 消息的内容：

```c
void codes_dump_content(codes_handle* h,FILE* out,const char* mode, unsigned long option_flags,void* arg);
void grib_dump_content(grib_handle* h,FILE* out,const char* mode, unsigned long option_flags,void* arg);
```

检查某个 key 是否被定义（存在）：

```c
int codes_is_defined(codes_handle* h, const char* key);
int grib_is_defined(grib_handle* h, const char* key);
```

检查某个 key 的值是否是缺失值：

```c
int codes_is_missing(codes_handle* h, const char* key, int* err);
int grib_is_missing(grib_handle* h, const char* key, int* err);
```

统计文件中的消息个数：

```c
int codes_count_in_file(codes_context* c, FILE* f,int* n);
int grib_count_in_file(grib_context* c, FILE* f,int* n);
```

## 示例：codes_get

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

    FILE* in = fopen(file_path, "rb");
    if(!in)
    {
        cout<<"ERROR: unable to open file "<<file_path<<endl;
        return 1;
    }

    int err = 0;
    codes_handle *h = nullptr;

    while((h=codes_handle_new_from_file(nullptr, in, PRODUCT_GRIB, &err)) != nullptr)
    {
        long data_date = -1;
        codes_get_long(h, "dataDate", &data_date);

        long type_of_level = -1;
        codes_get_long(h, "typeOfLevel", &type_of_level);

        long level = -1;
        codes_get_long(h, "level", &level);

        size_t size;
        codes_get_size(h, "values", &size);

        double *values = new double[size];
        codes_get_double_array(h, "values", values, &size);

        cout<<data_date<<" "<<type_of_level<<" "<<level<<" "<<size<<" "<<values[size-1]<<endl;

        delete [] values;
        codes_handle_delete(h);
    }

    fclose(in);

    return 0;
}
```

## 示例：grib_get

将上面的代码替换为以 `grib_` 开头的函数

```cpp
#include <iostream>

#include <grib_api.h>

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
    grib_handle *h = nullptr;

    while((h=grib_handle_new_from_file(nullptr, in, &err)) != nullptr)
    {
        long data_date = -1;
        grib_get_long(h, "dataDate", &data_date);

        long type_of_level = -1;
        grib_get_long(h, "typeOfLevel", &type_of_level);

        long level = -1;
        grib_get_long(h, "level", &level);

        size_t size;
        grib_get_size(h, "values", &size);

        double *values = new double[size];
        grib_get_double_array(h, "values", values, &size);

        cout<<data_date<<" "<<type_of_level<<" "<<level<<" "<<size<<" "<<values[size-1]<<endl;

        delete [] values;
        grib_handle_delete(h);
    }

    fclose(in);

    return 0;
}
```

## ecCodes 可以做更多的事情

ecCodes 的宗旨是提供高层次的 key 集合和不同的函数，用于检索或计算已加载 GRIB 消息中的额外信息。

对于不同的应用：

- 只读key，返回数据值的均值和最值，分布经纬度信息
- 计算经纬度、数据值的函数：
    `codes_get_data`
- 获取指定坐标点最近的数据值
    `codes_grib_nearest_find`
- 根据 index 列表获取数据值
    `codes_get_double_elements`
