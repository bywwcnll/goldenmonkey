---
title: caddy开启https
date: 2020-10-28 14:33:24
tags: caddy https vue-cli3
---
## 开始

1. 自建v3版证书

1. 设置Caddyfile

1. 设置vue.config.js

### 自建证书

1. 复制`/System/Library/OpenSSL/openssl.cnf`副本，修改里面的配置

    ``` bash
    cp /System/Library/OpenSSL/openssl.cnf ~/Desktop
    cd ~/Desktop
    ```

    ``` conf
    [ req ]
    ...
    req_extensions = v3_req

    [ v3_req ]
    ...
    subjectAltName = @alt_names

    [ alt_names ]
    DNS.1 = localhost
    DNS.2 = app.try
    DNS.3 = app1.try
    DNS.4 = app2.try
    DNS.5 = app3.try
    ```

1. 按照命令创建证书

    ``` bash
    openssl genrsa -out ca.key 2048
    openssl req -new -key ca.key -out ca.csr
    openssl x509 -req -days 3650 -sha256 -extfile openssl.cnf -extensions v3_req -in ca.csr -signkey ca.key -out ca.crt
    ```

1. 将创建好的证书导入到本机钥匙串，并设置**始终信任**

### 设置Caddyfile

```bash
(wysource) {
  @wysource {
    path_regexp ^\/(open|api|static\/wangEditor)\/
  }
  reverse_proxy @wysource http://demo.wysource.com.cn
}

# file_server browse
https://app.try {
  tls /Users/Gaven/Desktop/ca.crt /Users/Gaven/Desktop/ca.key
  import wysource
  request_header Host {http.reverse_proxy.upstream.hostport}
  reverse_proxy * https://app.try:8080
}
```

### 设置vue.config.js

``` js
devServer: {
  host: 'app.try',
  https: {
    key: fs.readFileSync('/Users/Gaven/Desktop/ca.key'),
    cert: fs.readFileSync('/Users/Gaven/Desktop/ca.crt')
  }
}
```

### 设置vue.config.ts

``` js
server: {
   host: 'app3.try',
   https: {
      key: fs.readFileSync('/Users/ares/my_certificate/ca.key'),
      cert: fs.readFileSync('/Users/ares/my_certificate/ca.crt')
   },
   port: 8080,
}
```

### Safari开启wss(Websocket安全模式)测试功能, chrome上暂时无法测试

> 在“设置”>“ Safari”>“高级”>“实验性功能”>“ NSURLSession Websocket”中启用NSURLSession Websocket。应该可以在iOS 13.4.1以上版本上使用。