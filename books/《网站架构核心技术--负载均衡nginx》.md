## 学习大纲
* [一、nginx负载均衡与动态负载均衡](#1)
* [二、nginx反向代理](#2)
* [三、nginx域名、备份、不可用服务器](#3)
* [四、nginx长链接](#4)
* [五、nginx页面缓存](#5)
* [六、nginx读写分离](#6)
* [七、nginxUrl重写](#7)


## <span id="1">一、nginx负载均衡与动态负载均衡</span>

   对于负载均衡我们要关心的几个方面如下：
1. 上游服务器配置：使用upstream server配置上游服务器
2. 负载均衡算法：配置多个服务器均衡机制
3. 失败重试机制：超时或者不存活是否重试
4. 心跳检测：上游服务器心跳检测

一图胜千言：
![Nginx](https://github.com/kgtom/daily-life/blob/master/books/images/Nginx.jpg)


### 1.upstream 配置

~~~

upstream backend {
server 192.168.0.28:8001 weight=1;
server 192.168.0.28:8002 weight=2;

}
~~~

**配置参数:**

* IP地址和端口： 配置服务器IP 和端口
* 权重: 默认1，权重越高，分配给该服务器越多


配置 proxy_pass处理用户请求
~~~
location /{

proxy_pass http:// xxx.com;
}
~~~


### 2.负载均衡算法

负载均衡解决用户请求到来时，如何选择upstream server 进行处理，默认round-robin(轮询)，同时支持其他算法：

   * round-robin：轮询，可以配置weight 实现基于权重的轮询
   * ip_hash: 根据客户IP进行负载，即相同IP将负载到同一个upstream server

        ~~~
          upstream backend {
            ip_hash;
            server 192.168.0.28:8001 weight=1;
            server 192.168.0.28:8002 weight=2;

            }
        ~~~
   * hash key:对某一个key进行哈希或者使用一致性哈希算法


        **哈希算法：针对url进行负载**
        ~~~
          upstream backend {
            hash $url;
            server 192.168.0.28:8001 weight=1;
            server 192.168.0.28:8002 weight=2;

            }
        ~~~
        
        **一致性哈希算法：consistent_key 动态指定进行负载**
        ~~~
          upstream backend {
            hash $consistent_key consistent;
            server 192.168.0.28:8001 weight=1;
            server 192.168.0.28:8002 weight=2;

            }
        ~~~
   * least_conn: 基于最小活跃连接负载到这个机器上

    
   * least_time:商业版有，基于最小平均相应时间



### 3.失败重试

    配置两部分： upstream server 和 proxy_pass

    upstream backend {
       
    server 192.168.0.28:8001 weight=1;
    server 192.168.0.28:8002 weight=2;

    }
    
   

### 4.心跳检查
   Nginx 对上游服务器的心跳检查默认采用惰性策略，Nginx 商业版提供 health_check 进行主动检查。我们可以集成nginx_upstream_check_module模块进行主动检查，它包括两种：
   * tcp心跳检查
   * http心跳检查

~~~
 upstream backend {
           
            server 192.168.0.28:8001 weight=1;
            server 192.168.0.28:8002 weight=2;
            check interval=3000 rise=1 fall=3 timeout=2000 type=tcp;
            }

~~~

**tcp心跳：参数解读：**
 * interval:检查间隔时间，此处配置没3秒一次；
 * fall:检查失败多少次后，认为该服务器不存活；
 * rise:检查成功多少次后，认为该服务器存活；
 * timeout:检查请求超时时间的配置；


~~~
 upstream backend {
           
            server 192.168.0.28:8001 weight=1;
            server 192.168.0.28:8002 weight=2;
            check interval=3000 rise=1 fall=3 timeout=2000 type=http;
            check_http_send "HEAD /status HTTP/1.0\r\n\r\n";
            check_http_expeck_alive http_2xx http_3xx;
            
            }

~~~

**http心跳参数解读：**
* check_http_send:检查时发的http请求内容；
* check_http_expect_alive:当上游服务器返回匹配的响应状态码时，则认为服务器存活；

**注意：** 配置间隔时间不能太短，否则可能因为配置心跳检查包太多，服务器挂掉，此外需要配置合理的超时时间。


## <span id="2">二、nginx反向代理</span>


   ### 1.理解正向代理和反向代理
   
   * 正向代理：客户端通过中间代理层访问后端服务器。例如：客户端 通过代理 访问google
   * 反向代理：将代理层作为公网地址，客户端访问代理层，代理层将请求分发给后端服务器。例如：目前大型网站几乎都是这样做的。
      
   
   ### 2.proxy_pass_http配置代理，upstream实现负载均衡
   
  ~~~
  upstream backend {

  server 192.168.0.28:8001 weight=1;
  server 192.168.0.28:8002 weight=2;                                    

 }


server {

    listen       80;
    server_name  www.xxx.com;    #访问的域名，如果有多个，用逗号分开
    charset utf8;
    location / {

        proxy_pass       http://backend; 

        proxy_set_header Host      $host;

        proxy_set_header X-Real-IP $remote_addr;

        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

      }

   }

}
   ~~~


## <span id="3">三、nginx域名、备份、不可用服务器</span>


>Reference

* 开涛's《亿级流量网站架构核心技术》
* [51cto](http://blog.51cto.com/freeloda/1288553)
