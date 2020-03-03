### 一. js运行三部曲：
+ 【语法分析】：js引擎检查代码是否有语法错误
+ 【预编译】：简单理解就是在内存中开辟一些空间，存放一些变量与函数
+ 【解释执行】

### 二. 预编译：
+ 【脚本代码块script执行前】：
    1. 查找**全局变量声明**（包括**隐式全局**变量声明，省略var声明），变量名作为全局对象GO/执行上下文/（window对象）的属性，值为undefined
    2. 查找**函数声明**（不包括函数表达式），函数名作为GO属性，值为函数引用
+ 【函数执行前】：
    1. 创建AO对象/执行上下文
    2. 查找**函数形参**和**函数内变量声明**，形参名和变量名作为AO的属性，值为undefined
    3. 实参赋给形参
    4. 查找函数声明，函数名作为AO的属性，值为函数引用

### 三. 实例分析：
```js
var a = 1;//**全局变量声明**
console.log(a);
function test(a){//**函数声明**
  console.log(a);
  var a = 123;
  console.log(a);
  function a(){}
  console.log(a);
  var b = function(){}
  console.log(b);
  function d(){}
}
var c = function(){
  console.log("i at c function")
}
console.log(c);
test(2);
```
#### 分析预编译和解释执行过程如下：
+ 预编译：
```js
GO = {
  a: undefined,
  c: undefined,
  test: function(a){
    console.log(a);
    var a = 123;
    console.log(a);
    function a(){}
    console.log(a);
    var b = function(){}
    console.log(b);
    function d(){}
  }
}
```
+ 解释执行：
```js
GO = {
  a: 1,
  c: function(){
    console.log("i at c function")
  },
  test: function(a){//**函数形参**
    console.log(a);
    var a = 123;//**函数内变量声明**
    console.log(a);
    function a(){}
    console.log(a);
    var b = function(){}//**函数内变量声明**
    console.log(b);
    function d(){}
  }
}
```
##### 开始执行test()之前，发生：
###### 预编译
（1. 2.）:
```js
AO = {
  a: undefined,
  b: undefined
}
```
------>
(3. ）:
```js
AO = {
  a: 2,
  b: undefined
}
```
------->
(4. ）:
```js
AO = {
  a: function a(){},
  b: undefined,
  d: function d(){}
}

```
##### 开始执行test():
```js
console.log(a);//function a(){}
var a = 123;
AO = {
  a: 123,
  b: undefined,
  d: function d(){}
}
console.log(a);//123
function a(){}
console.log(a);//123
var b = function(){}
AO = {
  a: 123,
  b: function(){},
  d: function d(){}
}
console.log(b);//function(){}
function d(){}
```