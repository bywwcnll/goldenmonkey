---
title: caddy教程
date: 2020-05-28 17:06:44
tags:
- caddy
- vue
---

## Caddyfile

```bash
http://testapp.com {
  # tls internal
  # file_server browse

  @wysource {
    path_regexp ^\/(open|api|static\/wangEditor)\/
  }
  reverse_proxy @wysource http://demo.wysource.com.cn {
    header_up Host demo.wysource.com.cn
  }
  reverse_proxy * http://localhost:8080
}
```

## vue.config.js

```js
module.exports = {
  devServer: {
    host: 'testapp.com',
    onListening: function(server) {
      const port = server.listeningApp.address().port
      console.log('Listening on port:', port)
    }
  }
}
```
