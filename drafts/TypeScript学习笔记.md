---
title: TypeScript学习笔记
date: 2019-03-24 14:20:05
tags:
  - 前端开发
  - javascript
categories: javascript
---

## TypeScript简介

#### 1、TypeScript是哪个公司在维护，目前经历了哪些版本迭代？

+ 微软开发的JavaScript的超集，是一种`面向对象的编程语言`；
+ 超集意味着完全兼容JavaScript；
+ 到目前（2019-03-24）为止，最新版本的TypeScript为3.3


#### 2、TypeScript的设计初衷是什么？是为了解决什么问题？

TypeScript是JavaScript的一个超集，那么在设计的初衷上来讲肯定是为了弥补JavaScript自身的一些缺陷和短板。那么TypeScript相对于JavaScript究竟做了哪些事情：

+ TypeScript 是 Microsoft 推出的开源语言，使用 Apache 授权协议
+ TypeScript 增加了静态类型、类、模块、接口和类型注解
+ TypeScript 可用于开发大型的应用
+ TypeScript 易学易于理解

#### 3、TypeScript的优势是什么

1. 静态输入
   静态类型化是一种功能，可以在开发人员编写脚本时检测错误。查找并修复错误是当今开发团队的迫切需求。有了这项功能，就会允许开发人员编写更健壮的代码并对其进行维护，以便使得代码质量更好、更清晰。

2. 大型的开发项目
   有时为了改进开发项目，需要对代码库进行小的增量更改。这些小小的变化可能会产生严重的、意想不到的后果，因此有必要撤销这些变化。使用TypeScript工具来进行重构更变的容易、快捷。

3. 更好的协作
   当发开大型项目时，会有许多开发人员，此时乱码和错误的机也会增加。类型安全是一种在编码期间检测错误的功能，而不是在编译项目时检测错误。这为开发团队创建了一个更高效的编码和调试过程。

4. 更强的生产力
   干净的 ECMAScript 6 代码，自动完成和动态输入等因素有助于提高开发人员的工作效率。这些功能也有助于编译器创建优化的代码。

## TypeScript学习记录

+ TypeScript 只会进行静态检查，如果发现有错误，编译的时候就会报错。
+ TypeScript 编译的时候即使报错了，还是会生成编译结果，我们仍然可以使用这个编译之后的文件。

   
#### 1、原始的数据类型

+ 布尔值: let isDone: boolean = false;
+ 数值：let decLiteral: number = 6;
+ 字符串：let myName: string = 'Tom';
+ 空值：JavaScript 没有空值（Void）的概念，在 TypeScript 中，可以用 void 表示没有任何返回值的函数，声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null：let unusable: void = undefined;
+ Null 和 Undefined：在 TypeScript 中，可以使用 null 和 undefined 来定义这两个原始数据类型，undefined 类型的变量只能被赋值为 undefined，null 类型的变量只能被赋值为 null。

#### 2、任意值

+ 任意值（Any）用来表示允许赋值为任意类型；
+ 在任意值上访问任何属性都是允许的、也允许调用任何方法；
+ 可以认为，声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值；
+ 变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型；

#### 3、类型推论

+ 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型
+ 如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查

#### 4、联合类型

+ 联合类型（Union Types）表示取值可以为多种类型中的一种
+ 联合类型使用 | 分隔每个类型
+ 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法，如果所有类型中没有这个公用的属性和方法，会报错
+ 联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型

#### 5、对象的类型——接口

+ 在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型
+ 在面向对象语言中，接口（Interfaces）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implements）；
+ TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述；
+ 接口一般首字母大写；
+ 赋值的时候，变量的形状必须和接口的形状保持一致：定义的变量比接口少了一些属性是不允许的，多一些属性也是不允许的；

**可选属性：**

+ 可选属性的含义是该属性可以不存在
+ 这时仍然不允许添加未定义的属性：

```javascript
      interface Person {
          name: string;
          age?: number;
      }
```

**任意属性**

+ 一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集：

```javascript
      interface Person {
          name: string;
          age?: number;
          [propName: string]: any;
      }
```

**只读属性**

+ 只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候：

```javascript
      interface Person {
          readonly id: number;
          name: string;
          age?: number;
          [propName: string]: any;
      }
```

#### 6、数组的类型

在 TypeScript 中，数组类型有多种定义方式，比较灵活。

+ 「类型 + 方括号」表示法：let fibonacci: number[] = [1, 1, 2, 3, 5];

 

## 参考资料

+ [TypeScript VS JavaScript 深度对比](https://segmentfault.com/a/1190000012744810)