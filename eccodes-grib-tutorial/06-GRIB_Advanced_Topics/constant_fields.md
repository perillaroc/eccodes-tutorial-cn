# 常数场（Constant fields）

如果要素场的所有数值都相等，那么使用常规的 GRIB 消息会将同一个数值重复存储 N 次，效率非常低。

ecCodes 使用常数场保存这种类型的数据，该常数应该是唯一保存的数值，数据段应该为空，因此常数场非常小，并且被精确地编码。

使用如下命令可以创建一个常数场：

```
grib_set -d 1 in.grib out.grib
```

常数场中未定义打包参数（packing parameters），即 `bitsPerValue=0`。

## 常数场的问题

加载一个常数场：

```fortran
codes_grib_new_from_file(infile, igrib)
```

设置一些非常数的数据值：

```fortran
codes_set(igrib, 'values', values)
```

**注意**：此时还不知道打包参数

将消息写入到文件中：

```fortran
codes_write(igrib, outfile)
```

我们会得到什么样的 `packingError`？

因为常数场未设置打包参数，因此 ecCodes 不知道我们需要什么样的小数精度。
一个安全的选择是设置 `bitsPerValue = 24`。

更好的实践是在设置数值前设置 `decimalPrecision` 或 `bitsPerValue`。

```fortran
codes_grib_new_from_file(infile,igrib)
codes_set(igrib,’decimalPrecision’,4)
codes_set(igrib,’values’,values)
codes_write(igrib,outfile)
```

```fortran
codes_grib_new_from_file(infile,igrib)
codes_set(igrib,’bitsPerValue’,16)
codes_set(igrib,’values’,values)
codes_write(igrib,outfile)
```

## 练习

1. 创建一个常数 GRIB 文件。

    ```
    $ grib_set -d 1 t.grib2 c.grib2
    $ grib_set -s packingType=grid_simple c.grib2 c1.grib2
    $ grib_dump c1.grib2 
    ***** FILE: c1.grib2 
    #==============   MESSAGE 1 ( length=179 )                 ==============
    GRIB {
      # Meteorological products (grib2/tables/4/0.0.table)  
      discipline = 0;
      editionNumber = 2;
      ...
      values(1036800) =  {
      1, 1, 1, 1, 1, 
      1, 1, 1, 1, 1, 
      1, 1, 1, 1, 1, 
      ...
      } 
      #-READ ONLY- maximum = 1;
      #-READ ONLY- minimum = 1;
      #-READ ONLY- average = 1;
      #-READ ONLY- numberOfMissing = 0;
      #-READ ONLY- standardDeviation = 0;
      #-READ ONLY- skewness = 0;
      #-READ ONLY- kurtosis = 0;
      #-READ ONLY- isConstant = 1;
      #-READ ONLY- getNumberOfValues = 1036800;
    }
    ```

2. 设置 `values={23.26, 42.51, 61.22, 45.95}`，打印 `packingError` 和 `bitsPerValue`。

    创建一个 filter 规则文件 step02.filter。

    ```
    $ cat step02.filter 
    set values={23.26, 42.51, 61.22, 45.95};
    write;
    print "packingError = [packingError]";
    print "bitsPerValue = [bitsPerValue]";
    ```

    执行命令：

    ```
    $ grib_filter -o step02.grib2 step02.filter c1.grib2
    packingError = 0.00500122070312
    bitsPerValue = 12
    ```

> 译注：此处应该无法读取 `packingError` 值，但实际运行结果可以显示，可能我用的方式有问题。

3. 设置 `decimalPrecision=1`，再设置相同的数据值，再一次打印 `packingError` 和 `bitsPerValue`。

    创建一个规则文件 `step03.filter`。

    ```
    $ cat step03.filter 
    set decimalPrecision=1;
    set values={23.26, 42.51, 61.22, 45.95};
    write;
    print "packingError = [packingError]";
    print "bitsPerValue = [bitsPerValue]";
    ```

    执行命令：

    ```
    $ grib_filter -o step03.grib2 step03.filter c1.grib2
    packingError = 0.0500007629395
    bitsPerValue = 9
    ```

4. 比较两个文件的大小和 `packingError`。

    两个文件大小相似，但 `packingError` 不同，见步骤 2 和 3 的输出。

    ```
    185 step02.grib2
    184 step03.grib2
    ```
