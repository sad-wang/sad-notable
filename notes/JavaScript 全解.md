---
tags: [JavaScript]
title: JavaScript 全解
created: '2020-05-25T09:02:24.065Z'
modified: '2020-05-25T09:26:12.202Z'
---

# JavaScript 全解

```JavaScript
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

