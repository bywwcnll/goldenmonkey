---
title: caddy教程
date: 2020-05-28 17:06:44
tags:
- caddy
- vue
---

## Caddyfile

```bash
(wysource) {
  @wysource {
    path_regexp ^\/(open|api|static\/wangEditor)\/
  }
  reverse_proxy @wysource http://demo.wysource.com.cn {
    header_up Host demo.wysource.com.cn
  }
}

# tls internal
# file_server browse

http://app.try {
  import wysource
  reverse_proxy * http://app.try:8080
}

http://app1.try {
  import wysource
  reverse_proxy * http://app.try:8081
}
```

## vue.config.js

```js
module.exports = {
  devServer: {
    host: 'app.try'
  }
}
```
