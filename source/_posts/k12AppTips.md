---
title: k12AppTips
date: 2019-06-17 10:10:19
tags: k12 tip
---
# k12App开发注意事项
1. k12debugger`0.0.4`以下版本不兼容旧浏览器
1. element-ui版本不高于`2.4.9`
1. webpack3.0的打包环境下，如果使用了最新的k12vux和k12lib库，需要在`webpack.prod.conf.js`中修改`OptimizeCSSPlugin`参数  
原因是autoprefixer会自动移除`-webkit-box-orient: vertical;`属性  
修改如下：
  ``` JS
  new OptimizeCSSPlugin({
    cssProcessorOptions: config.build.productionSourceMap
      ? { safe: true, map: { inline: false }, autoprefixer: { remove: false } }
      : { safe: true, autoprefixer: { remove: false } }
  })
  ```
1. 为了是搜索人员的接口能处理大参数传递，需要把vuex/modules/common.js中的`searchUserByKeyword`改成post方式：
  ``` JS
  async searchUserByKeyword ({ commit, state }, data) {
    return request({
      url: api.searchUserByKeyword,
      data,
      method: 'POST',
      useUrlencode: true
    })
  }
  ```
