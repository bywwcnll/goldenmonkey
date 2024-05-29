---
title: 【vue2】插槽传递
date: 2024-05-29 10:14:42
tags: vue2 插槽
---

## 主动式
``` js
Vue.component('VNodes', {
  functional: true,
  render: (h, ctx) => ctx.props.vnodes,
})
// 或
components: {
  VNodes: {
    functional: true,
    render: (h, ctx) => ctx.props.vnodes,
  },
}
```

``` html
<comp>
  <template v-for="k in Object.keys($slots)" v-slot:[k]>
    <v-nodes :vnodes="$slots[k]"></v-nodes>
  </template>
</comp>
```

## 被动式
``` html
<comp>
  <template v-for="k in Object.keys($slots)" v-slot:[k]>
    <slot :name="k"></slot>
  </template>
</comp>
```