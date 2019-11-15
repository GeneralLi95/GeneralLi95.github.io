---
title: Python 通过sys.argv控制台输入参数
date: 2019-11-15 16:41:44
tags:
  -
categories:
  -
mathjax:
---

通过python编写的大型代码往往需要在服务器中运行，而服务器中往往不会安装IDE，所以只能在控制台通过`python yourcode.py`使程序执行，如果程序中有参数需要调整，通过文本编辑器到程序里去调整参数十分麻烦，并且不能保证代码的一致性。

所以就需要利用，控制台输入参数，参考代码

```
import sys

if __name__ == "__main__":
    if len(sys.argv) >= 3:
        a = sys.argv[1]
        b = sys.argv[2]
    elif len(sys.argv) == 2:
        a = sys.argv[1]
        b = input("请输入参数b ")
    elif len(sys.argv) == 1:
        a = input("请输入参数a ")
        b = input("请输入参数b ")

    print(a + b)
    print(sys.argv)
```


我们在控制台执行这个程序

```
YaodeMacBook-Pro:learnpy yaoli$ python3 raw_input_test.py 2 3
# 输出
23
['raw_input_test.py', '2', '3']
```

通过 `python3 raw_input_test.py 2 3` 命令执行了 `raw_input_test.py` 这个程序，其中2,3分别是 `sys.argv[1]` 和 `sys.argv[2]`，为什么不是 `sys.argv[0]`我们通过`print(sys.argv)` 发现其实 `sys.argv` 就是一个list，它的0号元素是这个程序的程序名。

通过 `print(a+b)` 得出 `23`，我们发现这些参数的输入都是作为字符串变量导入的。

同时以上参考代码，还具有如果输入参数不够两个，自动要求你继续输入的功能。
