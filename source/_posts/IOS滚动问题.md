---
title: 解决IOS滚动问题
date: 2020-06-08 17:30:24
tags:
---

## 方法

将 `html, body, #app`高度设为`100%`并隐藏滚动条，接着对子元素开启滚动即可

```css
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  -webkit-overflow-scrolling: touch;
}

html, body, #app {
  width: 100%;
  height: 100%;
  background-color: #F7F7F7;
  font-family: 'Microsoft YaHei', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* 主要是这句话 */
  overflow: hidden;
}

.mobilePageContainer {
  margin: 0 auto;
  height: 100%;
  overflow: auto;
}
.pcPageContainer {
  min-width: 800px;
  background-color: #fff;
  padding: 20px;
  height: 100%;
  overflow: auto;
}
```

## 隐藏微信顶部 "网页由xx提供" 的信息

```css
body:before {
  width: 100%;
  height: 100%;
  content: ' ';
  position: fixed;
  z-index: -1;
  top: 0;
  left: 0;
  background: #fff;
}
```
