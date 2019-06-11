## 安装
1. 首先需要购买一个linux云服务器，所以所有的流程都基于linux而来；

2. 首先需要安装一些nginx的环境，
> yum -y install gcc automake autoconf libtool make（安装make）
        
> yum install gcc gcc-c++（安装g++）<br >并不知道它们干嘛用的，但是nginx中文官方文档是这么说的（https://wizardforcel.gitbooks.io/nginx-doc/content/Text/1.3_install.html）
    
3. 接着我们需要正式安装了
>cd /usr/local/src （随意进入一个目录，我们就以此目录进行安装，例如我们在windows装一个游戏一样）

>安装PCRE库
```
wget ftp://ftp.csx.cam.ac.uk/pub/software/
programming/pcre/pcre-8.34.tar.gz 
tar -zxvf pcre-8.34.tar.gz
cd pcre-8.34
./configure
make
make install
```

>安装zlib库
```
cd /usr/local/src
wget http://zlib.net/zlib-1.2.8.tar.gz
tar -zxvf zlib-1.2.8.tar.gz
cd zlib-1.2.8
./configure
make
make install
```

>安装ssl

```
cd /usr/local/src
wget http://www.openssl.org/source/openssl-1.0.1c.tar.gz
tar -zxvf openssl-1.0.1c.tar.gz

```

### 特别提示一下
<b>如果wget下载之后你无法发现目录里有那个文件，基本是是你没有安装到那个版本的库，所以我们就需要去库的官网看下那个版本是有的，就不会出现安装不到问题<b>

>上面安装完毕以后开始安装nginx 
* cd /usr/local/src
* wget http://nginx.org/download/nginx-1.14.2.tar.gz
* tar -zxvf nginx-1.14.2.tar.gz
* cd nginx-1.14.2(去nginx官网看下最新的稳定版本是多少，然后在选择下载）

到现在为止，我们所需要的包都安装好了，那么我们现在进行配置了

>一个个的敲，然后注意你所安装包的版本哦！！！！！切记
```
./configure --sbin-path=/usr/local/nginx/nginx \
--conf-path=/usr/local/nginx/nginx.conf \
--pid-path=/usr/local/nginx/nginx.pid \
--with-http_ssl_module \
--with-pcre=/usr/local/src/pcre-8.34 \
--with-zlib=/usr/local/src/zlib-1.2.8 \
--with-openssl=/usr/local/src/openssl-1.0.1c
```
敲完以后，再敲 make，你需要等一会，然后再敲 make install

> 以上都很顺利的情况，我们通过ls就能看见nginx的目录了


## 启动

1. /usr/local/nginx/nginx 运行这个命令基本就启动完成了，然后访问云服务器的ip就能看到nginx的欢迎页面了
netstat -ano|grep 80 看下80端口的启动情况，如果80端口被占用了，是启动不成功的，那我们需要干掉80端口
    
2. 接下来我们配置环境变量，现在你输入nginx是没有任何反应的，我们通过ln命令去连接
ln -s /usr/local/nginx/nginx  /usr/bin
<ln -s 'a' 'b'>命令：
{
    a:(这是nginx的目录所在地方，看看上面启动命令是不是同一个地方),
    b:(这个是所有环境变量的地方，如果你不想连接，那么你就进入这个),
}
完成以后你在命令行敲入nginx就会出现提示了，说明link成功了
如果你不想这个连接了，你可以进入usr／bin文件通过rm 删除那个连接
-----nginx的命令-----
nginx  #启动nginx
nginx -s quit  #快速停止nginx
nginx -V #查看版本，以及配置文件地址
nginx -v #查看版本
nginx -s reload|reopen|stop|quit   #重新加载配置|重启|快速停止|安全关闭nginx
nginx -h #帮助