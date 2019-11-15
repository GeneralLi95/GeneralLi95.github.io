---
title: 详解 Python 3 的 print()
catalog: true
date: 2019-11-15 10:02:06
tags: Python
categories: Python
mathjax:
---

> 从 Python2 到 Python3 一个很大的变化是 print statement 被 print() function 替代
> —— Guido van Rossum

PEP 3105 -- Make print a function

`print()` 函数的定义是


```python
def print(self, *args, sep=' ', end='\n', file=None): # known special case of print
    """
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)

    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
    """
    pass
```

其中
1. sep 参数为分隔，默认是一个空格，如果输出的几个值之间取其他分隔，可以自定义 sep 为什么都没有，或者 `'\t'`
2. end 为输出的最后一个值之后的终止，默认取  `'\n'`即换行，如果想让 `print()` 不换行，可以自定义这个 end.


![15401722413509.jpg](https://upload-images.jianshu.io/upload_images/11400909-b43e36b7ec1ef1e3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##  参考资料
[《What's New In Pyhton 3.0》](https://docs.python.org/3/whatsnew/3.0.html?highlight=print)

[《PEP 3105 -- Make print a function》](https://www.python.org/dev/peps/pep-3105/)
