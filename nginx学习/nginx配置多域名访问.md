## 修改nginx配置

1. 我们有个很实际的需求也是我一个前端去学习nginx的原因
>  我们现在就一个域名，一个ip，但是我想跑好几个web服务怎么办呢

>  解决这个问题当然需要我们去修改nginx.conf这个文件了；

  
2. 首先我们去将一个顶级域名分解出多个二级域名出来
>  例如我现在的ip是 www.shawVi.com

>  进入域名控制面板，然后就可以配置了，我现在配置以下两个域名
    a. www.dev.shawVi.com
    b. www.test.shawVi.com

>  这两个域名都指向一个ip，有了这两个ip之后我们开始修改我们的nginx.conf文件

3.  
```
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        root   html;
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
>   其他我们不管，我们就来改这个server这个配置项，然后关于php的注释其实可以删了，没啥卵用；
> listen： 表示监听的端口 80 <br >server_name: 就是主机名，那么ok，我们就把刚刚分解出来的两个域名给拿过来<br >  把 localhost 改成 www.dev.shawVi.com

>   这个改完以后,我们就来改 localtion ／{ },root代表的就是根目录，我们现在去linux里面的一个文件夹里创建两个目录dev，test，名字随意，然后我们可以在里面创建一个html文件，

>   接下来我们在创建一个server对象，把刚刚那个copy一份放下面，将 把 localhost 改成 www.test.shawVi.com即可，然后localtion里面就是 ／home／test ，里面同样放一个html文件，

然后来看下我们修改以后的nginx.conf文件：
```
server {
        listen       80;
        server_name  www.dev.shawVi.com;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   /home/dev;
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

server {
    listen       80;
    server_name  www.test.shawVi.com;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        root   /home/test;
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

<b>温馨提示： 这仅仅是里面server的，其他的原封不动不需要改，就是创建两个server对象<b>


4. 我们现在去访问   www.dev.shawVi.com,www.test.shawVi.com就会看见两个不一样的页面了，那说明我们就搞定了这个功能了；


