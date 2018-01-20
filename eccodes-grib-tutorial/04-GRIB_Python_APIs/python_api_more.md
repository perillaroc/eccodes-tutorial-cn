# Python API: 更多内容

## 异常处理

所有 ecCodes 函数在出错时都会抛出下面的异常：

```py
CodesInternalError
```

所有 GRIB API 函数在出错时都会抛出下面的异常：

```py
GribInternalError
```

上面的异常都包含 C API 的错误信息。

## 高层接口（正在开发）

更高层面，更加 _pythonic_ 的接口。

```py
with GribFile(filename) as grib:
    # Iterate through each message in the file
    for msgin grib:
        # Access a key from each message
        print(msg[key_name])
        # Report message size in bytes
        msg.size()
        # Report keys in message
        msg.keys()
        # Set scalar value
        msg[scalar_key] = 5
        # Array values are set transparently
        msg[array_key] = [1, 2, 3]
        # Messages can be written to file
        with open(testfile, "w") as test:
            msg.write(test)
        # Messages can be cloned from other messages
        msg2 = GribMessage(clone=msg)
```

## 参考

### Python 标准

[http://www.python.org/](http://www.python.org/)

### NumPy

[http://numpy.scipy.org/](http://numpy.scipy.org/)

[http://www.scipy.org/Numpy_Functions_by_Category](http://www.scipy.org/Numpy_Functions_by_Category)

[http://docs.scipy.org/numpy/docs/numpy/](http://docs.scipy.org/numpy/docs/numpy/)

[http://www.scipy.org/NumPy_for_Matlab_Users](http://www.scipy.org/NumPy_for_Matlab_Users)

### SciPy

[http://www.scipy.org/](http://www.scipy.org/)

### Matplotlib & Basemap

[http://matplotlib.sourceforge.net/](http://matplotlib.sourceforge.net/)

[http://matplotlib.org/basemap](http://matplotlib.org/basemap)

### ecCodes

[https://software.ecmwf.int/wiki/display/ECC/ecCodes+Home](https://software.ecmwf.int/wiki/display/ECC/ecCodes+Home)

### Magics

[https://software.ecmwf.int/wiki/display/MAGP](https://software.ecmwf.int/wiki/display/MAGP)
