---
title: js实现对象的深度克隆
date: {{date}}
tags:
  - 前端开发
  - js实现对象的深度克隆
  - javascript
  - 深度克隆
categories: javascript
---


## 变量访问的方式

在JavaScript中，访问变量可以通过**值访问**也可以通过**引用访问**。

+ `按值访问：`直接读取值的本身

+ `按引用访问：`读取引用地址，根据地址查找真实值。引用类型的值是保存在内存中的对象，在JavaScript中不允许直接访问内存中的位置。所以在操作对象时，实际上操作的是在操作对象的引用而不是实际的对象。

## 函数中的传参方式

在JavaScript的函数传参数方式只能`按值传递`（不管传递的变量是值类型还是引用类型）。也就是将传递的参数复制给函数的一个局部变量（arguments）。

值复制和引用复制：

+ `值复制:`将值本身复制给arguments的一个元素，参数不会影响原变量
+ `引用复制:`将值得地址复制给arguments的一个元素，参数会影响到原来的变量



## 如何实现一个对象的深度克隆

### 为什么要深度克隆一个对象

在日常中对一个对象的复制都只是对其引用的复制，两者之间还是会相互影响，如：

```javascript
    var a = [1,2,3];
    var b = a;
    b.push(5);
    console.log(a);// [1, 2, 3, 5]，对b的操作影响了a
```

		
所以为了是b和a完全一致,但是又不相互影响,就有了深度克隆这一说。


### 获取对象的基本类型

我们都知道在JavaScript中,基本的数据类型(string/boolean/undefined/null/number)的复制属于值复制,相互之间不会有什么影响。那么我们接下来主要讨论的是对象的复制。

+ 对象的类型：function、object、Array
+ 如何获取以上的基本类型： Object.prototype.toString.call(para) 
  
  + [object Function]
  + [object Object]
  + [object Array]
 
  也就是返回值是一个字符串，可以通过以下方法获取具体的类型
  
```javascript
function objType(obj) {
    var obj = Object.prototype.toString.call(obj).substring(8);
    return obj.substring(0,obj.length-1);
}

//获取值分别是：Function、Object、Array
           
```  

	    
+ 函数的复制我们怎么去理解：函数是一个对象,函数名是指向对象的名称。
  
```javascript
    function text(){
       console.log("a");
    }
```
            
            
这里,我们可以理解,以上的声明在内存中开辟了一个空间,保存了一个方法,这个方法包含一个打印功能。然后 变量text包含执行这个方法的地址。所以,var b = text;操作,就是将方法的地址复制给函数b。
    
+ 在JavaScript中,函数不存在重载,也就是不存在某个函数被修改了的情况(只是断开了指向)。
   
### 具体实现

这里的实现分为你三个部分,入口函数deepClone,对象的克隆函数,以及数组的克隆函数


#### 1. 对象的克隆

```javascript
 function cloneObj(obj) {
    var tempObj = new Object();
    for(key in obj){
        switch (objType(obj[key])){
            case "Object":
                tempObj[key] =cloneObj(obj[key]);
                break;
            case "Array":
                tempObj[key] = cloneArr(obj[key]);
                break;
            default:
                tempObj[key] = obj[key];

            break;
        }
    }
    return tempObj;
 }
```
       
            
            
#### 2. 数组的克隆
  
```javascript
function cloneArr(arr) {
    var tempArr = new Array();
    for(var i = 0;i<arr.length;i++){
        switch (objType(arr[i])){
            case "Object":
                tempArr.push(cloneObj(arr[key]));
                break;
            case "Array":
                tempArr.push(cloneArr(arr[key]))
                cloneArr(arr[i]);
                break;
            default:
                tempArr.push(arr[i])
        
                break;
        }
    }
    
    return tempArr;
}
```
            
            
            
#### 3. 克隆的入口

```javascript
function deepClone(para) {
    var temp = null;
    switch (objType(para)){
         case "Object":
             temp = cloneObj(para);
             break;
         case "Array":
             temp = cloneArr(para);
             break;
         default:
             temp = para;
             break;
    }
    return temp;
}
```
            
  
            
#### 4. 测试

```javascript

 var testObj = {
        "name":"xujiao",
        "age":"28",
        "say":function () {
            console.log(123);
        },
        "arr":[1,3,4],
        "obj":{
          "a":12,
          "b":1,
          "c":3
        }
    }
    var clonexx = deepClone(testObj);
    clonexx.arr.push(333);
    console.log(clonexx);
    console.log(testObj);//对clonexx的操作并没有影响testObj
    
```
      

	
	
	
	
	


