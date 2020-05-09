---
title: 基于Angular7响应式表单实现一个简单的登录页面
date: 2019-04-21 09:57:12
tags:
  - 前端开发
  - javascript
  - Angular7
categories: Angular7
---
## 页面功能

{% asset_img login.png 页面功能 %}

+ 页面的基本排版如上
+ 基本功能：姓名为一个组件、密码为一个组件
+ 姓名：有默认值、同步校验（change时校验，错误提示在input右侧悬浮提示）、异步校验（blur时校验，错误提示在input底部出现）
+ 密码：同步校验
+ 提交时会重新对名称和密码再次校验



## 遇到的问题汇总

### Uncaught Error: Template parse errors:Can't bind to 'formGroup' since it isn't a known property of 'form'.

#### 原因
在Angular中要使用FormGroup属性，需要在app.module.ts中引入FormsModule、ReactiveFormsModule模块

#### 解决方法

{% asset_img 2.png Template parse errors解决方法 %}

### ERROR Error: Cannot find control with unspecified name attribute/ERROR TypeError: Cannot create property 'validator' on string 'loginName'

```html
<div class="form-item mt15">
   <label class="formLabel">登录名：</label>
   <input class="form-input" type="text" formControl="loginName"/>
</div>
```
```javascript
export class LoginNameComponent implements OnInit {
  constructor() { }
  loginName: FormControl
  ngOnInit() {
    loginName: new FormControl('xujiao')
  }
}
```
#### 原因

修改为

```html
<div class="form-item mt15">
   <label class="formLabel">登录名：</label>
   <input class="form-input" type="text" [formControl]="loginName"/>
</div>
```

去掉formCtrol外层的[]会报错：Cannot create property 'validator' on string 'loginName'

```javascript
export class LoginNameComponent implements OnInit {
  constructor() { }
  loginName: FormControl
  ngOnInit() {
    this.loginName = new FormControl('xujiao')
  }
}
```
+ this.loginName = new FormControl('xujiao') 改成 loginName: new FormControl('xujiao')会报错ERROR Error: Cannot find control with unspecified name attribute






## 响应式表单和模板驱动表单的区别

在Angular7中存在



## formGroup 和  formArray的区别




-----
做个勤于思考的人~