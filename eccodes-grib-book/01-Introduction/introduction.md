# GRIB 和 ecCodes 介绍

## 什么是GRIB？

GRIB是由世界气象组织 (World Meteorological Organization，WMO)维护的二进制文件格式。
用于编码由数值天气预报模式(Numerical Weather Prediction models)生成的数据。

GRIB 最初代表 GRIdded Binary，但后来被扩展为 General Regularly-distributed Information in Binary form。

## GRIB文件长什么样？

GRIB 格式是面向二进制的数据交换格式，与文本文件等人类可读的文件不同，必须由符合WMO标准规则的软件解码。

下图是使用16进制编辑器打开 GRIB 文件的示例，除了最开始4个字节外，其余部分都无法解析成文本，后面将会看到前4个字节对应的 ASCII 编码内容 GRIB 正是 GRIB 文件的标志。

### WMO 二进制编码

WMO 提供对 GRIB 编码标准的完整描述文件 Manual on Codes。

这是 WMO 二进制编码 GRIB 的唯一官方文档，可以从 WMO 的官网上获取。

    http://www.wmo.int/pages/prog/www/WMOCodes.html

每三年发布完整的说明文档。

每年两次发布作为新版本的代码表格，代码表格是说明文档的一部分。

最新版本的表格请访问 WMO 网站

    http://www.wmo.int/pages/prog/www/WMOCodes/WMO306_vI2/LatestVERSION/LatestVERSION.html




