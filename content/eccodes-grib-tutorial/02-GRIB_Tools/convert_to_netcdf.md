# GRIB 转 NetCDF

## grib_to_netcdf

> 转换为 NetCDF 格式

使用 `grib_to_netcdf` 将 GRIB 要素场转码为 NetCDF 格式。

输入的 GRIB 要素场必须为标准网格。即 `typeOfGrid` 是 `regular_ll` 或 `regular_gg`。

可以通过选项进行如下设置：

- NetCDF 数据类型
    - `NC_BYTE`, `NC_SHORT`, `NC_INT`, `NC_FLOAT` 或 `NC_DOUBLE`
    - 默认类型是 `NC_SHORT`

- 经典（NetCDF 3）格式或 NetCDF 4 格式
- 参考日期
    - 默认是 `19000101`

该命令用于 MARS 的 web 接口和公共数据服务，用于提供 NetCDF 格式的文件。

### 用法

```
grib_to_netcdf [options] grib_file grib_file ...
```

选项

选项 | 含义
----|----
-o output_file | 输出的 NetCDF 文件名
-R YYYYMMDD | 使用 YYYYMMDD 作为参考日期
-D NC_DATATYPE | NetCDF 日期类型
-k kind | 文件格式: <br/>1 → netCDF 经典文件格式<br/>2 → netCDF 64位经典文件格式（默认）<br/>3 → netCDF-4 文件格式</br>4 → netCDF-4 经典模型文件格式
-T | 不使用有效时间
-u dimension | 设置 `dimension` 为不受限制的维度
-f | 出错时不异常退出
... | ...

### 示例

将文件转为 NetCDF

```
$ grib_to_netcdf -o out.nc t.grib2
grib_to_netcdf: Version 2.6.0
grib_to_netcdf: Processing input file 't.grib2'.
grib_to_netcdf: Found 1 GRIB field in 1 file.
grib_to_netcdf: Ignoring key(s): method, type, stream, refdate, hdate
grib_to_netcdf: Creating netCDF file 'out.nc'
grib_to_netcdf: NetCDF library version: 4.1.3 of Feb 24 2014 21:05:37 $
grib_to_netcdf: Creating large (64 bit) file format.
grib_to_netcdf: Defining variable 't'.
grib_to_netcdf: Done.

$ ls -s out.nc
2036 out.nc
```

将输出文件的数据类型设为 `NC_FLOAT`。

```
$ grib_to_netcdf -D NC_FLOAT -o out.nc t.grib2
grib_to_netcdf: Version 2.6.0
grib_to_netcdf: Processing input file 't.grib2'.
grib_to_netcdf: Found 1 GRIB field in 1 file.
grib_to_netcdf: Ignoring key(s): method, type, stream, refdate, hdate
grib_to_netcdf: Creating netCDF file 'out.nc'
grib_to_netcdf: NetCDF library version: 4.1.3 of Feb 24 2014 21:05:37 $
grib_to_netcdf: Creating large (64 bit) file format.
grib_to_netcdf: Defining variable 't'.
grib_to_netcdf: Done.

$ ls -s out.nc
4060 out.nc
```

输出的 NetCDF 文件大小大约是之前的文件两倍。
