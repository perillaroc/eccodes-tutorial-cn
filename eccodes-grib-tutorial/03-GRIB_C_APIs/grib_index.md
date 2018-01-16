# ecCodes 索引访问

先介绍与索引访问有关的几个函数。

- 创建文件内容的索引

```cpp
codes_index* codes_index_new_from_file(codes_context* c, char* filename,const char* keys,int *err);
```

- 从索引中获取某个 key 的维度

```cpp
int codes_index_get_size(codes_index* index,const char* key,size_t* size);
```

- 从索引中获取 key 的不同“值”

```cpp
int codes_index_get_long(codes_index* index,const char* key,long* values,size_t *size);
int codes_index_get_double(codes_index* index,const char* key, double* values,size_t *size);
int codes_index_get_string(codes_index* index,const char* key,char** values,size_t *size);
```

- 从索引中为某个 key 选择一个“值”

```cpp
int codes_index_select_long(codes_index* index,const char* key,long value);
int codes_index_select_double(codes_index* index,const char* key,double value);
int codes_index_select_string(codes_index* index,const char* key,char* value);
```

- 加载与选择模型对应的 GRIB 消息

```cpp
codes_handle* codes_handle_new_from_index(codes_index* index,int *err);
```

