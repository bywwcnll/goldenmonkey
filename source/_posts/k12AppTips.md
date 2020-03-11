---
title: k12AppTips
date: 2019-06-17 10:10:19
tags: k12 tip
---

## k12App开发注意事项

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

1. `wangEditor`组件使用方法：

  ``` JS
  // 在main.js中引入脚本：
  window.document.title = '上下级通知'
  window.appCode = 'announcement'
  ;(async () => {
    wisLoading(true)
    await Promise.all([
      setCorpId(),
      setWechatSDK({ hideShareBtn: true }),
      loadScript(window.HOST + '/static/wangEditor/wangEditor.min.js')
    ])
    /* eslint-disable no-new */
    new Vue({
      router,
      store,
      render: h => h(App)
    }).$mount('#app')
  })()

  //  使用方法：
  this.$refs.wisEditor.setHtml(content.replace(/<_wisbr>/g, '<br>'))
  this.$refs.wisEditor.getHtml().replace(/(\n|<\/?br>)/g, '<_wisbr>')
  ```

1. 兼容性说明：

  pc组件页面支持到IE10+

  移动组件页面只支持chrome70+

1. 使用`mixins`时，请将js文件必须放置在mixins目录下(没有可在src/components下创建)

  0.0.4版本的`babel-plugin-k12swagger`处理机制如下：

  ``` JS
  if (/(\.vue|mixins\/\w+\.js)$/.test(filename)) {
    path.traverse(ObjectPropertyVisitor)
  }
  ```

1. k12-upload使用问题解决：

  删除图片时，一直删除的是第一张图片的原因如下

  ``` JS
  onDeleteAddedImg (data) {
    let delIndex = this.formData.map(el => el.localId).indexOf(data.localId)
    if (delIndex > -1) {
      this.formData.splice(delIndex, 1)
    }
  }
  ```

  解决方法：在给`defaultFormData`赋值时添加`localId`属性

  ``` JS
    this.defaultFormData = (tmp.ossObjects || []).map(el => {
      return {
        objectId: el.objectId,
        localId: el.objectId,
        thumbnailUrl: el.thumbnailUrl,
        srcData: el.previewUrl
      }
    })
  ```

1. `v-infinite-scroll`使用注意事项

  ``` JS
  async handleChangeState (v) {
    this.recordListData = {...recordListDataInitData}
    this.recordListRows = []
    await this.getRecordList()
  }
  ```

  当`infinite-scroll-immediate-check`为`false`，父元素的`scrollTop`不为0时，切换状态后`v-infinite-scroll`绑定的方法依然会触发，这样会导致发送两次请求的问题，可以通过以下方法避免

  ``` HTML
  <div class="mainC">
    <div ref="scrollDom"
          v-infinite-scroll="loadMore"
          infinite-scroll-disabled="isLoading"
          infinite-scroll-distance="10"
          infinite-scroll-immediate-check="false">
      <!-- 其他代码 -->
    </div>
  </div>
  ```

  ``` JS
  async getRecordList (exeLoading = true) {
    let { parentElement } = this.$refs.scrollDom || {}
    if (parentElement && parentElement.scrollTop > 0 && isEmpty(this.recordListRows)) return
    if (exeLoading) this.$wisLoading(true)
    /* ... */
  }
  ```
