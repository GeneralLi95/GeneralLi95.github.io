---
title: Java基础题目(温度转换、时间换算、信号报告）
date: 2018-05-19 13:42:44
tags:
  - Java
categories:
  - Programming Lauguage
  - Java
catalog: true
---
# Java基础题目
题目主要来自浙江大学翁凯教授的零基础学习Java课程的配套习题。
## 1. 温度转换
**题目内容:**
写一个将华氏温度转换成摄氏温度的程序，转换的公式是：

    °F = (9/5) * °C + 32

其中C表示摄氏温度，F表示华氏温度。
程序的输入是一个整数，表示华氏温度。输出对应的摄氏温度，也是一个整数。
提示，为了把计算结果的浮点数转换成整数，需要使用下面的表达式：
    (int)x;
其中x是要转换的那个浮点数。

注意：除了题目要求的输出，不能输出任何其他内容，比如输入时的提示，输出时的说明等等都不能。这道题目要求转换后的数字，程序就只能输出这个数字，除此之外任何内容都不能输出。

输入格式:
一个整数。

输出格式：
一个整数。

输入样例：
100

输出样例：
37
时间限制：500ms内存限制：32000kb

**代码**

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int a = 0;
        a = in.nextInt();
        float b = 0 ;
        b = (a - 32)*5/9;
        System.out.print((int)b);
    }
}

```
这个题主要考察输入输出的一些基本操作吧。

## 2.时间换算

**题目内容：**
UTC是世界协调时，BJT是北京时间，UTC时间相当于BJT减去8。现在，你的程序要读入一个整数，表示BJT的时和分。整数的个位和十位表示分，百位和千位表示小时。如果小时小于10，则没有千位部分；如果小时是0，则没有百位部分；如果分小于10分，需要保留十位上的0。如1124表示11点24分，而905表示9点5分，36表示0点36分，7表示0点7分。
有效的输入范围是0到2359，即你的程序不可能从测试服务器读到0到2359以外的输入数据。
你的程序要输出这个时间对应的UTC时间，输出的格式和输入的相同，即输出一个整数，表示UTC的时和分。整数的个位和十位表示分，百位和千位表示小时。如果小时小于10，则没有千位部分；如果小时是0，则没有百位部分；如果分小于10分，需要保留十位上的0。
提醒：要小心跨日的换算。

输入格式:
一个整数，表示BJT的时和分。整数的个位和十位表示分，百位和千位表示小时。如果小时小于10，则没有千位部分；如果小时是0，则没有百位部分；如果小时不是0而且分小于10分，需要保留十位上的0。

输出格式：
一个整数，表示UTC的时和分。整数的个位和十位表示分，百位和千位表示小时。如果小时小于10，则没有千位部分；如果小时是0，则没有百位部分；如果小时不是0而且分小于10分，需要保留十位上的0。

输入样例：
933

输出样例：
133
时间限制：500ms内存限制：32000kb

**代码**

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int a = 0;
        a = in.nextInt();
        int b;
        int c;
        String cc ;
        b = a / 100;
        c = a % 100;
        b = b - 8;
        cc = Integer.toString(c);


        if (b==0)
        {
            System.out.print(cc);
            System.exit(0);
        }
        else if (b<0)
        {

            b = b + 24;
        }

        if (c<10){
            cc = "0"+ cc;
        }
        System.out.print(b);
        System.out.print(cc);
    }
}
```
这道题是主要考察if else语句的用法。

## 3. 信号报告
**信号报告**
题目内容：
无线电台的RS制信号报告是由三两个部分组成的：
R(Readability) 信号可辨度即清晰度.
S(Strength)    信号强度即大小.
其中R位于报告第一位，共分5级，用1—5数字表示.
1---Unreadable
2---Barely readable, occasional words distinguishable
3---Readable with considerable difficulty
4---Readable with practically no difficulty
5---Perfectly readable
报告第二位是S，共分九个级别，用1—9中的一位数字表示
1---Faint signals, barely perceptible
2---Very weak signals
3---Weak signals
4---Fair signals
5---Fairly good signals
6---Good signals
7---Moderately strong signals
8---Strong signals
9---Extremely strong signals
现在，你的程序要读入一个信号报告的数字，然后输出对应的含义。如读到59，则输出：
Extremely strong signals, perfectly readable.

输入格式:
一个整数，信号报告。整数的十位部分表示可辨度，个位部分表示强度。输入的整数范围是[11,59]内有效的数字，这个范围外的数字不可能出现在测试数据中。

输出格式：
一句话，表示这个信号报告的意义。按照题目中的文字，先输出表示强度的文字，跟上逗号和空格，然后是表示可辨度的文字，跟上句号。注意可辨度的句子的第一个字母是小写的。注意这里的标点符号都是英文的。

输入样例：
33

输出样例：
Weak signals, readable with considerable difficulty.
时间限制：500ms内存限制：32000kb

**代码**

```Java
import java.util.Scanner;

public class Main {
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int a = 0;
        a = in.nextInt();
        int b;
        int c;
        b = a / 10;
        c = a % 10;

        switch (c)
        {
            case 1:
                System.out.print("Faint signals, barely perceptible");
                break;
            case 2:
                System.out.print("Very weak signals");
                break;
            case 3:
                System.out.print("Weak signals");
                break;
            case 4:
                System.out.print("Fair signals");
                break;
            case 5:
                System.out.print("Fairly good signals");
                break;
            case 6:
                System.out.print("Good signals");
                break;
            case 7:
                System.out.print("Moderately strong signals");
                break;
            case 8:
                System.out.print("Strong signals");
                break;
            case 9:
                System.out.print("Extremely strong signals");
                break;
        }

        System.out.print(", ");

        switch (b)
        {
            case 1:
                System.out.print("unreadable");
                break;
            case 2:
                System.out.print("barely readable, occasional words distinguishable");
                break;
            case 3:
                System.out.print("readable with considerable difficulty");
                break;
            case 4:
                System.out.print("readable with practically no difficulty");
                break;
            case 5:
                System.out.print("perfectly readable");
                break;
        }

        System.out.print(".");
    }
}

```


这道题主要考察switch case多路分支语句的用法。

***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
