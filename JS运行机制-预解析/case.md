```js
function fn(a){
  console.log(a);
  var a = 123;
  console.log(a);
  function a(){}
  console.log(a);
  console.log(b);
  var b = function(){}
  console.log(b);
  function d(){}
  console.log(d)
}
fn(1);
```
#### 分析预解析
```js
AO = {
  a: function a(){},
  b: undefined,
  d: function d(){}
}
```
#### 解释执行
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
  console.log(b);//undefined
  var b = function(){}
  AO = {
    a: 123,
    b: function(){},
    d: function d(){}
  }
  condole.log(b);//function(){}
  function d(){}
  console.log(d)//function d(){}
```