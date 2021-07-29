## Nginx

#### 1.什么是Nginx?

Nginx是一个高性能的HTTP和反向代理服务器。同时也是一个 IMAP/POP3/SMTP 代理服务器。 官方网站:http://nginx.org。

#### 2.Nginx主要特征？

处理静态文件，索引文件以及自动索引;打开文件描述符缓冲. 无缓存的反向代理加速，简单的负载均衡和容错. FastCGI，简单的负载均衡和容错.模块化的结构。包括 gzipping, byte ranges, chunked responses,以及 SSI-filter 等filter。如果由 FastCGI 或其它代理服务器处理单页中存在的多个 SSI，则这项处理可以并行 运行，而不需要相互等待。

支持 SSL 和 TLSSNI.

Nginx 它支持内核 Poll 模型，能经受高负载的考验,有报告表明能支持高达 50,000 个并发连接数。

Nginx 具有很高的稳定性。 例如当前 apache 一旦上到 200 个以上进程，web 响应速度就明显非常缓慢了。而 Nginx 采取了分阶段资源分配技术，使得它的 CPU 与内存占用率非常低。nginx 官方表示保持 10,000 个没有活动的连接，它只占 2.5M 内存，所以类似 DOS 这样的攻击对 nginx 来说基本上是毫无用处的。

Nginx 支持热部署。它的启动特别容易, 并且几乎可以做到 7*24 不间断运行，即使运 行数个月也不需要重新启动。对软件版本进行进行热升级。

Nginx 采用 master-slave 模型,能够充分利用 SMP 的优势，且能够减少工作进程在磁 盘 I/O 的阻塞延迟。当采用 select()/poll()调用时，还可以限制每个进程的连接数。

Nginx 代码质量非常高，代码很规范，手法成熟， 模块扩展也很容易。特别值得一提的是强大的 Upstream 与 Filter 链。

Nginx 采用了一些 os 提供的最新特性如对 sendfile (Linux2.2+)，accept-filter (FreeBSD4.1+)，TCP_DEFER_ACCEPT (Linux 2.4+)的支持，从而大大提高了性能。

免费开源，可以做高并发负载均衡。

#### 3.Nginx 常用命令?

启动 nginx 。
停止 nginx -s stop 或 nginx -s quit 。
重载配置 ./sbin/nginx -s reload(平滑重启) 或 service nginx reload 。
重载指定配置文件 .nginx -c /usr/local/nginx/conf/nginx.conf 。
查看 nginx 版本 nginx -v 。
检查配置文件是否正确 nginx -t 。
显示帮助信息 nginx -h 。

#### 4.工作模式及连接数上限?

```nginx
events {

use epoll; #epoll 是多路复用 IO(I/O Multiplexing)中的一种方 式,但是仅用于 linux2.6 以上内核,可以大大提高 nginx 的性能

worker_connections 1024;#单个后台 worker process 进程的最大并发链接数

# multi_accept on; 
}
```

#### 5.Nginx负载均衡几种算法？

5种。

1.轮询模式（默认）
每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
2.权重模式
指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况
3.IP_hash模式 （IP散列）
每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
4.url_hash模式
5.fair模式
按后端服务器的响应时间来分配请求，响应时间短的优先分配。

#### 6.nginx有几种进程模型?

分为master-worker模式和单进程模式。在master-worker模式下，有一个master进程和至少一个的worker进程，单进程模式顾名思义只有一个进程。

#### 7.如何定义错误提示页面？

\# 定义错误提示页面

```nginx
error_page 500 502 503 504 /50x.html;

location = /50x.html { root /root;

}
```

#### 8.如何精准匹配路径?

location  =开头表示精准匹配

```nginx
location = /get {
#规则 A }
```

#### 9.路径匹配优先级？

多个 location 配置的情况下匹配顺序为
首先匹配 =，其次匹配^~, 其次是按文件中顺序的正则匹配，最后是交给 / 通用匹配。当 有匹配成功时候，停止匹配，按当前匹配规则处理请求。

#### 10.如何把请求转发给后端应用服务器？

```nginx
location = / {

proxy_pass http://tomcat:8080/index 

}
```

#### 11.如何根据文件类型设置过期时间？

```nginx
location ~* \.(js|css|jpg|jpeg|gif|png|swf)$ {
if (-f $request_filename) {
expires 1h;
break;
}
}
```

#### 12.禁止访问某个目录？

```nginx
location ^~/path/ {
    deny all;
}


```

#### 13.Nginx负载均衡实现过程？

首先在 http 模块中配置使用 upstream 模块定义后台的 webserver 的池子，名为 proxy-web，在池子中我们可以添加多台后台 webserver，其中状态 检查、调度算法都是在池子中配置;然后在 serverr 模块中定义虚拟主机，但是这个虚拟主 机不指定自己的 web 目录站点，它将使用 location 匹配 url 然后转发到上面定义好的 web 池子中，最后根据调度策略再转发到后台 web server 上 。

#### 14.负载均衡配置？

```nginx
Upstream proxy_nginx {

server 192.168.0.254  weight=1max_fails=2 fail_timeout=10s ; 
server 192.168.0.253 weight=2 max_fails=2fail_timeout=10s;
server192.168.0.252 backup; server192.168.0.251 down;

}

server{
 listen 80;
 server_name xiaoka.com;

location / {

 proxy_pass http:// proxy_nginx;
 proxy_set_header Host
 proxy_set_header X-Real-IP
 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

}
```

#### 15.设置超时时间？

```nginx
http {
 ……….
 keepalive_timeout 60; ###设置客户端连接保持会话的超时时间，超过这个时间，服务器会关闭该连接。 tcp_nodelay on;
 \####打开 tcp_nodelay，在包含了 keepalive 参数才有效
 client_header_timeout 15; ####设置客户端请求头读取超时时间，如果超过这个时间，客户端还没有发送任何数据， Nginx 将返回“Request time out(408)”错误
 client_body_timeout 15;

\####设置客户端请求主体读取超时时间，如果超过这个时间，客户端还没有发送任何数据， Nginx 将返回“Request time out(408)”错误
 send_timeout 15; ####指定响应客户端的超时时间。这个超过仅限于两个连接活动之间的时间，如果超过这 个时间，客户端没有任何活动，Nginx 将会关闭连接。

…… }
```

#### 16.开启压缩功能好处？坏处？

好处：压缩是可以节省带宽，提高传输效率

坏处：但是由于是在服务器上进行压缩，会消耗服务器起源

#### 17.Nginx配置文件nginx.conf有哪些属性模块?

```
worker_processes  1；                					# worker进程的数量
events {                              					# 事件区块开始
    worker_connections  1024；            				# 每个worker进程支持的最大连接数
}                                    					# 事件区块结束
http {                               					# HTTP区块开始
    include       mime.types；            				# Nginx支持的媒体类型库文件
    default_type  application/octet-stream；     		# 默认的媒体类型
    sendfile        on；       							# 开启高效传输模式
    keepalive_timeout  65；       						# 连接超时
    server {            								# 第一个Server区块开始，表示一个独立的虚拟主机站点
        listen       80；      							# 提供服务的端口，默认80
        server_name  localhost；       					# 提供服务的域名主机名
        location / {            						# 第一个location区块开始
            root   html；       						# 站点的根目录，相当于Nginx的安装目录
            index  index.html index.htm；      			# 默认的首页文件，多个用空格分开
        }          										# 第一个location区块结果
        error_page   500502503504  /50x.html；     		# 出现对应的http状态码时，使用50x.html回应客户
        location = /50x.html {          				# location区块开始，访问50x.html
            root   html；      							# 指定对应的站点目录为html
        }
    }  
    ......

```

#### 18.Nginx静态资源?

 静态资源访问，就是存放在nginx的html页面，我们也可以编写

#### 19.如何用Nginx解决前端跨域问题？

- 使用Nginx转发请求。把跨域的接口写成调本域的接口，然后将这些接口转发到真正的请求地址。

#### 20.Nginx虚拟主机怎么配置?

- 1、基于域名的虚拟主机，通过域名来区分虚拟主机——应用：外部网站
- 2、基于端口的虚拟主机，通过端口来区分虚拟主机——应用：公司内部网站，外部网站的管理后台
- 3、基于ip的虚拟主机。

#### 参考:

- 《Nginx从入门到精通》

- 《Nginx高性能Web服务器详解》

- 《深入理解nginx》

  ![WechatIMG360](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/common1.png)
