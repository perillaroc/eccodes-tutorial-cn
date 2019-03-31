# GRIB 1 到 2 的转换

```
grib_set -s edition=2 in.grib1 out.grib2
```

上述命令会转换如下内容：

- time
- geography
- vertical
- parameter
- local
- data

因为两种编码标准各有不同，再加上一些气象中心会使用本地码表，所以参数转换比较复杂。

转换会基于变量参数的唯一标识符 `paramId`。

## 基于 `paramId` 的转换

如何生成 2 米温度的 GRIB 文件：

```
grib_set -s paramId=165 in.grib1 out.grib1
grib_set -s paramId=165 in.grib2 out.grib2
```

如何将 GRIB 1 文件转码为 GRIB 2 文件：

```
grib_set -s edition=2 in.grib1 out.grib2
```

在转换到版本 2 的过程中，ecCodes 从 GRIB 1 拷贝 `paramId` 到 GRIB 2：

1. 从 GRIB 1 中获取 `paramId`（=165）
2. 修改版本号为 2，生成 GRIB 2 文件
3. 设置 GRIB 2 的 `paramId`（=165）

转换仅在 `paramId` 定义了两个版本的情况下可行。

访问参数数据库网站查看转换是否可行。

## 练习

使用上一节生成的文件。

1. 将 p.grib1 转换为 GRIB 版本 2 文件 p_converted.grib2

    ```
    grib_set -s edition=2 p.grib1 p_converted.grib2
    ```

2. 查询 p.grib2 的参数命名空间。

    ```
    $ grib_ls -n parameter p.grib2
    p.grib2
    centre      paramId     shortName   units       name        
    ecmf        165         10u         m s**-1     10 metre U wind component 
    1 of 1 messages in p.grib2

    1 of 1 total messages in 1 file
    ```

3. 查询 p_converted.grib2 的参数命名空间。

    ```
    $ grib_ls -n parameter p_converted.grib2 
    p_converted.grib2
    centre      paramId     shortName   units       name        
    ecmf        165         10u         m s**-1     10 metre U wind component 
    1 of 1 messages in p_converted.grib2

    1 of 1 total messages in 1 files
    ```

    可以看到，不同版本的 GRIB 消息的变量信息相同。

4. 准备一个当前定义配置无法识别的 GRIB 消息：

    ```
    $ grib_ls -n parameter undefined.grib2 
    undefined.grib2
    centre      paramId     shortName   units       name        
    babj        0           unknown     unknown     unknown    
    1 of 1 messages in undefined.grib2

    1 of 1 total messages in 1 files
    ```

5. 将上面的文件转换成 GRIB 1 文件。

    ```
    $ grib_set -s edition=1 undefined.grib2 undefined.grib1
    ECCODES ERROR   :  concept: no match for paramId=0
    ECCODES ERROR   :  grib_set_values[0] edition (1) failed: Concept no match
    ```
