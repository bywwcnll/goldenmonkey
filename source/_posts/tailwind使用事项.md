---
title: tailwind使用事项
date: 2024-08-01 16:29:36
tags:
---

# tailwind使用事项

## 添加前缀

#### group使用

```js
# tailwind.config.js
module.exports = {
    prefix: 'zz-',
}
```

```html
<div class="zz-group/btn">
    <div class="group-hover/btn:zz-text-primary"></div>
</div>
```