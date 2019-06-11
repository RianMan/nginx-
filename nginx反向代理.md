##配置
1.首先还是修改nginx.conf的配置文件
    >server {
            listen 80;
            # 监听的域名是 test.com，即用户访问 test.com 时，触发这个配置
            server_name test.com;

            location / {
                #这个就是我们买的云服务器的3000端口的服务，可以用node跑一个服务
                proxy_pass http://12.333.11.1:3000/;
            }
        }

        第二种配置是多个服务器列表的反向代理：
            # 配置服务器列表
            upstream server_list{
                server 103.94.185.215:3000;
                server 103.94.185.215:4000;
            }
            server {
                listen 80;
                server_name test.com;
                location / {
                    #这种配置多个
                    proxy_pass server_list;
                }
            }
        访问 test.com的时候会随机导到server_list的服务；
        这样基本就完成了基本的反向代理了。。。。

2.按照1的配置来说，我现在有问题，那我怎么知道我会访问到那台服务器呢？
    >  现在我们可以配置多个服务器列表
        upstream server_a{
            server 103.94.185.215:3000;
        }
            upstream server_b{
            server 103.94.185.215:4000;
        }
        server {
            listen 80;
            server_name test.com;
            #当请求test.com/a 就会跑到3000端口的服务
            location /a/ {
                #这种配置多个
                proxy_pass server_a;
            }
            #当请求test.com/b 就会跑到4000端口的服务
            location /b/ {
                #这种配置多个
                proxy_pass server_b;
            }
        }