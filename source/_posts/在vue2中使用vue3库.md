---
title: 在vue2中使用vue3库
date: 2022-05-10 14:56:27
tags: vue lib
---

# vue2中使用vue3的库

## vue3 **vite.config.js** 打包设置

```js
import path from 'path'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import cssInjectedByJsPlugin from 'vite-plugin-css-injected-by-js'

// https://vitejs.dev/config/
export default defineConfig(({ command }) => {
  if (command === 'serve') {
    return {
      plugins: [vue()],
    }
  } else {
    return {
      plugins: [vue(), cssInjectedByJsPlugin()],
      build: {
        lib: {
          entry: path.resolve(__dirname, 'src/lib.ts'),
          name: 'myLib',
          fileName: (format) => `myLib.${format}.js`,
        },
        rollupOptions: {
          // 确保外部化处理那些你不想打包进库的依赖
          external: ['vue'],
          output: {
            // 在 UMD 构建模式下为这些外部化的依赖提供一个全局变量
            globals: {
              vue: 'Vue3',
            },
          },
        },
      },
    }
  }
})
```

## 修改vue3js的全局变量名为`Vue3`

## vue2中引入vue3的组件库和vue3js

## 使用vue3组件
```js
export default {
  mounted () {
    let { navs } = this
    Vue3.createApp({
      components: { 'cc-nav': myLib.Nav },
      data () { return { navs } },
      template: `
        <cc-nav :data="navs"><button>232</button></cc-nav>
        <h1>2323</h1>
      `
    })
    .mount(this.$refs.nav)
  },
  data () {
    return {
      navs: ['大鱼', '紫'],
    }
  }
}
```