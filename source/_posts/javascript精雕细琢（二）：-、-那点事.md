---
title: javascript精雕细琢（二）：++、--那点事
date: 2018-12-28 20:23:52
cateories:
  - 前端
  - JS
tags:
  - JS
---

### 引言

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对于接触 JS 时间不长的前端来说，刚开始要实现诸如轮播图，选项卡等小模块时，肯定会用到 index 索引来实现。而这其中很大一部分又会使用++（自加）、--（自减），所以清楚的知道++（自加）、--（自减）的计算模式，可以在运用时减少错误。本文将从数学运算、变量引用、数据转换 3 个方面，对++（自加）、--（自减）进行一个详细的说明，力求达到一个在通读全文后，明确++（自加）、--（自减）应用方式的效果。
<!--more-->
### ++和--在数学运算中的计算规则

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;老规矩，先上代码：

```
        let a = 1,
            b = 1;
        let num1 = a++ + b++, //num1执行前a=1,b=1，num1执行后a=2,b=2
            num2 = a++ + ++b, //num2执行前a=2,b=2，num2执行完后a=3,b=3
            num3 = ++a + b++, //num3执行前a=3,b=3，num3执行完后a=4,b=4
            num4 = ++a + ++b; //num4执行前a=4,b=4，num4执行完后a=5,b=5
        console.log(num1);    //结果为2，步骤分解: a + b（1 + 1）=> a++(2) => b++(2) => a=2,b=2;
        console.log(num2);　　//结果为5，步骤分解: a + (++b)（2 + 3） => a++(3) => a=3,b=3;
        console.log(num3);　　//结果为7，步骤分解: (++a) + b（4 + 3） => b++(4) => a=4,b=4;
        console.log(num4);　　//结果为10，步骤分解: (++a) + (++b)（5 + 5） => a=5,b=5;
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过以上代码，可以总结出++和--在数学运算出的计算规则如下（加减乘除没区别）：

1. 当**++、--写在变量前**时，数学计算时，引用的是**++、--未执行**的变量值；
2. 当**++、--写在变量后**时，数学计算时，引用的是**++、--执行后**的变量值；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过以上的理解，就不难看出，**num1**得到的求和结果实际上就是 a + b，而**num2**实际上是 a + (++b)，**num3**是(++a) + b，**num4**是(++a) + (++b)，但是求和归求和，++、--只是没在求和结果中执行，实际上变量的++、--都是执行了的，这一点要明确。

### ++和--在变量引用时的计算规则

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;上代码：

```
        let num1 = 0,
            num2 = 0;
        let arr = [0, 1, 2, 3, 4, 5, 6];
        console.log(arr[num1++]); // 0  num1=1
        console.log(arr[++num2])  // 1  num2=1
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过以上代码，不难看出，当在变量引用时执行++、--，与数学运算有相似的规律。总结起来就是一句话——**++、--在变量前，引用++、--执行后的变量值；++、--在变量后，引用当前变量值（即执行++、--前的值）**。

### ++和--的数据转换应用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;上代码：

```
        let a = true;
        let b = false;
        let c = "a";
        console.log(++a); //2
        console.log(b++); //0
        console.log(c++); //NaN
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;总结——对非 Number 类型的数据，应用++、--，相当于执行了 Number()方法。如果转换后是 number 类型，则按上边的规则执行++和--，如果转换后不是 number 类型，则不执行++和--。输出 NaN。不能直接进行类似++true 的操作，必须通过声明变量值为 true 或 false，然后再++、--。

### 结语

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;越是简单常用的技巧，可能越容易被人忘记它的其他方面。保持一颗蓝翔毕业的心，挖掘所有东西~
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本文同时发布于我的博客园https://www.cnblogs.com/keepStudying/p/9743457.html
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;转载请注明出处
