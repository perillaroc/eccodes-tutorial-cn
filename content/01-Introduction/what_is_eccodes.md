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

## ecCodes 方法