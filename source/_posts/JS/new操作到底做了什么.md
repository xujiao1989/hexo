---
title: new操作到底做了什么
date: 2017-03-25
tags:
  - 前端开发
  - 个人成长
  - JS
  - 继承
categories: JS
---
  
## new操作做了哪些


```javascript
function F(name,age){
    this.name = name;
    this.age = age;
    this.say = function(){
       console.log(this.name+"/"+this.age)
    }
   return {
      a:1,
      b:2
   }
 }

 var f = new F()；
 console.log(f);
```
    
 
new调用函数（实例化）执行了5项操作

#### 第一步：创建一个新的对象

```javascript
var f = new Object();
```

    
#### 第二步：创建原型链接
```javascript
f._proto_ = F.prototyte;
f._proto_ === F.prototype // true
```
      
#### 第三步：绑定this

让F中的的this指向f上

#### 第四步：执行构造函数中的代码，也就是为新对象添加属性

```javascript
f.name = undefined, 
   f.age = undefined,
   f.say= function(){
   console.log(this.name+"/"+this.age)//这里this指向f
}
```
       
#### 第五步：返回新对象
  + 如果返回的对象是值类型，就丢弃，返回这个新对象
     return 1;是无效的，控制台打印的是对象f
  + 如果是引用类型，这返回这个引用类型，取代新对象
     return {a:1,b:2}会取代f

## call、apply、bind

三者的作用都是改变this指向的，并且参第一个参数都是this要指向的对象，都可以利用后续参数传参

### 区别在于：

+ call和apply本质上功能一致，都是直接执行了，但是传参的方式不一样
    + obj.call(obj1,arg1,arg2),
    + obj.apply(obj1,[arg1,arg2])
+ bind没有直接执行，而是返回一个函数，我们可以链式一直玩下去~

## 继承
       
上面扯了那么多似乎都跟继承没什么关系，我理解的是，什么是继承，继承就是能够使用其他对象的属性和方法，管你用什么方法。
那么有几种方法可以让我们使用别的对象的方法和属性呢。

+ 看上面的new操作第四步做了什么，执行了构造函数里面的代码，相当于复制了一份，那么相当于可以使用F中的属性和方法了。
从这个层面上看，构造函数是实现继承的一个方式。

    + 问题是：如果每次继承父元素的属性和方法都得复制父元素的全部代码的话，似不似撒
    + 解决方法是什么，看new操作第二步干了什么，是的，__proto__属性指向了F的prototype，所以可以把F中通用的方法都移到F.prototype上，这就是传说中的基于原型的继承。

+ 另一个关于继承的方法，是利用改变this指向实现的
   
```javascript
        function Person(name,age) {
           this.name = name;
           this.age = age;
           this.say=function () {
               console.log(this.name+"/"+this.age);
           }
       }
   
       function xx(name,age){
           Person.apply(this,arguments);
       }
   
       var b = new xx("我","18");
       b.say();
```

看到了吗  b是从xx的实例，可是它居然能使用parson的say方法，可不就是继承吗   

## 小知识     

### Object.create()了解
+ 是es5的新特性
+ 有两个参数，第二个参数可选（描述性）
```javascript
//可以看下object.create的实现
if (!Object.create) {
  Object.create = function (o) {
     function F() {}  //定义了一个隐式的构造函数
     F.prototype = o;
     return new F();  //其实还是通过new来实现的
 };
}

object.create(null)
//创建一个很空的对象，他的原型都是空的，区别new object
```
        
### _ proto _ 和prototype的区别

+ **JS中对象**:具有属性__proto__，可称为隐式原型，一个对象的隐式原型指向构造该对象的**构造函数的原型**，这也保证了实例能够访问在构造函数原型中定义的属性和方法。
  
+ 函数是个特殊的对象，除了和其他对象一样有上述_proto_属性之外，还有自己特有的属性——**原型属性（prototype)**，这个属性是一个指针，指向一个函数的原型对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫constructor，这个属性包含了一个指针，指回原构造函数。        
```javascript

function F(name,age){
    this.name = name;
    this.age = age;
    this.say = function(){
       console.log(this.name+"/"+this.age)
    }
 }
var f = new F();
console.log(F.constructor === Function);//F的构造函数是Function
console.log(Function.prototype);//function(){}
console.log(Function.__proto__);//function(){}
console.log(F.__proto__)//F.__proto__指向构造函数Function的原型prototype:function(){}

console.log(f.constructor === F);//true

console.log(F.prototype);//Object{}
console.log(f.__proto__);//f.__proto__指向构造函数F的原型:Object{}

console.log(F.prototype);//Object{}
console.log(f.prototype);//prototype是方法的属性,f是个普通的对象,没有prototype属性


```
     
                 
+ constructor：constructor不需要手动创建，它是对象原型上的一个属性。Object.prototype.constructor，返回一个**指向创建了该对象原型的函数引用**。需要注意的是，该属性的值是那个函数本身，该属性为只读。
```javascript
//对于上面的例子：
console.log(F.constructor)
//Function() { [native code] }
 console.log(f.constructor);
function F(name,age){
     this.name = name;
     this.age = age;
     this.say = function(){
        console.log(this.name+"/"+this.age)
     }
 }
```
    
  
## 其他

+ 构造函数的首字母一般大写
+ 值类型赋值是值的复制
+ 对象类型的赋值不仅是值的复制也是引用的复制
```javascript
var a = [1,2,3];
var b = a;
b.push(4);
//a[1,2,3,4] b[1,2,3,4]

var a = [1,2,3];
    b = a;//值和引用都给了b
    b = [1,2,3,4];// a[1,2,3] b[1,2,3,4]
```
         
              
  
---
做一个勤于思考的人