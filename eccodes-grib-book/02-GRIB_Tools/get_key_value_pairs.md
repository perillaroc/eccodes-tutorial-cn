# 获取键值对

## grib_get

> 获取键值对

使用`grib_get`从一个或多个文件中获取一个或多个key值，与`grib_ls`类似。

默认情况下，如果有错误发生（例如没有找到key），`grib_get`则会运行失败，返回非0退出值。

- 适合在脚本中获取 GRIB 消息的键值。
- 可以强制`grib_get`在出错时不会运行失败。

支持获取所有的 MARS key 和特定名称空间中的所有 key。可以添加其他 key 到默认集合。

浮点类型数据的输出支持 C 风格的格式化描述。

可以用于寻找某个经纬度坐标点的最近邻点，与`grib_ls`工作方式相同。

## 用法

```
grib_get [options] grib_file grib_file ...
```

## 选项

选项 | 含义
---|---
-p key1,key2,... | Keys to get
-P key1,key2,... | Additional keys to get with –m, -n
-w key1=val1,key2!=val2,... | Where option
-s key1=val1,key2=val2,... | Keys to set (temporary for printing) 
-m | Get all MARS keys
-n namespace | Get all keys for namespace 
-l lat,lon[,MODE,FILE] | Value(s) nearest to lat-lon point 
-F format | Format for floating point values
-f | Do not fail on error
... | ...


## 使用 where 选项

where 选项 -w 可以用于所有的 GRIB 工具，具有相同的格式。

约束条件可以表示为如下几种形式：

- `key=value`
- `key!=value`
- `key=value1|value2|value3`


