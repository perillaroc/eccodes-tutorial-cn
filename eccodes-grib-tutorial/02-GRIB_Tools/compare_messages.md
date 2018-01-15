# 比较消息

## grib_compare

> 比较 GRIB 消息

使用`grib_compare`比较两个文件中的 GRIB 消息。

默认情况下，两个文件中的消息会按相同顺序，逐位比较，精确比较浮点数值。

- 可以指定数据值的误差，基于绝对值（absolute）、相对值（relative）或 packing error。
- 默认的数据值误差是绝对误差为0。

如果`grib_compare`找到不同：

- 切换到 key 模式寻找那些编码的 key 是不同的。
- 异常退出，并返回一个非零退出值。

提供选项用于设置仅比较特定的 key 或一组 key。

### 基本用法

```
grib_compare [options] grib_file grib_file
```

选项 | 参数
----|----
-b key1,key2,... | 比较时忽略的 key 列表
-c key1,namespace2:n... | 指定比较的 key 列表
-H | 仅比较消息头
-e | 版本无关比较
-w key1=val1,key2!=val2,... | where 选项
-f | 出错时不退出
-r | 消息排序不一致
-v | 显示详细信息
... | ...

### 示例

#### 简单示例

f1.grib1 和 f2.grib1 中的两个 GRIB 消息包含不同预报时效的海陆掩码。

```
> grib_compare f1.grib1 f2.grib1
-- GRIB #1 -- shortName=lsm paramId=172 stepRange=3
  levelType=sfc level=0 packingType=grid_simple
  gridType=reduced_gg --
long [P1]: [3] != [6]

> echo $?
1
```

因为比较失败，所以返回值被设为1

我们将 P1 key 放入黑名单再重新比较：

```
> grib_compare -b P1 f1.grib1 f2.grib1

> echo $?
0
```

因为设置了黑名单，所以比较成功，返回值被设为0。

#### 详细输出

`verbose`选项会显示所有被比较的 key 的详情。下面比较两个不同预报时效的要素场。

```$ grib_compare -w count=72 -v  gmf.gra.2018010300003.grb2 gmf.gra.2018010300009.grb2 gmf.gra.2018010300009.grb2
  comparing identifier as string
  comparing discipline as long
  comparing editionNumber as long
  comparing totalLength as long

-- GRIB #37 -- shortName=unknown paramId=0 stepRange=3 levelType=sfc level=0 packingType=grid_jpeg gridType=regular_ll --
long [totalLength]: [495109] != [628595]
  comparing section1Length as long
  comparing numberOfSection as long
  comparing centre as string
  comparing subCentre as long
  comparing tablesVersion as long
  comparing localTablesVersion as long
  comparing significanceOfReferenceTime as long
  comparing year as long
  comparing month as long
  comparing day as long
  comparing hour as long
  comparing minute as long
  comparing second as long
  comparing productionStatusOfProcessedData as long
  comparing typeOfProcessedData as string
  comparing section3Length as long
  comparing numberOfSection as long
  comparing sourceOfGridDefinition as long
  comparing numberOfDataPoints as long
  comparing numberOfOctectsForNumberOfPoints as long
  comparing interpretationOfNumberOfPoints as long
  comparing gridDefinitionTemplateNumber as long
  comparing shapeOfTheEarth as long
  comparing scaleFactorOfRadiusOfSphericalEarth as long
  comparing scaledValueOfRadiusOfSphericalEarth as long
  comparing scaleFactorOfEarthMajorAxis as long
  comparing scaledValueOfEarthMajorAxis as long
  comparing scaleFactorOfEarthMinorAxis as long
  comparing scaledValueOfEarthMinorAxis as long
  comparing Ni as long
  comparing Nj as long
  comparing basicAngleOfTheInitialProductionDomain as long
  comparing subdivisionsOfBasicAngle as long
  comparing latitudeOfFirstGridPoint as long
  comparing longitudeOfFirstGridPoint as long
  comparing resolutionAndComponentFlags as long
  comparing latitudeOfLastGridPoint as long
  comparing longitudeOfLastGridPoint as long
  comparing iDirectionIncrement as long
  comparing jDirectionIncrement as long
  comparing scanningMode as long
  comparing section4Length as long
  comparing numberOfSection as long
  comparing NV as long
  comparing productDefinitionTemplateNumber as long
  comparing parameterCategory as long
  comparing parameterNumber as long
  comparing typeOfGeneratingProcess as long
  comparing backgroundProcess as long
  comparing generatingProcessIdentifier as long
  comparing hoursAfterDataCutoff as long
  comparing minutesAfterDataCutoff as long
  comparing indicatorOfUnitOfTimeRange as long
  comparing forecastTime as long
long [forecastTime]: [3] != [9]
  comparing typeOfFirstFixedSurface as string
  comparing scaleFactorOfFirstFixedSurface is set to missing in both fields
  comparing scaledValueOfFirstFixedSurface is set to missing in both fields
  comparing typeOfSecondFixedSurface as long
  comparing scaleFactorOfSecondFixedSurface is set to missing in both fields
  comparing scaledValueOfSecondFixedSurface is set to missing in both fields
  comparing section5Length as long
  comparing numberOfSection as long
  comparing numberOfValues as long
  comparing dataRepresentationTemplateNumber as long
  comparing referenceValue as double
  (1 values) tolerance=0.000976562 	using compare_double_absolute
double [referenceValue]: [-4.28992724609375000000e+03] != [-1.32530458984375000000e+04]
	absolute diff. = 8963.12, relative diff. = 0.676306
	tolerance=0.000976562
  comparing binaryScaleFactor as long
  comparing decimalScaleFactor as long
  comparing bitsPerValue as long
long [bitsPerValue]: [14] != [15]
  comparing typeOfOriginalFieldValues as long
  comparing typeOfCompressionUsed as long
  comparing targetCompressionRatio as long
  comparing section6Length as long
  comparing numberOfSection as long
  comparing bitMapIndicator as long
  comparing section7Length as long
long [section7Length]: [494933] != [628419]
  comparing numberOfSection as long
  comparing codedValues as double
  (1036800 values) tolerance=0 	using compare_double_absolute
double [codedValues]: 1036800 out of 1036800 different
 max absolute diff. = 8.9631186523437500e+04, relative diff. = 0.676306
	max diff. element 690373: -4.28992724609375000000e+04 -1.32530458984375000000e+05
	tolerance=0.0000000000000000e+00
	values max= [42230.7]  [100240]         min= [-42899.3] [-132530]
  comparing 7777 as string
```

#### 限制比较的 key

使用`-c`选项指定要比较的 key

```
$ grib_compare -w count=72 -c stepRange gmf.gra.2018010300003.grb2 gmf.gra.2018010300009.grb2 

-- GRIB #37 -- shortName=unknown paramId=0 stepRange=3 levelType=sfc level=0 packingType=grid_jpeg gridType=regular_ll --
string [stepRange]: [3] != [9]
```

#### 仅比较头信息

比较两个不同时效的要素场，默认情况

```
$ grib_compare -w count=72 gmf.gra.2018010300003.grb2 gmf.gra.2018010300009.grb2 

-- GRIB #37 -- shortName=unknown paramId=0 stepRange=3 levelType=sfc level=0 packingType=grid_jpeg gridType=regular_ll --
long [totalLength]: [495109] != [628595]
long [forecastTime]: [3] != [9]
double [referenceValue]: [-4.28992724609375000000e+03] != [-1.32530458984375000000e+04]
	absolute diff. = 8963.12, relative diff. = 0.676306
	tolerance=0.000976562
long [bitsPerValue]: [14] != [15]
long [section7Length]: [494933] != [628419]
double [codedValues]: 1036800 out of 1036800 different
 max absolute diff. = 8.9631186523437500e+04, relative diff. = 0.676306
	max diff. element 690373: -4.28992724609375000000e+04 -1.32530458984375000000e+05
	tolerance=0.0000000000000000e+00
	values max= [42230.7]  [100240]         min= [-42899.3] [-132530]
```

仅比较头信息

```
$ grib_compare -w count=72 -H gmf.gra.2018010300003.grb2 gmf.gra.2018010300009.grb2 

-- GRIB #37 -- shortName=unknown paramId=0 stepRange=3 levelType=sfc level=0 packingType= gridType=regular_ll --
long [totalLength]: [495109] != [628595]
long [forecastTime]: [3] != [9]
```

`-H`选项不能与`-c`选项一同使用。

#### 版本无关比较

如果两个 GRIB 消息使用不同的版本编码，它们会非常不一致。

```
> grib_compare sp.grib1 sp.grib2
-- GRIB #1 -- shortName=sp paramId=134 stepRange=0 levelType=sfc level=0 packingType=grid_simple gridType=reduced_gg --
long [totalLength]: [4284072] != [4284160]
long [editionNumber]: [1] != [2]
long [section1Length]: [52] != [21] [table2Version] not found in 2nd field [gridDefinition] not found in 2nd field [indicatorOfParameter] not found in 2nd field [indicatorOfTypeOfLevel] not found in 2nd field [yearOfCentury] not found in 2nd field [unitOfTimeRange] not found in 2nd field
[P1] not found in 2nd field
[P2] not found in 2nd field
...
```

使用`-e`选项设置`grib_compare`仅比较两个消息的高层信息。

```
> grib_compare –e sp.grib1 sp.grib2
-- GRIB #1 -- shortName=sp paramId=134 stepRange=0
  levelType=sfc level=0 packingType=grid_simple
  gridType=reduced_gg --
string [param]: [134.128] != [134]
```

上面的两个消息使用两种不同的方式编码同样的信息。仅 MARS 参数不一致。

#### 汇总

当文件中包含多个要素场且某些 key 不相同时，输出汇总结果很有帮助。

```
$ grib_compare -w count=72/73/74 -f gmf.gra.2018010300003.grb2 gmf.gra.2018010300009.grb2 

-- GRIB #37 -- shortName=unknown paramId=0 stepRange=3 levelType=sfc level=0 packingType=grid_jpeg gridType=regular_ll --
long [totalLength]: [495109] != [628595]
long [forecastTime]: [3] != [9]
double [referenceValue]: [-4.28992724609375000000e+03] != [-1.32530458984375000000e+04]
	absolute diff. = 8963.12, relative diff. = 0.676306
	tolerance=0.000976562
long [bitsPerValue]: [14] != [15]
long [section7Length]: [494933] != [628419]
double [codedValues]: 1036800 out of 1036800 different
 max absolute diff. = 8.9631186523437500e+04, relative diff. = 0.676306
	max diff. element 690373: -4.28992724609375000000e+04 -1.32530458984375000000e+05
	tolerance=0.0000000000000000e+00
	values max= [42230.7]  [100240]         min= [-42899.3] [-132530]

-- GRIB #38 -- shortName=sde paramId=3066 stepRange=3 levelType=sfc level=0 packingType=grid_jpeg gridType=regular_ll --
long [totalLength]: [241569] != [241372]
long [forecastTime]: [3] != [9]
long [section7Length]: [241393] != [241196]
double [codedValues]: 159561 out of 1036800 different
 max absolute diff. = 1.5500000000000114e-01, relative diff. = 0.00492956
	max diff. element 161112: 3.14430000000000013927e+01 3.12880000000000002558e+01
	tolerance=0.0000000000000000e+00
	values max= [57.629]  [57.627]         min= [0] [0]

## ERRORS SUMMARY #######
##
## Different number of messages 
## 723 more messages in gmf.gra.2018010300003.grb2 than in gmf.gra.2018010300009.grb2
##
## Summary of different key values 
## totalLength ( 2 different )
## forecastTime ( 2 different )
## referenceValue ( 1 different )
## bitsPerValue ( 1 different )
## section7Length ( 2 different )
## codedValues ( 2 different )
##
## 2 different messages out of 38
```

#### 顺序无关的比较

比较两个包含同样消息但消息顺序不对应的文件会有很多错误。
默认情况下，`grib_compare`假定消息有相同的排列次序。

```
> grib_compare–f –H f1.grib1 f2.grib1
...
## ERRORS SUMMARY #######
##
## Summary of different key values
## indicatorOfParameter( 6 different )
## level ( 7 different )
##
## 10 different messages out of 12
```

比较顺序不同的消息时，可以使用`-r`选项。注意这将花费大量时间。

```
grib_compare –r –f –H f1.grib1 f2.grib1
```

#### 比较数据值

默认会精确比较浮点数。

使用下面的选项设置不同的比较方法：

选项 | 含义
----|----
-A absolute_error | 比较绝对误差
-R key=rel_error,… | 比较某个key的相对误差
-P | 使用 packing error 作为比较误差
-T factor | 指定特定参数 -A -R -P 时，使用给定的 tolerance 乘以一个整数因子的数值作为比较误差。

##### 设置比较误差

比较两个文件的数据显示，在默认绝对比较误差为0的情况下，7个数据中的1个值不一样。

```
> grib_compare–cdata:nf1.grib1 f2.grib1
--GRIB #1 --shortName=2t paramId=167 stepRange=0 levelType=sfclevel=0 packingType=grid_simplegridType=reduced_gg--
double [packedValues]: 1 out of 7 different
max absolute diff. = 2.0000000000000000e+00, relative diff. = 0.4
max diff. element 2: 3.00000000000000000000e+00 5.00000000000000000000e+00
tolerance=0.0000000000000000e+00 packingError: [0.0625005] [0.0625005]
values max= [70] [70] min= [1] [1]
```

将绝对误差设为 2.0，比较就会显示两者一样。

```
> grib_compare–A 2.0–c data:nf1.grib1 f2.grib1
```

可以为每个 key 设置相对比较误差。

```
> grib_compare–cdata:nf1.grib1 f2.grib1
--GRIB #1 --shortName=2t paramId=167 stepRange=0 levelType=sfclevel=0 packingType=grid_simplegridType=reduced_gg--
double [packedValues]: 1 out of 7 different
max absolute diff. = 2.0000000000000000e+00, relative diff. = 0.4
max diff. element 2: 3.00000000000000000000e+00 5.00000000000000000000e+00
tolerance=0.0000000000000000e+00 packingError: [0.0625005] [0.0625005]
values max= [70] [70] min= [1] [1]
values max= [70] [70] min= [1] [1]
```

为 `packagedValues` 设置 0.4 的相对比较误差。

```
> grib_compare–R packedValues=0.4–c data:nf1.grib1 f2.grib1
```

因为相对比较误差大于相对误差，所以比较结果显示两者一致。

不同的打包经度也会导致不同的数据值。

```
> grib_compare–cdata:nf1.grib1 f3.grib1
--GRIB #1 --shortName=2t paramId=167 stepRange=0 levelType=sfclevel=0 packingType=grid_simplegridType=reduced_gg--
double [packedValues]: 1 out of 7 different
max absolute diff. = 5.0000000000000000e-01, relative diff. = 0.166667
max diff. element 1: 2.50000000000000000000e+00 3.00000000000000000000e+00
tolerance=0.0000000000000000e+00 packingError: [0.0625005] [0.5]
values max= [70] [70] min= [1] [1]
values max= [70] [70] min= [1] [1]
```

可以使用 packing error 作为比较误差。

```
> grib_compare–P–c data:nf1.grib1 f3.grib1
```

比较结果显示两者一致，是因为最大的绝对误差在两个 packing error 的最大值之内，表示仅有打包经度被改变了。


### 练习：使用 `grib_compare`
 
