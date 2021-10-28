---
title: 申请免费Https证书
date: 2021-05-08 14:38:13
tags:
- Https
- certbot
---

## 安装 [certbot](https://certbot.eff.org/lets-encrypt/centosrhel8-nginx)

### centos

   ``` bash
   yum -y install certbot
   ```

### ubuntu

   ``` bash
   apt-get update
   //用于添加ppa源的小工具，ubuntu server默认没装
   apt-get install software-properties-common
   //把ppa源添加到source list中
   add-apt-repository ppa:certbot/certbot
   apt-get update
   apt-get install certbot
   ```

## webroot方式获取证书

### nginx示例

1. 运行配置

    ``` conf
    server {
        listen 80;
        server_name security.wysource.com.cn;
    
        location / {
            root /usr/share/nginx/html;
        }
    }
    ```

1. 获取证书

    ``` bash
    certbot certonly --webroot -w /usr/share/nginx/html -d security.wysource.com.cn --preferred-chain "ISRG Root X1"
   ```
   
1. 修改配置

    ``` bash
    server {
        listen 80;
        server_name security.wysource.com.cn;
        listen 443 ssl;
        ssl_certificate /etc/letsencrypt/live/security.wysource.com.cn/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/security.wysource.com.cn/privkey.pem;
   
        location / {
            root /usr/share/nginx/html;
        }
    }
   ```
   
1. 测试续期

   ``` bash
   certbot renew --dry-run
   ```
   
## 限制

- 申请的证书有效期只有90天 
- 同一个顶级域名下的二级域名，一周做多申请 20 个
- 一个域名一周最多申请 5 次
- 1 小时最多允许失败 5 次
- 请求频率需要小于 20 次/s
- 一个 ip 3 小时内最多创建 10 个账户
- 一个账户最多同时存在 300 个 pending 的审核

## Cron自动续期

   ``` bash
   crontab -e
   # * * * * * /bin/echo $(date) >> test.log 每分钟执行一次
   # 0 3 1 */2 * certbot renew 每2个月的凌晨3点续期一次
   ```
   
   