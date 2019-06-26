# 配置https流程

1. 首先我们找到域名，给这个域名申请一个https的证书，网上有很多教程，当审批下来以后，得到两个文件
申请的域名.crt 和 .key这两个文件

2. 接下来我们通过上传到Linux 的远程服务器中去，记住自己上传的目录，一般放在nginx的配置目录即可，这个没有强制，只需要自己记住即可。

3. 然后我们就需要去修改nginx.conf这个文件的配置了，首先我贴出之前的配置一个server配置；

```
server {
        listen       80;
        server_name  www.test.jwjy1995.site;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   /home/test/build;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
现在当我们我们想去把www.test.jwjy1995.site变为https的话，其实我们只需要加一个配置
<b>rewrite ^(.*) https://$server_name$1 permanent</b>
```
server {
    listen       80;
    server_name  www.test.jwjy1995.site;
    rewrite ^(.*) https://$server_name$1 permanent;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    #error_page  404              /404.html;

    #redirect server error pages to the static page /50x.html
    
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }
}
```
那接下来我们还需要配置我们的证书让其彻底变为https

4. 找到最下面有一个https server的设置，我们将设置改为
```
# HTTPS server
#
server {
    listen       443 ssl;
    server_name  www.test.jwjy1995.site;

    ssl on;
    # 这个即位刚刚下载的两个证书的配置
    ssl_certificate      /usr/local/nginx/www.test.jwjy1995.site.crt;
    ssl_certificate_key  /usr/local/nginx/www.test.jwjy1995.site.key;

    # 这一坨应该是https的相关配置，没有研究具体含义
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  5m;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    server_tokens off; 

    # 这个很关键，就是我们之前的访问路径了
    location / {
        root   /home/test;
        index  index.html index.htm;
    }       
}
```

5. 配置完成以后，输入nginx -t 去测试nginx配置是否正确，如果没有报错即ok；如果失败那需要找些原因。<b>特别注意，因为我们配置的https是443端口，那我们的linux云主机一定要将这个端口开放出去，不然不报错也访问不了，找不到原因</b>
