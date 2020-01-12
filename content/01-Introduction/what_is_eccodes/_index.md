---
title: "ecCodes简介"
weight: 1
---

## 什么是 ecCodes

ecCodes 是 ECMWF 开发的气象数据编解码包，支持下列WMO标准的二进制格式：

- GRIB 版本1和版本2
- BUFR 版本3和版本4
- GTS abbreviated header

解码包包含：

- Fortran90，C 和 Python 的 API 接口
- 一组方便使用数据的命令行工具

本教程将专注于解码GRIB数据。

ecCodes 提供对消息内容的高级访问，隐藏二进制格式的复杂性。
提供简单和可靠的方式使用相同的函数编解码 GRIB1 和 GRIB 2 消息。
同时 ecCodes 可以很容易地更新到最新版本的 GRIB 模板和表格。

## ecCodes 方法

ecCodes 使用 key/value 的方式访问 GRIB 消息中的信息。例如 `originatingCentre` 保存 "originating centre"，`numberOfPointsAlongAParallel` 保存 "number of points along a parallel" 的值。

不同GRIB消息之间key集合根据下面的条件变化：

- GRIB版本号
- 消息内容

改变一个key可能导致某些其它key失效或者可用。

eccodes区分编码key（coded keys，消息中的原样编码），和计算key（computed keys，临时或从其他key计算出的key）。

某些key有别名，例如

- Ni 或 Nx 或 numberOfColumns 是 numberOfPointsAlongAParallel 的别名
- Nj 或 Ny 或 numberOfRows 是 numberOfPointsAlongAMeridan 的别名


## 与 GRIB-API 的关系

ecCodes 的前身就是的 GRIB-API，ecCodes 是后者的升级（添加对 BUFR 格式）的支持。

对于 GRIB 编解码，ecCodes 提供与 GRIB-API 同样的功能。
目前 ecCodes 是 ECMWF 使用的主要 GRIB 编解码工具。
ecCodes 最终将完全替代 GRIB-API。

ecCodes 使用 Apache 2.0 许可证。

![](asset/grib_api_to_eccodes.png)

## ecCodes 与更早期的软件对比

早期基于 Fortran 的工具提供对 GRIB 消息基于数组的访问方式。例如：

- ksec2(2) => 纬度圈格点个数
- ksec2(3) => 经度圈格点个数

ecCodes 提供基于键值对的访问方式。例如：

- NumberOfPointsAlongAParallel => 经度圈格点个数
- NumberOfPointsAlongAMeridan => 纬度圈格点个数

## ecCodes 架构

![](asset/eccodes_architecture.png)

## ecCodes 与 GRIB 版本1和版本2

<table>
<tbody>
<tr>
  <td>GRIB 1 编码</td>
  <td>latitudeOfFirstGridPoint=40000</td>
</tr>
<tr>
  <td>GRIB 2 编码</td>
  <td>latitudeOfFirstGridPoint=40000000</td>
</tr>
<tr>
  <td>ecCodes计算值</td>
  <td>latitudeOfFirstGridPointInDegrees=40</td>
</tr>
</tbody>
</table>

<table>
<tbody>
<tr>
  <td>GRIB 1 编码</td>
  <td>
    dataRepresentationType=4<br />
    numberOfPointAlongAParallel=MISSING<br />
    ijDirectionIncrementGiven=0<br />
    pl ={...}
  </td>
</tr>
<tr>
  <td>GRIB 2 编码</td>
  <td>
    ridDefinitionTemplateNumber=40<br />
    numberOfPointsAlongAParallel=MISSING<br />
    iDirectionIncrementGiven=0<br />
    iDirectionIncrement=MISSING<br />
    pl={...}
  </td>
</tr>
<tr>
  <td>ecCodes计算值</td>
  <td>gridType=reduced_gg</td>
</tr>
</tbody>
</table>

## 接口

所有接口都提供设置/读取 GRIB 消息中键值对的方法，每类接口的键值对均相同。

- C接口提供所有的功能（工具库用C语言编写）
- Fortran 90接口提供主要的功能。
- Python 接口通过访问 C 函数提供功能。
- Tools 提供命令行工具。

### C接口

```c
h = codes_handle_new_from_file(context, in, PRODUCT_GRIB, &err);

codes_get_double(h, "latitudeOfFirstGridPointInDegrees", &lat1); 
codes_set_long(h, "centre", centre);
codes_set_string(h, "date", date, &len);

codes_handle_delete(h);
```

> 注意：为了兼容 grib_api，ecCodes 依然支持以 grib_ 开头的函数。

### Fortran 90接口

```fortran
call codes_new_from_file(ifile, igrib, CODES_PRODUCT_GRIB, iret)

call codes_get(igrib, 'latitudeOfFirstGridPointInDegrees', lat1) 
call codes_set(igrib, 'centre', centre)
call codes_set(igrib, ‘date', ’20070212’)

call codes_release(igrib)
```

> 注意：为了兼容 grib_api，ecCodes 依然支持以 grib_ 开头的函数。

### Python 接口

```python
gid = codes_new_from_file(f, CODES_PRODUCT_GRIB)

lat = codes_get(gid, 'latitudeOfFirstGridPointInDegrees') 
codes_set(gid, 'centre', centre)
codes_set(gid, 'date', date)

codes_release(gid)
```

> 注意：为了兼容 grib_api，ecCodes 依然支持以 grib_ 开头的函数。

#### Tools

```bash
grib_get –p latitudeOfFirstGridPointInDegrees input.grib 
grib_set –s centre=ecmf,date=20070212 input.grib out.grib
```

## 下载和安装

ecCodes 使用 CMake 编译和安装。

CMake 是跨平台的免费软件，使用与编译器无关的方式管理软件的过程。

从如下网址下载 ecCodes：

    https://software.ecmwf.int/wiki/display/ECC/Releases

从如下网址获取安装说明：

    https://software.ecmwf.int/wiki/display/ECC/ecCodes+installation

### 安装要求

Linux 或者 Mac 平台

- 较新版本的 CMake （版本大于 2.8.11）和 GNU Make
- C 编译器 （gcc，icc，craycc）

可选，但推荐安装：

- Fortran 编译器：用于 Fortran 接口
- Python：用于 Python 接口
- Jasper/OpenJPEG：用于 jpeg 解码
- netCDF/netCDF4：用于 grib_to_netcdf 转换工具

编译命令示例：

```
$> tar -xzf  eccodes-x.y.z-Source.tar.gz
$> mkdir build ; cd build
$> PREFIX=/path/to/where/you/install/eccodes
$> cmake -DCMAKE_INSTALL_PREFIX=$PREFIX ../eccodes-x.y.z-Source
...
$> make
$> ctest
$> make install
```

## 帮助和支持

提交bug，请联系

Software.Support@ecmwf.int

Wiki：

https://software.ecmwf.int/wiki/display/ECC/ecCodes+Home

请使用论坛讨论一般问题：

https://software.ecmwf.int/wiki/display/ECC/Forums
