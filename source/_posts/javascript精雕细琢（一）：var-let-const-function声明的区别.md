---
title: javascript精雕细琢（一）：var let const function声明的区别
date: 2018-12-25 11:54:21
categories:
  - 前端
  - JS
tags:
  - JS
---
### 引言

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在学习 javascript 的过程中，变量是无时无刻不在使用的。那么相对应的，变量声明方法也如是。变量是由自己决定，但变量声明方法是早已经定义好的。那么在使用变量之前，了解变量声明方法，就变得尤为重要。在 ES6 推出之前，最常用的声明变量方法就是 var。但是由于 var 自身的缺陷，ES6 推出了 let 和 const 替代 var。虽然修正了 var 的缺陷，但不能改变的，是之前已经用 var 写了多少年的项目，这些项目是无法随着 var 的被取代而轻易更改的。所以仍存在着使用 var 的公司和项目，这也使得了解 var、let、const 的区别变得有必要。
<!--more-->
### var

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在说明 var 并看代码之前，我们先统一思路，将变量的声明及使用过程分为：**创建 → 初始化 → 赋值 → 修改**四步。

> **首先，来看 var 的声明及使用：**

```
function test() {
　　/*
   　　预解析，初始化与赋值分离
       var a = undefined // 创建变量a，并初始化为undefined。此时变量已经存在与环境中
   */
        　　
   console.log(a) //undefined

   var a = 0; //赋值为0
   console.log(a); // 0

   a = 1; //可以修改变量值
   console.log(a) // 1
}

test();
```

以上代码可以看出，var 在它的执行环境中的声明及操作过程为：

**（ 创建 → 初始化 ）→ 赋值 → 修改**

1. 预解析：在环境最顶端，创建变量，并初始化为 undefined—— 变量提升；
2. 为变量赋值；
3. 对变量进行操作，可以在后续操作中对变量值进行修改；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过 console.log 的打印结果，我们可以清晰的认识到一点——var 的初始化与赋值是分离的，而且初始化的过程优先于执行环境中的所有操作。这就是为什么在 var 声明赋值前 console.log 变量，会打印出 undefined，而不是报错的原因。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;var 声明的初始化先于赋值的现象，就叫做**变量提升**。

> **然后着重说一下变量提升**

1. if 判断中的变量提升

```
function test() {
　　/*
   　　预解析
      虽然if判断没有执行，但var的变量提升已经发生，此时执行环境中，已经存在变量a，值为undefined；
   */
        　　
   console.log(a) // undefined，变量提升
        　　
   if (false) {
   　　var a = 1;
       console.log(a) //不执行
   }

   console.log(a); //undefined，变量未赋值

   a = 1; //注意，此时因为a的变量提升，未加声明符号的赋值，并没有提升到全局环境中
   console.log(a); // 1
}

test();

console.log(a); // 报错，因为if中a的变量提升，a = 1的赋值并没有存在与全局中。如果注释掉if判断中的内容，a = 1因为没加变量声明符号，相当于在与全局中声明，那么最后的console.log(a)将打印1
```

<br>
2.for循环中的变量提升
```
function test() {
　　/*
   　　预解析
      虽然if判断没有执行，但var的变量提升已经发生，此时执行环境中，已经存在变量a，值为undefined；
   */
        　　　
   console.log(a) // undefined，变量提升
        　　
   console.log(i); //undefined，变量提升
   for (var i = 0;i < 0;i++) {
   　　var a = 1;
      console.log(a) //未执行
   }
   console.log(i); //1，for中的初始化语句已经执行

console.log(a); //undefined，变量未赋值

a = 1;
console.log(a);  
}

test();
console.log(a); //报错，变量不存在

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;从上边代码中可以看出，**当var声明存在于if和for循环中时，不管赋值有没有执行，创建及初始化的过程都已经提升到了执行环境中**。

> **最后说明var的一个缺陷：**
```

function test() {
　 var a = 0;
var a = 1;
var a = 2;
  
 console.log(a); // 2
}

test();

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由以上代码可以看出，var声明一个变量后，可以**无限次的以同一个变量名不断的重复 创建→初始化→赋值**，这跟直接修改变量值的结果是一样的。但是实际操作中不会有人通过这种方式操作变量，而且如果项目很大，很难保证不出现给不同变量声明同一个变量名的情况，很容易出现错误。

### let
> **首先，来看var的声明及使用：**
```

function test() {
/_
预解析，没有变量提升，啥也没有。创建、初始化与赋值同时进行
_/
  
　 console.log(a); //报错，变量仍未创建

    let a = 0; // 创建变量a，初始化并赋值为0.不赋值的话，则为undefined;
    console.log(a); // 0

    a = 2; // 可以修改变量值
    console.log(a); // 2

}

test();

```
同样，先分析过程：

**（ 创建→初始化→赋值 ）→修改**

1. 预解析，啥也没有；~~在环境最顶端，创建变量，并初始化为undefined—— 变量提升；~~
2. 创建变量、初始化并赋值；
3. 对变量进行操作，可以在后续操作中对变量值进行修改；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过上方代码，我们可以看出let声明变量时，**（ 创建→初始化→赋值 ）是在一步完成的，不存在变量提升的现象**。所以在let声明前console.log(a)，报错a is not defined，因为此时a还没有被创建。而let初始化前的执行区域就叫做**暂存死区**。

> **然后，来看let在重复声明时的表现：**
```

function test() {
　 let a = 1;
let a = 2;
let a = 3;

console.log(a); // 报错
}

test();

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;以上说明了let区别于var的另一个特性——**变量的唯一性**。**同一个变量名，不能在let中重复使用**，所以执行上方代码操作的结果，就是报错**Identifier 'a' has already been declared**；

> **最后，let与var最大的区别——块级作用域**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;首先说明，**块级作用域**的概念——简单理解，**{}一个大括号就是一个代码块，一个单独的执行环境**。那么它理应不受外部影响（如果不是刻意为之的话，它也不应该影响外部环境）。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;但是在let之前，JS中的变量声明是**没有块级作用域**的属性的。这其中最典型的案例，就是**for循环中的var声明i**。
```

function a() {
　　 for (var i = 1;i < 5;i++) {
　　 console.log(i);
for( var i = 10;i < 20;i++) {
　　 console.log(i);
}
}
}
a(); //只执行外部循环一次，因为内部循环由于 var 声明的缘故，执行过后 i 值已经为 11，外部循环判断后，将不再执行

function testA() {
　　/_
　 预解析
创建 i 并赋值为 undefined
注意，此时实际上是两个 i 的变量提升
_/
console.log(i); // undefined

var i = 0; // 赋值 i = 0
console.log(i); // 0;

{
　　 console.log(i); // 0，访问第一个 i
var i = 10 // 赋值 i = 10，覆盖第一个 i
console.log(i); // 10，访问第二个 i
}

console.log(i); // 10，访问第二个 i，i 已经被覆盖
}
testA();

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;以上代码，通过testA函数，侧面解释了一下a函数中的行为原因。这一行为模式，充分暴露了var声明的缺陷。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**然后再来看let声明中的块级作用域：**
```

function b() {
　　 for (let i = 0;i < 5;i++) {
　　 console.log(i);
for(let i = 10;i < 20;i++) {
　　 console.log(i);
}
}
}
b(); //因为块级作用域的存在，两个循环的 i 互不影响，所有循环次数都会被执行。实际使用时，建议还是不要都用同一变量名

function testB() { 　　
/_预解析啥也没有 _/
console.log(i); // 报错，暂存死区

let i = 0;
// 赋值 i = 0
console.log(i); // 0;

{
console.log(i); // 报错，暂存死区，因为块中又声明了变量 i。如果块中没有 let i 的话，则按作用域链向上查找，打印外部 i 值 0
let i = 10 // 赋值 i = 10，不覆盖第一个 i
console.log(i); // 10，访问第二个 i
}

console.log(i); // 0，访问第一个 i，两个 i 相互不影响  
}
testB();

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;以上代码，通过testB函数，侧面解释了一下b函数中的行为原因。这一行为模式，体现了let生命中块级作用域的存在，并暴露出了let与var的最大区别。

### const
```

function test() {
　　/_
　　预解析啥也没有，创建、初始化与赋值同时进行
_/
console.log(a); // 报错，变量仍未创建

const a = {
　　 name: "Lyu" // 创建对象 a，并赋值为一个对象地址
}
console.log(a); //{name: "Lyu"}
console.log(a.name); // Lyu

a = 1; //报错，常量值不可修改

a.name = "Jack";
　　 console.log(a); // {name: "Jack"}
console.log(a.name); // Jack
}

test();

```
分析过程：

**（ 创建→初始化→赋值 ）→修改**

1. 预解析，啥也没有；~~预解析在环境最顶端，创建变量，并初始化为undefined—— 变量提升；~~
2. 创建变量、初始化并赋值。必须赋值，不赋值会报错；
3. 对变量进行操作，可以在后续操作中对变量值进行修改，不可以对变量进行修改，但是可以对变量的属性进行修改；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;为了说明const的特性，**特意声明了一个对象**。在理解了var和let的过程之后，再来看const的整个过程，会发现**在（ 创建→初始化→赋值 ）的过程中，const和let是没有区别的**。唯一的区别在于**→修改**。如果执行了上方的代码，在**a = 1**那步会报错**Assignment to constant variable**。其中的constant就是const的英文全拼，它的意思的**不变的、恒定的、恒量**。那么从字面上就能理解，通过const声明的变量，**是一个恒定值，即无法更改的值**。所以当通过**a = 1**试图修改a的值时，报错。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;虽然**a本身的值**无法修改，但是a为对象，值为地址，所以**a的属性是可以修改的**。从代码最后一步可以看出。a.name的值成功修改为"Jack"。这就是const 的第二个特性。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;同let一样的，const的第三个特性也是**变量的唯一性**，不再过多阐述。

### function
> **首先，来看function的声明及使用：**　
```

/_
　　 funtcion test() {...}
预解析，创建、初始化、赋值三位一体
此时函数已经完成变量声明的所有操作，在执行环境的任何位置都可以调用函数  
_/

test(); //调用函数，只要函数确实存在并可用，那么在执行环境任何位置都可以调用

function test() {...}

```
分析过程：

**（ 创建→初始化→赋值 ）→执行/修改**

1. 预解析，在环境最顶端，创建函数，初始化并赋值为函数定义；
2. 执行函数，无论函数在何位置，只要可用，就可以调用；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;function是专门用于函数声明的方法，由于函数的复杂性，以及利用性。function声明的函数，会在整个环境变量最顶端完成**创建、初始化、赋值三位一体**的操作。这样一来，不管在何处声明了函数，可以在任何地方调用函数方法。这是比较合乎常理的性质。

> **然后，function同var一样，同样存在变量声明的特性：**

1. if判断中的变量提升
```

function test() {
　　/_
　　预解析
　　虽然 if 判断没有执行，但 function 的变量提升已经发生，此时执行环境中，已经存在变量 a，值为 undefined；
_/
　　
console.log(a) // undefined，变量提升
　　
if (false) {
　　 function a(){}; // 变量提升
console.log(a) //不执行
}

console.log(a); //undefined，变量未赋值

a = 1; //注意，此时因为 a 的变量提升，未加声明符号的赋值，并没有提升到全局环境中
console.log(a); // 1  
}

test();

console.log(a); // 报错，因为 if 中 a 的变量提升，a = 1 的赋值并没有存在与全局中。如果注释掉 if 判断中的内容，a = 1 因为没加变量声明符号，相当于在与全局中声明，那么最后的 console.log(a)将打印 1

```
<br>

2. for循环中的变量提升
```

function test() {
　　/_
　　预解析
虽然 if 判断没有执行，但 function 的变量提升已经发生，此时执行环境中，已经存在变量 a，值为 undefined；
_/
　　　
console.log(a) // undefined，变量提升
　　
console.log(i); //报错，暂存死区
for (let i = 0;i < 0;i++) {
　　 function a() {}; //变量提升
console.log(a) //未执行
}
console.log(i); //报错，let 不存在变量提升

console.log(a); //undefined，变量未赋值

a = 1;
console.log(a);  
}

test();
console.log(a); //报错，变量不存在

```

> **最后，function同var一样，它也可以对同一变量重复声明，而且后边的函数定义会覆盖前边的函数定义：**

```

test(); // 2

function test() {
　　 console.log(1);　　
}

function test() {
　　 console.log(2);
}

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;结果打印2，说明前边声明的函数方法被后边所覆盖。

### 结语
综上所述，可以总结为如下几点：

1.var与let、const的区别在于**变量提升**，以及**变量的唯一性**；
2.const与let的区别，**除了变量值不能修改，其他性质一样**；
3.function由于其自身的需要，**创建→初始化→赋值三位一体**，在环境最顶端完成；也正因为这种性质，**函数声明的函数可以在任何位置被调用**；
4.如果可以，尽量**使用let、const代替var**；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本文仅仅简单罗列了在函数声明及操作方面，var、let、const、function的区别，意在为初学者理清概念偏差，少走弯路，并通过理解学习，早日跨入JS门槛。

**以上，如有错误或是纰漏，欢迎批评指正~**
