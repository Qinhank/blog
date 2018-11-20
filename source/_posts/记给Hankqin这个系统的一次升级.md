---
title: 记给Hankqin这个系统的一次升级
date: 2018-11-20 09:34:29
tags: nginx 腾讯云 https
categories: 技术
---
最近跟朋友聊了些关于技术的实践性，受到启发，于是将我的个人系统——Hankqin进行了一次小方面的改动，包括：将接口和页面升级为https、将页面全托管于腾讯云的cdn。
## 升级Https
  升级到https其实对我这个系统没多大影响，反正还有些不方便，但是我觉得还是值得的，毕竟在前端方面对安全的防范本就不够，能开的安全方式就给开了吧。  
  升级过程是这样的，先申请证书，我一共申请了3个，阿里云2个，腾讯云1个，为什么阿里云需要2个呢，因为阿里云的免费证书只能绑定一个域名，而我接口有两个，而且我还不想合并，所以就申请了两个，分别对应  
  https://api.hankqin.com  
  和  
  https://music.hankqin.com  
  腾讯云的那一个则对应cdn里的域名，分别是  
  https://www.hankqin.com  
  和  
  https://hankqin.com  
  升级过程腾讯云其实还算简单，概括的说就是点点点就好了。  
  阿里云因为是服务器端的，所以要下载证书，因为我用的Ngnix，所以配置如下：  
  ```ngnix
  server 
    {
        listen 443;
        server_name api.hankqin.com; #填写绑定证书的域名
        ssl on;
        root html;
    	index index.html index.htm;
        ssl_certificate cert/cert-xxx_api.hankqin.com.crt;
        ssl_certificate_key cert/cert-xxx_api.hankqin.com.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        location / {
            proxy_pass http://xxx.xxx.xxx.xxx::7001;
            proxy_redirect default;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
  ```

## 托管CDN
这个实现起来其实很简单，但是值得一提的是托管到cdn后更新起来还是挺麻烦的，因为本身cdn的缓存就比较久，再配合代码自身的缓存，所以想实时看到修改后的效果还是有点困难。  
不过全部托管也有好处，那就是流量全部交由腾讯云，这样一来方便统计数据二来给服务器减小了很多压力而且访问速度还大大提升，也算是得大于失吧。  