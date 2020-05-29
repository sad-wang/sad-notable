---
tags: [JavaScript]
title: JavaScript 全解
created: '2020-05-25T09:02:24.065Z'
modified: '2020-05-29T10:40:32.795Z'
---

# JavaScript 全解

```JavaScript
// 输出打印结果
function sad(n, o) {
  console.log(o)
  return {
    sad: function (m) {
      return sad(m, n)
    }
  }
}
const a = sad(0) // undefined
a.sad(1) // 0
a.sad(2) // 0
a.sad(3) // 0
const b = sad(0) // undefined
b.sad(1).sad(2).sad(3) // 0 1 2
const c = sad(0).sad(1) // undefined 0 1 1
c.sad(2) 
c.sad(3)
```
> 解析：

```JavaScript
// 输出打印结果
3.toString() // error
3..toString() // 3
3...toString() // error
```

> 解析：
运算符优先级问题
点运算符会被优先识别为数字常量的一部分，然后才是对象属性访问符
在JavaScript中，3.1，3.，.1都是合法的数字
3.toString() 会被JS引擎解析成 (3.)toString() 报错
3..toString() 会被JS引擎解析成 (3.).toString() "3"
3...toString() 会被JS引擎解析成 (3.)..toString() 报错

```
// 实现一个 safeGet 函数，可以安全的获取无限多层次的数据，一旦数据不存在不会报错，会返回 undefined
let data = { a: { b: {c: 'sad'} } }
safeGet(data, 'a.b.c')
safeGet(data, 'a.b.c.d')
safeGet(data, 'a.b.c.d.e.f.g')
```
```
let safeGet = (data, target) => {
  let result = data
  const array = target.split('.')
  array.every(char => {
    if (!result.hasOwnProperty(char)) {
       result = undefined
       return false
    }
    result = result[char]
    return true
  })
  console.log(result)
}
```
```JavaScript
function sad () {}
const a = {}
const b = Object.prototype
console.log(a.prototype === b)
console.log(Object.getPrototypeOf(a) === b)
console.log(sad.prototype === Object.getPrototypeOf(sad))
```
> 解析：

```JavaScript
let SAD = function () {}
Object.prototype.a = () => {
  console.log('sad')
}
Function.prototype.b = () => {
  console.log('happy')
}
var sad = new SAD()
SAD.a() // sad
SAD.b() // happy
sad.a() // sad
sad.b() // error
```

