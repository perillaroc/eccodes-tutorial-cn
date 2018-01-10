# 获取数据值

## grib_get_data

> 打印数据值

使用`grib_get_data`打印一个或多个 GRIB 文件中的一系列（格点数据的）纬度（latitude）、经度（longitude）和数据值。

输出格式可以通过 -f 选项使用 C 风格的描述符控制。

- `-F "%.4f"` - 小数格式，带4位小数（1.2345）
- `-F "%.4e"` - 指数格式，带4位小数（1.2345E-03）
- 默认格式是`-F "%.4e"`

默认情况下，不输出缺失值（misssing values）。
用户可以提供在缺失值处输出的字符串。

默认情况下，`grib_get_data`在出错时会异常结束。
使用`-f`选项强制`grib_get_data`在出错时不异常结束。

### 用法

```
grib_get_data [options] grib_file grib_file
```

选项 | 含义
---|----
-p key1,key2,... | Keys to print
-w key1=val1,key2!=val2,... | Where option
-m missingValue | Specify missing value string
-F format | C-style format for output values
-f | Do not fail on error
... | ...

### 示例

```
$ grib_get_data -w count=72 -F "%.4f" gmf.gra.2018010300003.grb2 
Latitude, Longitude, Value
   89.875    0.000 244.9167
   89.875    0.250 244.8767
   89.875    0.500 244.8867
   89.875    0.750 244.9667
```

> 格式化字符串只作用在数值上，不影响经纬度坐标值。

缺失值显示，没有相关示例，用PPT中的例子代替。

```
> grib_get_data –m XXXXX –F “%.4f” f1.grib1
Latitude, Longitude, Value
...
    81.000  90.000 9.4189 
    81.000  91.500 8.6782 
    81.000  93.000 XXXXX 
    81.000  94.500 XXXXX 
    81.000  96.000 XXXXX 
    81.000  97.500 XXXXX 
    81.000  99.000 6.7627 
    81.000 100.500 7.4097 
    81.000 102.000 7.9307
...
```

### 练习

使用`grib_get`和`grib_get_data`。
