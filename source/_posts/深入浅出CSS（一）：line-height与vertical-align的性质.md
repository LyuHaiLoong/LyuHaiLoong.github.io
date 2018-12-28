---
title: 深入浅出CSS（一）：line-height与vertical-align的性质
date: 2018-12-26 15:31:17
categories: 
  - 前端
  - CSS
tags: 
  - CSS
---
### 引言
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;学会CSS简单，学好CSS太难。CSS带给我的压迫感，甚于JS。虽然大的趋势是将CSS开发平台化，专注于业务逻辑。但是CSS本身可以挖掘的地方其实有很多，而那些趣味性十足（但实用性不大）的demo也确实带给我很多乐趣。所以在决定写《案例分享》系列和《javascript精雕细琢》系列时，《深入浅出CSS》系列我也早就心心念念。本系列会就CSS中一些细节以及奇技淫巧进行分享，志不在提高技术，在提升前端学习乐趣~
<!--more-->
> 测试代码
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vertical与line-height测试</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        div {
            border: 1px solid #000;
            width:300px;
            height: 200px;
            line-height: 200px;
            text-align: center;
        }
        p {
            display: inline-block;
            vertical-align: middle;
        img {
            vertical-align: middle;
            border: 1px solid #000;
        }
    </style>
</head>
<body>
    <div>
        <p class="p">我啊</p>
        <img src="http://mat1.gtimg.com/www/qq2018/imgs/qq_logo_2018x2.png" alt="图片加载失败">
    </div>
</body>
</html>
```
### line-height的性质
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;line-height在CSS中的定义为行高，具体计算方法如图所示：
![](https://upload-images.jianshu.io/upload_images/4473919-4b730589164c4674.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由图中可以看出，在**不设置元素高度以及padding、border、margin均为0的情况下，元素本身的高度就是它的line-height值**，而文本与文本之间的间隙就是**(line-height - font-size)**。所以此时通过**设置line-height值，就等于在设置元素高度**（元素高度不固定且padding、border、margin不为0时，高度由padding、border、margin及行高共同控制）。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;那么，在了解了line-height对元素高度的控制原理之后，就不难理解文本元素垂直居中的原理，令height=line-height，或者不设置height（默认line-height），都是通过让height = (line-height - font-size)/2 + font-size +  (line-height - font-size)/2，然后文本居中显示。如果在一个<div>中嵌套一个<p>分别把p元素display: inline-block、display:line，则为下图展示的原理：
1. display：block(默认);
![](https://upload-images.jianshu.io/upload_images/4473919-5076fd7a90089ebb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**p元素与div元素等高且等宽**

2. display：inline-block;
![](https://upload-images.jianshu.io/upload_images/4473919-48ec99a87de6f270.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**p元素与div元素等高，与内容等宽**

3. display:：inline;
![](https://upload-images.jianshu.io/upload_images/4473919-9943a469c184d386.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**p元素与内容等高且等宽**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;上面三张图片，分别展示了p在三种元素类型下的不同表现形式，从中可以理解inline-block的工作机理（子元素继承），实际上是**通过控制高度，使内容（inline）居中，而不是元素本身居中**。那么试想一下，如果在p元素前插入一段纯文本，或者P元素是多行文本，那么div中显示情况又会如何？图中的div的height是固定的100px，那如果不设置height的值，又会如何显示？？请各位自己动手进行验证~
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;另外，**从第一张及第二张图中还可以看出一个小问题，p元素的边框溢出了div的盒子**。通过父元素div设置固定值height与不设置固定值时，确定子元素p的height与父元素div的height的计算关系。希望各位同仁能够找到自己的答案~

### vertical-align的性质
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vertical-align从字面上理解就是垂直排列，这货有一大堆属性可写。不要觉得这货平常不怎么用，实际上它无处不在。因为浏览器默认设置中，inline与inline-block的对齐方式，就是baseline基线对齐，想要理解各种属性的对齐方式，我们先看一下各种属性的意义，如图所示：
![](https://upload-images.jianshu.io/upload_images/4473919-e21ae5c85d2bfcd5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这个图实在是难画，从水印也可以看出来，我是直接从网上粘过来的，如有雷同，我选择删除（哈哈，skr）
1. vertical-align各种属性：
　　　　　　vertical-align: baseline;
　　　　　　vertical-align: sub;
　　　　　　vertical-align: super;
　　　　　　vertical-align: text-top;
　　　　　　vertical-align: text-bottom;
　　　　　　vertical-align: middle;
　　　　　　vertical-align: top;
　　　　　　vertical-align: bottom;

2. vertical-align对齐方式：
  1）浏览器默认baseline对齐；
  2）设置了middle/baseline的情况下，元素自身middle/baseline会和行内其他元素的baseline对齐（一个inline-block元素，如果里面没有inline内联元素，或者overflow不是visible，则该元素的基线就是其margin底边缘，否则，其基线就是元素里面最后一行内联元素的基线——**摘自张鑫旭《CSS深入理解vertical-align和line-height的基友关系》**）；
3）设置了top/bottm情况下，自己的top/bottom会和行内其余元素中最高的top/最低的bottom对齐；
　　
3. 其余补充：　　　　 
1）verti-align只可用于inline或inline-block；　　　　
2）vertical-align必须在目标元素中声明，对其他元素不产生影响；
3）vertical-align如果设置%属性的话，以line-height值为计算基数；
4）vertical-align默认属性为baseline；

### line-height与vertical-align的搭配使用，元素垂直居中
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在第一部分最后我说到了，通过line-height达到的居中效果，实际上是子元素继承了父元素的height与line-height设置，**本质上是内容的居中，而非元素本身的居中**。试想一下，如果div中嵌入的是img而非p元素，如何在一个height设置了固定值的div中，将img垂直居中显示？？（这里的固定值，包括从父元素按%计算而来的固定值。未设置固定高度值的，高度以内容为准，可通过padding-top/bottom或line-height设置居中）
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这就涉及到了line-height与vertical-align的配合使用。

在实际操作前，首先回忆一下刚才的内容：
1. line-height通过控制元素高度与其内容字体高度的差值均分，实现垂直居中；
2. vertical-align默认baseline对齐；
3. vertical-align在目标元素中声明，且目标元素一定是inline或inline-block属性的元素；

Duang~准备就绪。现在开始说明操作过程，同样以图片直观的展现：
1. 当给div一个**固定的height值**，在**不设置line-height及vertical-align**时，由于设置了* {margin: 0;padding: 0;}，图片是紧贴顶端的。
![](https://upload-images.jianshu.io/upload_images/4473919-eb159024457c5674.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 让我们来加一段文字，让理解更直观。此时，我们就能看到**vertical-align的默认属性baseline对齐**。**图片由于不是文字，它的基线就是它的margin底（此图中margin为0），即border-bottom**。
![](https://upload-images.jianshu.io/upload_images/4473919-47656f4e246628ce.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 下面让我们给img属性增加**vertical-align: middle;**，可以看出文本与图片**似乎居中对齐了**，但是文本呢，有一点emmmmmm，不那么居中！因为**图片是以自身的middle line与文本的baseline对齐的**。所以实际的中点要在对齐线的上方（详情上翻至vertical-align的对齐方式）。
![](https://upload-images.jianshu.io/upload_images/4473919-dacf683641c4d742.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 设置div的line-height = height值，去除文字的border（防止子元素p继承父元素div的高度后，增加border溢出）。此时文本垂直居中显示，按照上一步的解释，不难看出，图片因为与文本并不是真正的居中对齐，所以实际上它在div中并没有真正的垂直居中。
![](https://upload-images.jianshu.io/upload_images/4473919-4115996dba610e9b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 解决方案
1）那么怎么才能让图片真正的垂直居中呢？脑子转的快的朋友肯定已经想到了，可以把**p元素也声明vertical-align: middle;**啊。这确实是个好方法，但是！如果文本设置了vertical-align后，它自身也并不是垂直居中了的话，那这种方法就不成立了。而且很不幸的是，情况就是这样（控制台可查看p元素高度，已经溢出div盒子）。那么为什么会出现这种情况呢。那就要我们回想一下，如果**行内只有一个p元素或者一个img元素的话，它又是跟谁对齐的**？
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;要理解行内元素对齐，这里就必须提出一个假设，**假设block元素内，存在一个empty元素**。**在block元素内只嵌套了一个inline或inline-block元素时，作为规则的参照物**。而且empty元素虽然看不见，但要**假设它里边有一个继承自父元素font-size的文本**，**这样它才具有正常的baseline、middle line及top、bottom等**。理解了这一假设，就不难理解为什么p元素设置vertical-align: middle;后，图片仍然不是垂直居中。因为**p元素中线与empty元素的基线对齐了**。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;那么有的朋友又会提出问题了，既然图片与文本的基线对齐，文本下移了，图片为什么没有跟着文本的基线一起下移呢？那是因为**基线始终都只有一条，那就是empty元素的基线**。这也就解释了，**为什么p元素设置了vertical-align: middle;后，只有p元素下移，而img元素纹丝不动**。因为**p元素与empty元素由baseline对齐baseline变为了middle line对齐baseline，造成了下移，而img元素始终都与empty元素对齐，所以纹丝不动**。
2）俗话说柳暗花明又一村，一条路走不通，必然有另一条路。既然改变p元素的vertical-align行不通，那就尝试其他方法，比如**改变基线的位置**。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这就需要另一个属性**font-size**，既然**基线由行内empty元素的字体大小决定**，那如果我把**font-size设置成0**，那不是**所有线全都汇聚成了一条**，那么**文字的居中显示，不就变成了这条汇聚线的居中显示？这样的话，跟基线对齐岂不是就成了居中对齐**？简直美滋滋~
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这样又有朋友说了，这样**文字就消失不见了**，一个问题解决了，又出来个新问题，这什么时候是个头啊？？？别急，这个方法简单，那就是**给p元素，单独加一个font-size声明**，这样的话，p元素的内容就出来。我们的目的只是改变empty元素的继承自父元素div的字体大小，完美解决~
![](https://upload-images.jianshu.io/upload_images/4473919-5cf3e3ccb5042429.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 结语
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;至此，line-height与vertical-align的性质及组合用法就介绍完毕，本文**主要参考了张鑫旭大神的《CSS深入理解vertical-align和line-height的基友关系》**，如有雷同，不用怀疑，就是受了他的启发~
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;文中若有错误及不妥之处，欢迎指正~


