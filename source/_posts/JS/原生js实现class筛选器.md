---
title: 原生js实现class筛选器
date: 2017-03-30
tags:
  - 前端开发
  - 个人成长
  - JS
categories: JS
---

可以从[can i use](http://caniuse.com/)网站查到getElementsByClassName方法兼容到IE9+（包含IE9），所以我们在前端面试中会经常被问到如何用原生的js去实现这个借口。


## 思考

1. 我希望接口使用的方式如：getElementsByClassName(classname,parentId,tagname); 
    + classname为我们要筛选的class值，数组类型，如["tab","tab-item"]
    + parentId为父元素的id，可选参数
    + tagname为标签名，可选参数
    + 返回一个包含匹配成功的节点的数组
 
2. 尽可能多的使用已经存在的方法
    + getElementsByTagName
    + getElementById

## 思路

1. 获取到符合要求的dom节点（node下的tag），是一个HTML Collection，是一个类数组，length属性缓存下来，查找代价比较大
2. 我们获取到dom节点的className通常是一个string类型的如“tab tab-item tab-active”,但是也可以是"tab tab-active tab-item"
3. 我们使用正则匹配，将className拆成多个className正则，测试tab正则，测试tab-item的正则等
4. dom节点的className进行正则匹配，通过所有匹配的则返回该节点

## 实现和效果

```javascript
    function getElementsByClassName(classname,parentId,tag){
        var parentNode = parentId && document.getElementById(parentId) || document,
                tagName = tag || '*',
                classLength = classname.length,//获取我们需要筛选class的个数
                classReg = [];//匹配class的正则
    
        for(var i=0;i<classLength;i++){
            classReg[i] = new RegExp("(^|\\s)"+classname[i]+"(\\s|$)","g");//生成class匹配正则
        }
    
        var elems = parentNode.getElementsByTagName(tagName),//获取到所有满足要求的dom节点
                elemLength = elems.length,//elems是类数组，获取length属性代价比较大，所以缓存下来 3
                result = [];//用于保存匹配成功的节点
    
        for(var j = 0;j < elemLength;j++){
    
            var elemTest = elems[j],//获取当前测试的节点
                    elemTestClassName = elemTest.className;//获取当前测试节点的class属性，className兼容IE6+
            //接下来开始对elemTestClassName进行匹配，看它是否匹配classReg中所有的规则
    
            var k=0;
            while(classReg[k] && (classReg[k].test(elemTestClassName))){
                console.log(classReg[k]+":"+elemTestClassName+":"+(classReg[k].test(elemTestClassName)));
                //测试成功进来
                if(k === classLength-1){
                    //如果通过所有测试，说明这个dom结果满足要求，我们push到结果中
                    result[result.length] = elemTest;
                    break;
                }
                k++;
            }
        }
    
        return result;
    }
```

## 问题总结

#### 1.关于while{}循环的问题

```javascript
        var k=0;
        while(classReg[k].test(elemTestClassName)){
            console.log(classReg[k]+":"+elemTestClassName+":"+(classReg[k].test(elemTestClassName)));
            //测试成功进来
            if(k === classLength-1){
                //如果通过所有测试，说明这个dom结果满足要求，我们push到结果中
                result[result.length] = elemTest;
                break;
            }
            k++;
        }
        
```

上面的逻辑在执行过程中报错了`Uncaught TypeError: Cannot read property 'test' of undefined`，原因在于，循环结束时while还是执行一遍classReg[k].test(elemTestClassName)，而此时classReg[k]已经是undefined了。
所以应该在while加上判断

```javascript
      var k=0;
      while(classReg[k] && classReg[k].test(elemTestClassName)){
          console.log(classReg[k]+":"+elemTestClassName+":"+(classReg[k].test(elemTestClassName)));
          //测试成功进来
          if(k === classLength-1){
              //如果通过所有测试，说明这个dom结果满足要求，我们push到结果中
              result[result.length] = elemTest;
              break;
          }
          k++;
      }
```

#### 2.实例

```html
<div id="container">
    <span class="aaa zzz ccc">1</span>
    <div id="div1" class="aaa    bbb   ccc">2</div>
</div>

<div id="div2" class="aaa ccc bbb">3</div>
```
当我们使用上面的方法时会出现一个奇怪的现象，我现在还没搞明白，生气~~~

```javascript
var a = getElementsByClassName(["aaa","ccc"],document,"div");
console.log(a);

```

看到while循环体中的`console.log(elemTestClassName+"测试第"+k+"个正则通过");`了吗？加上这个console时，就能正确打印出两个dom节点[div#div1.aaa.bbb.ccc, div#div2.aaa.ccc.bbb]，注释掉的话，就只能找到[div#div1.aaa.bbb.ccc]。已疯！！！


--------------
做一个勤于思考的人

