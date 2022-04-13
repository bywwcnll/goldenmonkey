---
title: simple-uuid
date: 2022-04-13 14:42:20
tags:
---

# 简易UUID

```js
const uid = () => 'xxxxx'.replace(/x/g, () => ((Math.random() * 16) | 0).toString(16))

let count = 0
const init = uid()
console.log(`初始值： ${init}`)
let c = uid()
while (true) {
  if (init !== c) {
    count++
    if (count % 10000 === 0) {
      console.log(`count： ${count}  c: ${c}`)
    }
    if (count >= 10 ** 8) {
      c = init
    }
    c = uid()
  } else {
    break
  }
}
console.log(`总共计算： ${count}`)
```