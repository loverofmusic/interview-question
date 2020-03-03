##### 浏览器和node的区别 微任务 宏任务 任务队列
```js
console.log("script start")

async function async1(){
  await async2()
  console.log("async1 end")
}

async function async2(){
  console.log("async2 end")
}

async1()

setTimeout(function(){
  console.log('setTimeout')
},0)

new Promise(resolve => {
  console.log('Promise');
  resolve()
})
.then(function(){
  console.log('Promise1')
})
.then(function(){
  console.log('Promise2')
})

console.log("script end")

// script start
// async2 end
// Promise
// script end
// async1 end
// Promise1
// Promise2
// undefined
// setTimeout

```
