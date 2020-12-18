---
title: 使用acme申请免费的HTTPS证书
date: 2020-12-11 18:10:26
tags:
- acme
- https
---

<!--more-->

## 相关资料
[acme 文档](https://github.com/acmesh-official/acme.sh/wiki/)
[Let’s Encrypt 证书颁发机构](https://letsencrypt.org/)

## 安装acme

``` bash
curl https://get.acme.sh | sh
``` 

默认安装目录在~/.acme.sh/

安装过程中会自动为你创建 cronjob, 每天 0:09 点自动检测所有的证书, 如果快过期了, 需要更新, 则会自动更新证书。

``` bash
9 0 * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" > /dev/null
``` 
### 可选参数

--home 是要安装的自定义目录acme.sh。默认情况下，它安装到~/.acme.sh。
--config-home 是一个可写文件夹，acme.sh将在其中写入所有文件（包括cert / keys，configs）。默认情况下--home。
--cert-home 是自定义的目录，用于保存您颁发的证书。默认情况下，它保存在中--config-home。
--accountemail 是用于向Let's Encrypt注册帐户的电子邮件，您将在此处收到续订通知电子邮件。默认为空。
--accountkey 是保存您帐户私钥的文件。默认情况下，它保存在中--config-home。
--useragent 请求的用户代理头的值。

## 申请证书

申请证书需要验证域名所有权

有三种方式验证

### 1.HTTP 方式
HTTP方式需要在你的网站根目录下放置一个文件, 来验证你的域名所有权,完成验证。就可以生成证书了。
--webroot 为你网站的根目录，acme会自动生成验证文件到网站根目录下，申请完成后会自动删除验证文件。
-d 后面要申请证书的域名，可多个

缺点 不可申请泛域名
``` bash
acme.sh  --issue  -d www.liangdonglin.cn   --webroot  /www/www.liangdonglin.cn
``` 
如果你用的是nginx或者apache,使用以下命令不需要指定网站根目录，它会自动根据配置完成验证。

``` bash
acme.sh  --issue   -d www.liangdonglin.cn   --nginx
acme.sh  --issue   -d www.liangdonglin.cn   --apache
``` 
### 1.DNS 方式
需要添加DNS解析验证域名所有权
缺点 使用这种方式无法自动更新证书
优点 
1.可以申请泛域名
2.可以在任意安装了acme的机器申请证书，只要在域名NS管理方添加一条TXT dns解析记录即可

#### 申请证书
``` bash
acme.sh --issue -d *.liangdonglin.cn  --dns  --yes-I-know-dns-manual-mode-enough-go-ahead-please

[2020年 12月 18日 星期五 16:19:25 CST] Using CA: https://acme-v02.api.letsencrypt.org/directory
[2020年 12月 18日 星期五 16:19:25 CST] Creating domain key
[2020年 12月 18日 星期五 16:19:25 CST] The domain key is here: /root/.acme.sh/*.liangdonglin.cn/*.liangdonglin.cn.key
[2020年 12月 18日 星期五 16:19:25 CST] Single domain='*.liangdonglin.cn'
[2020年 12月 18日 星期五 16:19:25 CST] Getting domain auth token for each domain
[2020年 12月 18日 星期五 16:19:30 CST] Getting webroot for domain='*.liangdonglin.cn'
[2020年 12月 18日 星期五 16:19:30 CST] Add the following TXT record:
[2020年 12月 18日 星期五 16:19:30 CST] Domain: '_acme-challenge.liangdonglin.cn'
[2020年 12月 18日 星期五 16:19:30 CST] TXT value: '86gDwJGcR26QvGkhl5oCZvFPL2Iv3FHEjLiZBCEBbyE'
[2020年 12月 18日 星期五 16:19:30 CST] Please be aware that you prepend _acme-challenge. before your domain
[2020年 12月 18日 星期五 16:19:30 CST] so the resulting subdomain will be: _acme-challenge.liangdonglin.cn
[2020年 12月 18日 星期五 16:19:30 CST] Please add the TXT records to the domains, and re-run with --renew.
[2020年 12月 18日 星期五 16:19:30 CST] Please add '--debug' or '--log' to check more details.
[2020年 12月 18日 星期五 16:19:30 CST] See: https://github.com/acmesh-official/acme.sh/wiki/How-to-debug-acme.sh
``` 
![dns_image](https://cdn.jsdelivr.net/gh/liangdongin/blog@latest/2020/12/11/acme/dns.jpg)

#### 查看解析

``` bash
dig -t txt  _acme-challenge.liangdonglin.cn @8.8.8.8

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> -t txt _acme-challenge.liangdonglin.cn @8.8.8.8
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 57426
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;_acme-challenge.liangdonglin.cn. IN	TXT

;; ANSWER SECTION:
_acme-challenge.liangdonglin.cn. 299 IN	TXT	"86gDwJGcR26QvGkhl5oCZvFPL2Iv3FHEjLiZBCEBbyE"

;; Query time: 143 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: 五 12月 18 16:32:25 CST 2020
;; MSG SIZE  rcvd: 116
``` 
#### 完成验证，得到证书

``` bash
acme.sh --renew -d *.liangdonglin.cn  --dns  --yes-I-know-dns-manual-mode-enough-go-ahead-please

[2020年 12月 18日 星期五 16:44:33 CST] Renew: '*.liangdonglin.cn'
[2020年 12月 18日 星期五 16:44:37 CST] Using CA: https://acme-v02.api.letsencrypt.org/directory
[2020年 12月 18日 星期五 16:44:37 CST] Single domain='*.liangdonglin.cn'
[2020年 12月 18日 星期五 16:44:37 CST] Getting domain auth token for each domain
[2020年 12月 18日 星期五 16:44:37 CST] Verifying: *.liangdonglin.cn
[2020年 12月 18日 星期五 16:44:43 CST] Success
[2020年 12月 18日 星期五 16:44:43 CST] Verify finished, start to sign.
[2020年 12月 18日 星期五 16:44:43 CST] Lets finalize the order.
[2020年 12月 18日 星期五 16:44:43 CST] Le_OrderFinalize='https://acme-v02.api.letsencrypt.org/acme/finalize/53165097/6806687071'
[2020年 12月 18日 星期五 16:44:46 CST] Downloading cert.
[2020年 12月 18日 星期五 16:44:46 CST] Le_LinkCert='https://acme-v02.api.letsencrypt.org/acme/cert/04470ac32d4f7cab1dd89c1721e29ad85813'
[2020年 12月 18日 星期五 16:44:48 CST] Cert success.
[2020年 12月 18日 星期五 16:44:48 CST] Your cert is in  /root/.acme.sh/*.liangdonglin.cn/*.liangdonglin.cn.cer 
[2020年 12月 18日 星期五 16:44:48 CST] Your cert key is in  /root/.acme.sh/*.liangdonglin.cn/*.liangdonglin.cn.key 
[2020年 12月 18日 星期五 16:44:48 CST] The intermediate CA cert is in  /root/.acme.sh/*.liangdonglin.cn/ca.cer 
[2020年 12月 18日 星期五 16:44:48 CST] And the full chain certs is there:  /root/.acme.sh/*.liangdonglin.cn/fullchain.cer 
``` 
## API添加DNS验证
[文档](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)
优点 acme 自动调用域名解析商API自动添加DNS解析完成验证，能自动更新。
以GoDaddy为列
登录GoDaddy账号生成自己App_KEY和App_Secret
``` bash
export GD_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export GD_Secret="asdfsdafdsfdsfdsfdsfdsafd"
acme.sh --issue --dns dns_gd -d example.com -d www.example.com
```
## 使用证书
默认证书都是放在~/.acme.sh/，不建议直接使用此目录
使用以下命令copy证书到自定义证书文件存放位置

以nginx为列
``` bash
acme.sh --installcert  -d  <domain>.com   \
        --key-file   /etc/nginx/ssl/<domain>.key \
        --fullchain-file /etc/nginx/ssl/fullchain.cer \
        --reloadcmd  "service nginx force-reload"
```
``` nginx
server {
 
        listen 80;
        server_name www.liangdonglin.cn;
        rewrite ^/(.*) https://www.liangdonglin.cn/$1 redirect; 
}

    server {

        listen       443 ssl;
        server_name  www.liangdonglin.cn;
        index index.html;
        root /app/www;
        
        ssl_certificate /etc/nginx/ssl/fullchain.cer;
        ssl_certificate_key /etc/nginx/ssl/*.liangdonglin.cn.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1.2 TLSv1.1 TLSv1;
        ssl_ciphers ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4;
        ssl_session_cache shared:SSL:10m;
        ssl_prefer_server_ciphers on;
        
        location  /{
            root html;
            index index.html index.htm;
  }
}
```
先检查nginx配置文件

``` bash
nginx -t
```
最后重启nginx

``` bash
service nginx force-reload or nginx -s reload
```

##  更新acme
### 手动更新

``` bash
acme.sh --upgrade
```
### 开启自动更新

``` bash
acme.sh  --upgrade  --auto-upgrade
```
### 关闭自动更新

``` bash
acme.sh  --upgrade  --auto-upgrade 0
```