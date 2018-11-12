
## 学习大纲
* [前言、nginx 概况](#0)
* [一、nginx负载均衡与动态负载均衡](#1)
* [二、nginx反向代理](#2)
* [三、nginx域名、备份、不可用服务器](#3)
* [四、nginx长链接](#4)
* [五、nginx页面缓存](#5)
* [六、nginx读写分离](#6)
* [七、nginx Url重写](#7)


## 前言、nginx 概况

### 1.出现原因
* 摩尔定律
* 一个连接对应一个进程，在高并发情况下，性能是瓶颈。

### 2. 优点
* 高并发 高性能 ，数千万连接
* 可扩展性好 第三方模块生态圈丰富 比如：js lua
* 高可靠性 企业内网 4个9以上
* 热部署 


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

### 5.动态负载均衡

#### 1.背景
   上述负载均衡实现，如果upstream 服务有变动的话，需要手动更改nginx upstream列表，对于新的upstream 服务上线，也不能自动更新nginx upstream 列表，因此我们需要一种服务注册，将upstream 自动注册到Nginx upstream，从而实现了服务自动发现。
   
#### 2. consul出现
consul一种分布式开源的服务注册与发现系统，通过http api 实现服务注册与发现。支持以下特性：
  * 服务注册:服务实现者 通过http api 将服务注册到consul
  * 服务发现：服务消费者 通过http api 从consul获取服务ip 和port
  * 故障检测：分tcp 和http 两种
  * K/V存储：kv存储配置信息
  * 多数据中心：避免数据中心单点故障
  * Raft算法：实现数据一致性
  
#### 3.动态负载均衡：consul + consul-template
   使用consul-template配置模板，长轮询检测服务变更，一旦发现变更，拉取consul配置来渲染模板生成Nginx实际配置，即修改Nginx upstream 列表，最后调用Nginx重启脚步，重启Nginx。
   一图胜千言：


## <span id="2">二、nginx反向代理</span>


   ### 1.理解正向代理和反向代理
   
   * 正向代理：客户端通过中间代理层访问后端服务器。例如：客户端 通过代理 访问google
   * 反向代理：将代理层作为公网地址，客户端访问代理层，代理层将请求分发给后端服务器。例如：目前大型网站几乎都是这样做的。
      
   
   ### 2.proxy_pass http配置代理，upstream实现负载均衡
   
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

### 1.域名服务器配置

~~~
upstream backend {
server a.com;
server b.com;

}
~~~

Nginx社区版中，ip地址发生变化后，upstream不会自动更新，Nginx商业版才支持动态更新。 

### 2.备份上游服务器

~~~
upstream backend {

server 192.168.0.28:8001 weight=1;
server 192.168.0.28:8002 weight=1 backup;

}

~~~
将8002端口上游服务器配置为备上游服务器，当所有主上游服务器都不存活的时候，请求会转发给备上游服务器上。

### 3.不可用上游服务器

~~~
upstream backend {
server 192.168.0.28:8001 weight=1;
server 192.168.0.28:8002 weight=2 down;
}
~~~

8002 为永久不可用服务器。使用场景：测试或者机器故障，使用down 临时摘掉该服务器。

## <span id="4">四、nginx长链接</span> 
  ### 1. 通过配置keeplive 指令配置长连接数量
  ~~~
  upstream backend {
server 192.168.0.28:8001 weight=1;
server 192.168.0.28:8002 weight=2;
keeplive 100;
}
  
  ~~~
  **注意：** 上游服务器不要忘记开启长连接的支持。
   ### 2.keeplive 如何工作？
   - 1.询问负载均衡使用哪台服务器(ip、port)
   - 2.1 轮询 “空闲连接池”，如果连接池中缓存了第1部 ip 和port，则使用该连接
   - 2.2 从 “空闲连接池” 移除该连接，并压入 “释放连接池”栈顶
   - 3. 如果“空闲连接池” 没有该连接，则创建短连接
   
   “释放连接池” 工作流程：
    - 1. 释放连接池没有要释放的连接，则需要空闲连接池腾出一个空间给新连接使用(这种情况出现于连接数超过连接池大小。)
    - 2. 从释放连接池 释放一个连接
    - 3. 将当前连接压入到 空闲连接池 栈顶供下次使用

  ### 3.总结
   * 空闲连接池 太小，连接不够用，需要不断建立连接；
   * 空闲连接池 太大，空闲连接太多，还没有使用就超时；
   
  ** 所以** 连接池大小根据场景合理设置。
  
## <span id="5">五、nginx页面缓存</span>


>Reference

* 开涛's《亿级流量网站架构核心技术》
* [51cto](http://blog.51cto.com/freeloda/1288553)
