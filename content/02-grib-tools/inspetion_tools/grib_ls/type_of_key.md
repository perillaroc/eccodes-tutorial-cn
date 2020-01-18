---
title: 指定key的类型
weight: 4
---

ecCodes的key都有默认类型，例如字符串，整型或浮点数。

- `key:i`：整数
- `key:s`：字符串
- `key:d`：浮点数

centre默认是字符串类型。

```
$ grib_ls -p centre:s -w count=1 gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
centre
babj
1 of 1 messages in gmf.gra.2020011800003.grb2
```

使用整型打印centre值，babj对应38。

```
$ grib_ls -p centre:i -w count=1 gmf.gra.2020011800003.grb2
gmf.gra.2020011800003.grb2
centre
38
1 of 1 messages in gmf.gra.2020011800003.grb2
```

