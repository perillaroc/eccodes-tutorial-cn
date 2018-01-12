# 复制消息

## grib_copy

> 拷贝 GRIB 文件的内容

使用 `grib_copy` 从 GRIB 文件中拷贝选定的消息，并可以选择打印某些 key 值。

不带选项的 `grib_copy` 不输出任何 key 信息。

可以通过选项指定需要打印的 key 列表，使用详情选项 `-v` 打印 key。

输出支持排序。例如升序或降序排列 step。

key 值可用于设定输出文件名。

如果某个 key 未找到，`grib_copy` 会出错退出。可以使用 `-f` 选项强制 `grib_copy` 在遇到错误是不出错。

### 用法

```
grib_copy[options] grib_file grib_file … out_grib_file
```

选项

选项 | 含义
-----|-----

-p key1,key2,… | 需要打印的 key 列表 (与 `-v` 一同使用)
-w key=val1,key2!=val2,… | where 选项
-B “key1 asc, key2 desc” | 排序，例如: “step asc, centre desc”
-v | 显示详情
-f | 出错时不异常退出
... | ...


### 示例

只拷贝文件中的地面层要素场

```
$ grib_copy -w typeOfLevel=surface gmf.gra.2018010300000.grb2 output.grb2

$ grib_ls output.grb2
output.grb2
edition      centre       date         dataType     gridType     typeOfLevel  level        stepRange    shortName    packingType  
2            babj         20180103     an           regular_ll   surface      0            0            acpcp        grid_jpeg   
2            babj         20180103     an           regular_ll   surface      0            0            ncpcp        grid_jpeg   
2            babj         20180103     an           regular_ll   surface      0            0            unknown      grid_jpeg   
...
32 of 32 messages in output.grb2

32 of 32 total messages in 1 files
```

拷贝文件中所有非地面层的要素场

```
$ grib_copy -w typeOfLevel!=surface gmf.gra.2018010300000.grb2 output.grb2
windroc@ubuntu:~/tmp/data$ grib_ls output.grb2
output.grb2
edition      centre       date         dataType     gridType     stepRange    typeOfLevel  level        shortName    packingType  
2            babj         20180103     an           regular_ll   0            nominalTop   0            ulwrf        grid_jpeg   
2            babj         20180103     an           regular_ll   0            nominalTop   0            unknown      grid_jpeg   
2            babj         20180103     an           regular_ll   0            nominalTop   0            nswrf        grid_jpeg   
》。。
729 of 729 messages in output.grb2

729 of 729 total messages in 1 files

```

可以使用 `-v` 和 `-p` 输出信息。

```
$ grib_copy -v -p shortName -w typeOfLevel=surface gmf.gra.2018010300000.grb2 output.grb2
gmf.gra.2018010300000.grb2
shortName     
acpcp        
ncpcp        
...     
32 of 761 messages in gmf.gra.2018010300000.grb2

32 of 761 total messages in 1 files
```

### 输出文件中使用 key 值

key 值可以用于设置输出文件名。注意需要使用引号来防止 shell 解析 `[]`。

```
$ grib_copy gmf.gra.2018010300000.grb2 "out_[shortName].grb2"
$ ls out_*
out_10u.grb2  out_2t.grb2     out_ccl.grb2       out_dpt.grb2 ...
```

该方式提供将一个文件中 GRIB 消息分割成不同文件的方法。
