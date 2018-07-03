---


---

<h3 id="etcd简介">etcd简介</h3>
<p>etcd是一个高可用的分布式键值(key-value)数据库。etcd内部采用raft协议作为一致性算法，etcd基于Go语言实现。</p>
<h3 id="etcd特点：">etcd特点：</h3>
<p>简单：安装配置简单，而且提供了HTTP API进行交互，使用也很简单<br>
安全：支持SSL证书验证<br>
快速：根据官方提供的benchmark数据，单实例支持每秒2k+读操作<br>
可靠：采用raft算法，实现分布式系统数据的可用性和一致性<br>
etcd项目地址：<a href="https://github.com/coreos/etcd/">https://github.com/coreos/etcd/</a><br>
etcd应用场景</p>
<h3 id="etcd应用场景用于服务发现">etcd应用场景用于服务发现</h3>
<p>服务发现(Service Discovery)要解决的是分布式系统中最常见的问题之一，即在同一个分布式集群中的进程或服务如何才能找到对方并建立连接。</p>
<p>从本质上说，服务发现就是要了解集群中是否有进程在监听upd或者tcp端口，并且通过名字就可以进行查找和链接。</p>
<p>要解决服务发现的问题，需要下面三大支柱，缺一不可。</p>
<p><strong>1.一个强一致性、高可用的服务存储目录。</strong><br>
基于Ralf算法的etcd天生就是这样一个强一致性、高可用的服务存储目录。</p>
<p><strong>2.一种注册服务和健康服务健康状况的机制。</strong><br>
用户可以在etcd中注册服务，并且对注册的服务配置key TTL，定时保持服务的心跳以达到监控健康状态的效果。</p>
<p><strong>3.一种查找和连接服务的机制。</strong><br>
通过在etcd指定的主题下注册的服务业能在对应的主题下查找到。为了确保连接，我们可以在每个服务机器上都部署一个proxy模式的etcd，这样就可以确保访问etcd集群的服务都能够互相连接。</p>
<h3 id="etcd-安装">etcd 安装</h3>
<p>etcd在生产环境中一般推荐<a href="https://coreos.com/etcd/docs/latest/v2/clustering.html">集群方式部署</a>。本文定位为入门，主要讲讲单节点安装和基本使用。</p>
<p>etcd目前默认使用2379端口提供HTTP API服务，2380端口与服务端通信。<br>
因为etcd是go语言编写的，安装只需要<a href="https://github.com/coreos/etcd/releases">下载对应的二进制文件</a>，并放到合适的路径就行。</p>
<p><strong>安装环境：</strong> linux/amd64 3.10</p>
<pre><code>[root@iz2ze71m2nxbuwib8avpwsz ~]# wget https://github.com/coreos/etcd/releases/download/v3.3.8/etcd-v3.3.8-linux-amd64.tar.gz

(ps:部分安装过程)
在连接 github-production-release-asset-2e65be.s3.amazonaws.com (github-production-release-asset-2e65be.s3.amazonaws.com)|52.216.81.152|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：11281861 (11M) [application/octet-stream]
正在保存至: “etcd-v3.3.8-linux-amd64.tar.gz”

100%[======================================&gt;] 11,281,861  1.69MB/s 用时 15s

2018-07-03 17:16:13 (740 KB/s) - 已保存 “etcd-v3.3.8-linux-amd64.tar.gz” [11281861/11281861])

</code></pre>
<pre><code>[root@iz2ze71m2nxbuwib8avpwsz ~]# tar xzvf etcd-v3.3.8-linux-amd64.tar.gz
[root@iz2ze71m2nxbuwib8avpwsz ~]# mv etcd-v3.3.8-linux-amd64 /opt/etcd

[root@iz2ze71m2nxbuwib8avpwsz ~]# cd /opt/etcd/
[root@iz2ze71m2nxbuwib8avpwsz etcd]# ls
Documentation  etcd  etcdctl  README-etcdctl.md  README.md  READMEv2-etcdctl.md
[root@iz2ze71m2nxbuwib8avpwsz etcd]# etcd -version
etcd Version: 3.2.18
Git SHA: eddf599
Go Version: go1.9.4
Go OS/Arch: linux/amd64
</code></pre>
<h3 id="etcd-启动">etcd 启动</h3>
<pre><code>[root@iz2ze71m2nxbuwib8avpwsz etcd]# ./etcd
2018-07-03 17:29:44.513419 I | etcdmain: etcd Version: 3.3.8
2018-07-03 17:29:44.513463 I | etcdmain: Git SHA: 33245c6b5
2018-07-03 17:29:44.513467 I | etcdmain: Go Version: go1.9.7
2018-07-03 17:29:44.513471 I | etcdmain: Go OS/Arch: linux/amd64

.....

2018-07-03 17:29:45.629494 I | embed: ready to serve client requests
2018-07-03 17:29:45.630040 N | embed: serving insecure client requests on 127.0.0.1:2379, this is strongly discouraged!

</code></pre>
<h3 id="etcd-基本使用">etcd 基本使用</h3>
<ol>
<li>etcd 对外通过 HTTP API 对外提供服务</li>
<li>etcdctl命令行客户端</li>
</ol>
<p><strong>前提</strong>：启动etcd 后，使用api 或者 etcdctl命令</p>
<h4 id="etcdctl">etcdctl</h4>
<p><strong>数据库操作</strong></p>
<p>数据库操作围绕对键值和目录的CRUD完整生命周期的管理。</p>
<p>etcd在键的组织上采用了层次化的空间结构(类似于文件系统中目录的概念)，用户指定的键可以为单独的名字，如:testkey，此时实际上放在根目录/下面，也可以为指定目录结构，如/cluster1/node2/testkey，则将创建相应的目录结构。</p>
<p>注：CRUD即Create,Read,Update,Delete是符合REST风格的一套API操作。</p>
<p><strong>set</strong><br>
指定某个键的值。例如:</p>
<pre><code>[root@iz2ze71m2nxbuwib8avpwsz ~]# etcdctl set /testdir/testkey "Hello world"
Hello world
[root@iz2ze71m2nxbuwib8avpwsz ~]# etcdctl get /testdir/testkey
Hello world
[root@iz2ze71m2nxbuwib8avpwsz ~]#
</code></pre>
<p><strong>get</strong><br>
获取指定键的值。例如：</p>
<pre><code>[root@iz2ze71m2nxbuwib8avpwsz ~]# etcdctl get /testdir/testkey
Hello world

</code></pre>
<p><strong>update</strong></p>
<pre><code>
[root@iz2ze71m2nxbuwib8avpwsz ~]# etcdctl update /testdir/testkey "hi etcd"
hi etcd
</code></pre>
<p><strong>delete</strong></p>
<pre><code>
[root@iz2ze71m2nxbuwib8avpwsz ~]# etcdctl rm /testdir/testkey
PrevNode.Value: hi etcd
[root@iz2ze71m2nxbuwib8avpwsz ~]# etcdctl get /testdir/testkey
Error:  100: Key not found (/testdir/testkey) [6]
[root@iz2ze71m2nxbuwib8avpwsz ~]#
</code></pre>
<p>此外还有 mk 、mkdir、setdir、updatedir、rmdir 、ls 等</p>
<p><strong>非数据库操作</strong></p>
<p><strong>backup</strong><br>
备份etcd的数据。</p>
<pre><code>$ etcdctl backup --data-dir /var/lib/etcd  --backup-dir /home/etcd_backup
</code></pre>
<p>支持的选项包括:</p>
<pre><code>--data-dir  etcd的数据目录
--backup-dir 备份到指定路径
</code></pre>
<p><strong>watch</strong></p>
<p>监测一个键值的变化，一旦键值发生更新，就会输出最新的值并退出。</p>
<p>例如:用户更新testkey键值为Hello watch。</p>
<pre><code>$ etcdctl get /testdir/testkey
Hello world
$ etcdctl set /testdir/testkey "Hello watch"
Hello watch
$ etcdctl watch testdir/testkey
Hello watch

</code></pre>
<p>支持的选项包括:</p>
<pre><code>--forever  一直监测直到用户按CTRL+C退出
--after-index '0' 在指定index之前一直监测
--recursive 返回所有的键值和子键值
</code></pre>
<p><strong>exec-watch</strong><br>
监测一个键值的变化，一旦键值发生更新，就执行给定命令。</p>
<p>例如：用户更新testkey键值。</p>
<pre><code>$ etcdctl exec-watch testdir/testkey -- sh -c 'ls'
config    Documentation  etcd  etcdctl  README-etcdctl.md  README.md  READMEv2-etcdctl.md

</code></pre>
<p>支持的选项包括:</p>
<pre><code>--after-index '0' 在指定 index 之前一直监测
--recursive 返回所有的键值和子键值
</code></pre>
<p><strong>member</strong><br>
通过list、add、remove命令列出、添加、删除etcd实例到etcd集群中。</p>
<p>查看集群中存在的节点</p>
<pre><code>$ etcdctl member list
8e9e05c52164694d: name=dev-master-01 peerURLs=http://localhost:2380 clientURLs=http://localhost:2379 isLeader=true

</code></pre>
<p>删除集群中存在的节点</p>
<pre><code>$ etcdctl member remove 8e9e05c52164694d
Removed member 8e9e05c52164694d from cluster
</code></pre>
<p>向集群中新加节点</p>
<pre><code>$ etcdctl member add etcd3 http://192.168.1.100:2380
Added member named etcd3 with ID 8e9e05c52164694d to cluster
</code></pre>
<h4 id="etcd-api">etcd API</h4>
<p>etcd 对外通过 HTTP API 对外提供服务，这种方式方便测试（通过 curl 或者其他工具就能和 etcd 交互），也很容易集成到各种语言中（每个语言封装 HTTP API 实现自己的 client 就行）。</p>
<p>这个部分，我们就介绍 etcd 通过 HTTP API 提供了哪些功能，并使用 <a href="https://github.com/jakubroztocil/httpie">httpie</a> 来交互（当然你也可以使用 curl 或者其他工具）。</p>
<p>获取 etcd 服务的版本信息</p>
<pre><code>➜  http http://127.0.0.1:2379/version

</code></pre>
<p><strong>key 的增删查改</strong><br>
etcd 的数据按照树形的结构组织，类似于 linux 的文件系统，也有目录和文件的区别，不过一般被称为 nodes。数据的 endpoint 都是以 /v2/keys 开头（v2 表示当前 API 的版本），比如 /v2/keys/names/cizixs 。</p>
<p>要创建一个值，只要使用 PUT 方法在对应的 url endpoint 设置就行。如果对应的 key 已经存在， PUT 也会对 key 进行更新。</p>
<pre><code>➜  http PUT http://127.0.0.1:2379/v2/keys/message value=="hello, etcd"

</code></pre>
<p>上面这个命令通过 PUT 方法把 /message 设置为 hello, etcd 。返回的格式中，各个字段的意义是：</p>
<ul>
<li>action ：请求出发的动作，这里因为是新建一个 key 并设置它的值，所以是 set</li>
<li>node.key ：key 的 HTTP 路径</li>
<li>node.value ：请求处理之后，key 的值</li>
<li>node.createdIndex ： createdIndex 是一个递增的值，每次有 key 被创建的时候会增加</li>
<li>node.modifiedIndex ：同上，只不过每次有 key 被修改的时候增加</li>
</ul>
<p>此外还有 get、delete。</p>
<h2 id="consul的优势">consul的优势</h2>
<p>1.封装了服务发现的api，开发调用非常简单<br>
2.提供了健康检查功能<br>
3.使用了raft算法保证了一致性，比复杂的paxos算法更直接，相比而言，zk采用的是paxos，二etcd采用的是raft<br>
4.支持多数据中心，保证多机房使用。<br>
5.支持 http 和 dns 协议接口.，zookeeper 的集成较为复杂,，etcd 只支持 http 协议<br>
6.官方提供web管理界面, etcd 无此功能</p>
<blockquote>
<p>reference:</p>
</blockquote>
<ul>
<li><a href="https://github.com/coreos/etcd/">https://github.com/coreos/etcd/</a></li>
<li><a href="https://blog.csdn.net/fei33423/article/details/79273367">https://blog.csdn.net/fei33423/article/details/79273367</a></li>
<li><a href="http://chuansong.me/n/1760466551708">http://chuansong.me/n/1760466551708</a></li>
<li><a href="https://www.tuicool.com/articles/UJbMZrj">https://www.tuicool.com/articles/UJbMZrj</a></li>
</ul>

