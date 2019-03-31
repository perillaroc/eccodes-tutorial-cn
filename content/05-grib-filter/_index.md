---
title: 5. Filter
weight: 5
---

> 译者注：如何体现高超的技术，设计一种宏语言吧。

`grib_filter` 是 ecCodes 高级命令行工具。

该命令逐个处理输入文件中的所有消息，对每个消息应用由用户定义的一组规则。

这些规则由 ecCodes 提供的宏语言定义。注意该宏语言不是完整的编程语言，不能完全替代 ecCodes 提供的 C、Python、Fortran 接口。`grib_filter` 和 `bufr_filter` 的宏语言语法相同。

## 功能

`grib_filter` 可以实现如下的功能：

- 通过 key 访问消息中的数据
- 打印消息内容
- 设置消息的数值
- 使用控制结构（`if`，`switch`） 
- 将消息写入磁盘

## 用法

```
grib_filter [-o out_file] rules_file in_file1 in_file2 …
```

输入文件中的每个要素场都被处理，应用 `rules_file` 中包含的规则。

GRIB 消息仅在执行 `write` 指令时才写入到输出文件。请将 `-o out_file` 放到其他参数前。

`rules_file` 中的说明必须以分号 `;` 结尾。

`rules_file` 中的语法错误会随着行号一同报告。例如对于下面的规则（第二行结尾缺少分号，比较常见的错误）：

```
$ cat s.filter 
print "[centre:s]";
print "[paramId]"
```

会输出如下的错误消息：

```
$ grib_filter s.filter levels.grib2 
ECCODES ERROR   :  grib_parser: syntax error at line 2 of (null)
ECCODES ERROR   :  ecCodes Version: 2.6.0
s.filter: error unable to create action
```

## print 语句

```py
print "some text"; # this is a comment
print "some text [key]";
```

向标准输出流中打印信息。

检索中括号中 key 的键值。

如果在消息中没找到某个 key，则该 key 的值会显示为 `undef`。

可以设置 key 的格式：

- `[key]` -> 原始格式
- `[key:i]` -> 整型
- `[key:s]` -> 字符串
- `[key:d]` -> 双精度浮点型
- `[key!c%^F'S']` -> 数组：`c` -> columns `F` -> format(C Style) `S` -> separator

```py
print ("filename") "some text [key]";
```

### 示例1：使用 print

```py
print "ed = [edition] centre is [centre:s] = [centre:i]";
```

执行命令

```
$ grib_filter rule01.filter ../data/simple/levels.grib2
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
ed = 2 centre is babj = 38
```

### 示例2：格式化输出

```py
# one column 3 decimal digits
print "[distinctLatitudes!1%3.f]";
```

执行命令

```
$ grib_filter rule02.filter ../data/simple/levels.grib2
 90
 90
 89
 89
 89
 89
 88
 88
 88
 88
 87
 87
...
```

### 示例3：带分隔符的输出

```py
# tree columns 5 decimal digits comma separated
print "[latLonValues!3%.5f',']";
```

执行命令

```
$ grib_filter rule03.filter ../data/simple/t.t639.grib2 | more
90.00000,0.00000,247.10527,
90.00000,0.28125,247.10527,
90.00000,0.56250,247.10527,
90.00000,0.84375,247.10527,
90.00000,1.12500,247.10527,
90.00000,1.40625,247.10527,
...
```

## write 语句

```py
write;
```

将当前消息写入到输出文件中，文件名由命令行的 `-o` 参数定义。

```
grib_filter -o outfile rules_file grib_file
```

如果没有指定 `-o` 参数，则使用默认值 `filter.out`。

```py
write "filename_[key]";
```

将当前消息写入到 `filename_[key]` 文件中，中括号中的 key 将被从消息中检索出来的值替换。

如果两个消息有不同的 `[key]` 值，那么它们会写入不同的文件中。

### 示例4：write 语句

```py
# Creating multiple files
write "[centre]_[typeOfLevel]_[level].grib[edition]";
```

执行命令

```
$ grib_filter rule04.filter ../data/simple/levels.grib2
$ ls *.grib*
babj_isobaricInhPa_1000.grib2
babj_isobaricInhPa_950.grib2
babj_isobaricInhPa_975.grib2
```

## append 语句

```py
append;
```

将当前消息追加到输出文件中，输出文件由命令行参数 `-o` 指定。

```
grib_filter -o outfile rules_file grib_file
```

如果没有指定 `-o` 参数，则使用默认值 `filter.out`。

```py
append "filename_[key]";
```

将当前消息追加到 `filename_[key]` 文件中，中括号中的 key 将被从消息中检索出来的值替换。

如果文件不存在，则创建该文件。

如果两个文件的 `[key]` 值不同，那么它们将被追加到不同的文件中。

### 示例5：append 语句

```py
append;
```

执行命令

```
$ grib_count output.grib2
5
$ grib_filter -o output.grib2 rule05.filter babj_isobaricInhPa_950.grib2
$ grib_count output.grib2
10
```

## 设置键值

```py
set key1 = key2 ; # set key1 to the value of key2
set key = {val1,val2,val3,val4} ;# set an array key
set key = "string" ;# set key to a string
set key = expression ; # set key to an expression
set key = MISSING ; # set value of key to missing
```

表达式操作符

- `==`：等于
- `!=`：不等于
- `is`：字符串相等
- `||`：或
- `&&`：与
- `!`：非
- `* / + -`：算术运算
- `( )`

### 示例6：设置键值

下面的规则将实现 GRIB 2 到 GRIB 1 的转码。

```py
set edition = 1;
write "[file][edition]";
```

执行命令

```
$ grib_filter rule06.filter  t.t639.grib2

$ grib_ls t.t639.grib21
t.t639.grib21
edition      centre       typeOfLevel  level        dataDate     stepRange    shortName    packingType  gridType
1            babj         isobaricInhPa  1000         20180113     0            t            grid_simple  regular_ll
1 of 1 messages in t.t639.grib21

1 of 1 total messages in 1 files
```

### 示例7：设置数组 key

```py
set values = {12.2, 14.8, 13.7, 72.3};
print "values = { [values] }";
write "[file].[edition]";
```

执行命令

```
$ grib_filter rule07.filter t.t639.grib2
values = { 12.2 14.8 13.7 72.3 }
```

## 临时 key

```py
transient key1 = key2;
```

定义一个新的 key1，将 key2 的值赋给 key1。

```py
transient key1 = "string";
```

```py
transient key1 = expression;
```

表达式操作符与 set 相同：

- `==`：等于
- `!=`：不等于
- `is`：字符串相等
- `||`：或
- `&&`：与
- `!`：非
- `* / + -`：算术运算
- `( )`

### 示例8：临时 key

```py
transient mystep = step + 24;
print "step = [step] mystep = [mystep]";
```

执行命令

```
$ grib_filter rule08.filter t.t639.grib2
step = 0 mystep = 24
```

## if 语句

```py
if ( expression ) { instructions }
if ( expression ) { instructions }
else { instructions }
```

没有 `else if`，需要创建一个新的 `if` 代码块。

表达式操作符：

- `==`：等于
- `!=`：不等于
- `<`：小于，`<=`：小于或等于
- `>`：大于，`>=`：大于或等于
- `is`：字符串相等（`!(key is value)` 表示 `not is`）
- `||`：或
- `&&`：与
- `!`：非
- `* / + -`：算术运算
- `( )`

### 示例9：if 语句

```py
if (shortName is 't') {
    write;
}
```

执行命令

```
$ grib_ls levels.grib2
levels.grib2
edition      centre       date         dataType     gridType     stepRange    typeOfLevel  level        shortName    packingType
2            babj         20180103     an           regular_ll   0            isobaricInhPa  1000         gh           grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  975          gh           grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  950          gh           grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  1000         t            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  975          t            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  950          t            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  1000         u            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  975          u            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  950          u            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  1000         v            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  975          v            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  950          v            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  1000         wz           grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  975          wz           grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  950          wz           grid_jpeg
15 of 15 messages in levels.grib2

15 of 15 total messages in 1 files

$ grib_filter -o out.grib2 rule09.filter levels.grib2

$ grib_ls out.grib2
out.grib2
edition      centre       date         dataType     gridType     stepRange    typeOfLevel  level        shortName    packingType
2            babj         20180103     an           regular_ll   0            isobaricInhPa  1000         t            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  975          t            grid_jpeg
2            babj         20180103     an           regular_ll   0            isobaricInhPa  950          t            grid_jpeg
3 of 3 messages in out.grib2

3 of 3 total messages in 1 files
```

## switch 语句

`if-else` 语句的替代版本。

当需要从多个选项中选择操作时，使用 `switch` 语句会非常方便。

```py
switch(var) {
    case val1:
        # set of actions
        ...
    case val2:
        # set of actions
        ...
    default:
        # default block of actions
}
```

`default`：默认情况，在语句为空时也要写出。

### 示例10：switch 语句

```py
print "processing [paramId] [shortName] [stepType]";
switch (shortName) {
    case "gh":
        print "processing gh";
    case "t":
        print "processing t";
    default:
        print "Unexpected parameter";
}
```

执行命令

```
$ grib_filter ../../eccodes-grib-tutorial-code/chap_05_grib_filter/rule10.filter levels.grib2
processing 156 gh instant
processing gh
processing 156 gh instant
processing gh
processing 156 gh instant
processing gh
processing 130 t instant
processing t
processing 130 t instant
processing t
processing 130 t instant
processing t
processing 131 u instant
Unexpected parameter
processing 131 u instant
Unexpected parameter
processing 131 u instant
Unexpected parameter
processing 132 v instant
Unexpected parameter
processing 132 v instant
Unexpected parameter
processing 132 v instant
Unexpected parameter
processing 260238 wz instant
Unexpected parameter
processing 260238 wz instant
Unexpected parameter
processing 260238 wz instant
Unexpected parameter
```

### 示例11

```py
if(centre is "lfpw" &&
    (indicatorOfParameter== 6 ||
     indicatorOfParameter== 11 ||
     indicatorOfParameter== 8) )
{
    if(step!=0) {
        set typeOfGeneratingProcess=0;
        set typeOfProcessedData=0;
    } else {
        # Other steps
        set typeOfProcessedData=1;
        switch(typeOfLevel) {
            case "hybrid":
                set changeDecimalPrecision=1;
            case "surface":
                set changeDecimalPrecision=2;
            case "isobaricInhPa":
                if(level > 300) {
                    print"level > 300";
                    set level = level*2 + 15;
                } # end if (level > 300)
            default:
                print"Unknown level type!";
        }# end switch (typeOfLevel)
    }# end if (step!=0)
    write;
}# end main if
```


## assert 语句

```py
assert(condition);
```

如果条件为 false，filter 会出错中止。

示例：

```py
# This filter should be run on GRIB edition 1 only;# abort otherwise
assert (edition == 1) ;
...
```

执行命令

```
> grib_filter–o out.grib2 rule.filterx.grib2
ECCODES ERROR : Assertion failure:
binop(access('edition=2'),long(2))
```
