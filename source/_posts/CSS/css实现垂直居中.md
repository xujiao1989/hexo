---
title: css实现垂直居中
date: 2017-03-28
tags:
  - 前端开发
  - 个人成长
  - CSS
categories: CSS
---


## 1. 方法一（inline/inline-block配合vertical-align）

这种方法应用于子元素都是inline/inline-block level的元素，可以使用vertical-align属性，兼容IE6+ 如果原本元素不是inline level的话，使用display:inline-block兼容到IE8+

```html
 <div class="parent">
       <span class="con">我是内容我是内容我是内容
       </span>
     <img class="con" src="/images/csstest1.png">
     <img class="con" src="/images/csstest1.png">
 </div>
```

```css
 .parent{
    background: #ccc;
    line-height: 2;
    font-size: 30px;
}
.con{
    vertical-align: middle;
}
```
根据上面的例子我们可以得到以下所示的样式

![css垂直居中示例图1](/images/csstest1.PNG)

在这里，span/img都是行内元素，我们可以采用vertical-align实现垂直居中。上面默认情况下，vertical-align是基于基线对其的。


对于上面的例子，我们也可以对span设置display:inline-block属性，设置高宽后，可以做到span中任意内容的居中效果

## 2.方法二（table-cell配合vertical-align）

使用display:tabel-cell，table-cell也是可以使用vertical-align：middle使得元素垂直居中，但是display:table-cell有兼容性问题，兼容IE8+

```html
     <div class="parent">
         <div class="con">这是第二个内容这是</div>
     </div>
```

```css
    .parent{
        background: #ccc;
        width: 200px;
        height: 200px;
        display: table-cell;
        vertical-align: middle;
    }
    .con{
        width:100px;
        height:100px;
        border: 2px solid red;
    }
```
根据上面的例子我们可以得到以下所示的样式

![css垂直居中示例图2](/images/csstest2.PNG)

这里需要注意的是table-cell设置需要设置在父元素上保证其内容垂直居中，如果你希望con中的文字也居中现在，也可以对con添加上`display: table-cell;vertical-align: middle;`。为什么没有line-height。line-height对多行文本的垂直居中实现效果并不是很好

+ 兼容性：IE6/7不兼容display:table-cell。
+ 自适应：子容器无法自适应于父容器，高度无法使用百分比单位，因为根据渲染规则，display:table-cell的元素的包含块是它父级的display:table的元素。溢出：父元素就算给定高度，设置overflow，也不会导致溢出隐藏；在子元素溢出的时候，父容器不能保有自身设置的高度，直接会被撑高。
+ 其他：display:table-cell本身让很多属性无效。

## 3. 方法三：（position:absolute来实现）--不兼容IE6/7

```HTML
    <div class="parent">
        <div class="con">这是第二个内容这是</div>
    </div>
```

利用position：absolute如何保证con始终在parent的中心位置。

```css
    .parent{
        background: #ccc;
        width: 300px;
        height: 300px;
        position: relative;
    }
    .con{
        height:140px;
        width:70%;
        background: yellow;
        position: absolute;
        top:0;
        bottom:0;
        left:0;
        right:0;
        margin: auto;
    }
```
![css垂直居中示例图3](/images/csstest3.PNG)

我们经常使用margin:0 auto使得块级元素水平居中。这里`position: absolute; top:0;bottom:0;left:0;right: 0;`的作用是什么，如果我们把这个删除了，可以发现原生con是水平居中了，在垂直方向上没有生效，这是为什么？？

原理是：
‘top’ + ‘margin-top’ + ‘border-top-width’ + ‘padding-top’ + ‘height’ + ‘padding-bottom’ + ‘border-bottom-width’ + ‘margin-bottom’ + ‘bottom’ = 包含块的高度。


## 4. 子容器绝对定位，top:50%，负margin（兼容IE6+）

要求子元素定高

```HTML
    <div class="parent">
        <div class="con">这是第二个内容这是</div>
    </div>
```

```css
    .parent{
        background: #ccc;
        width: 300px;
        height: 300px;
        position: relative;
    }
    .con{
        width:140px;
        height:140px;
        background: yellow;
        position: absolute;
        top:50%;
        margin-top: -70px;
    }
```

![css垂直居中示例图4](/images/csstest4.PNG)

## 5. 参考文章

+ [张鑫旭的深入理解CSS系列](http://www.imooc.com/u/197450/courses?sort=publish)
+ [子容器垂直居中于父容器的方案](https://segmentfault.com/a/1190000000381042)
+ [子容器超过父容器如何垂直居中](https://segmentfault.com/q/1010000000343188#a-1020000000343394)
---
做一个勤于思考的人
