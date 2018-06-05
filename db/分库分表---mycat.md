---


---

<blockquote>
<p>系统开发中，数据库是非常重要的一个点。除了程序的本身的优化，如：SQL语句优化、代码优化，数据库的处理本身优化也是非常重要的。主从、热备、分表分库等都是系统发展迟早会遇到的技术问题问题。Mycat是一个广受好评的数据库中间件，已经在很多产品上进行使用了。希望通过这篇文章的介绍，能学会Mycat的使用。</p>
</blockquote>
<h2 id="安装">安装</h2>
<p>Mycat官网：<a href="http://www.mycat.io/">http://www.mycat.io/</a><br>
可以了解下Mycat的背景和应用情况，这样使用起来比较有信心。</p>
<p>Mycat下载地址：<a href="http://www.mycat.io/">http://dl.mycat.io/</a><br>
官网有个文档，属于详细的介绍，初次入门，看起来比较花时间。</p>
<p><strong>下载：</strong><br>
建议大家选择 1.6-RELEASE 版本，毕竟是比较稳定的版本。</p>
<p><strong>安装：</strong><br>
根据不同的系统选择不同的版本。包括linux、windows、mac,作者考虑还是非常周全的，当然，也有源码版的。（ps:源码版的下载后，只要配置正确，就可以正常运行调试，这个赞一下。）<br>
<img src="https://ws3.sinaimg.cn/large/006tNc79gy1fjfersexy7j31kw0ac0zb.jpg" alt=""></p>
<p>Mycat的安装其实只要解压下载的目录就可以了，非常简单。<br>
安装完成后，目录如下：</p>
<p>目录</p>
<p>说明</p>
<p>bin</p>
<p>mycat命令，启动、重启、停止等</p>
<p>catlet</p>
<p>catlet为Mycat的一个扩展功能</p>
<p>conf</p>
<p>Mycat 配置信息,重点关注</p>
<p>lib</p>
<p>Mycat引用的jar包，Mycat是java开发的</p>
<p>logs</p>
<p>日志文件，包括Mycat启动的日志和运行的日志。</p>
<h2 id="配置">配置</h2>
<p>Mycat的配置文件都在conf目录里面，这里介绍几个常用的文件：</p>
<pre><code>server.xml:Mycat的配置文件，设置账号、参数等

schema.xml:Mycat对应的物理数据库和数据库表的配置

rule.xml:Mycat分片（分库分表）规则
</code></pre>
<p>Mycat的架构其实很好理解，Mycat是代理，Mycat后面就是物理数据库。和Web服务器的Nginx类似。对于使用者来说，访问的都是Mycat，不会接触到后端的数据库。<br>
我们现在做一个主从、读写分离，简单分表的示例。结构如下图：<br>
<img src="https://ws2.sinaimg.cn/large/006tNc79gy1fjglurmknmj30m80ipmyr.jpg" alt=""></p>
<h3 id="数据库读写分离">数据库读写分离</h3>
<p>配置如下：</p>
<pre><code>&lt;?xml version="1.0"?&gt;
&lt;!DOCTYPE mycat:schema SYSTEM "schema.dtd"&gt;
&lt;mycat:schema xmlns:mycat="http://io.mycat/"&gt;

&lt;!-- 数据库配置，与server.xml中的数据库对应 --&gt;
    &lt;schema name="lunch" checkSQLschema="false" sqlMaxLimit="100"&gt;
        &lt;table name="lunchmenu" dataNode="dn1"  /&gt;
        &lt;table name="restaurant" dataNode="dn1"  /&gt;
        &lt;table name="userlunch" dataNode="dn1"  /&gt;
        &lt;table name="users" dataNode="dn1"  /&gt;
        &lt;table name="dictionary" primaryKey="id" autoIncrement="true" dataNode="dn1"  /&gt;

        
    &lt;/schema&gt;

&lt;!-- 分片配置 --&gt;
    &lt;dataNode name="dn1" dataHost="test1" database="lunch" /&gt;


&lt;!-- 物理数据库配置 --&gt;
    &lt;dataHost name="test1" maxCon="1000" minCon="10" balance="1"  writeType="0" dbType="mysql" dbDriver="native"&gt;
        &lt;heartbeat&gt;select user();&lt;/heartbeat&gt;
        &lt;writeHost host="hostM1" url="192.168.0.2:3306" user="root" password="123456"&gt;  
        &lt;readHost host="hostM1" url="192.168.0.3:3306" user="root" password="123456"&gt;   
        &lt;/readHost&gt;
        &lt;/writeHost&gt;
    &lt;/dataHost&gt;


&lt;/mycat:schema&gt;
</code></pre>
<p>这样的配置与前一个示例配置改动如下：<br>
删除了table分配的规则,以及datanode只有一个<br>
datahost也只有一台，但是writehost总添加了readhost,balance改为1，表示读写分离。<br>
以上配置达到的效果就是102.168.0.2为主库，192.168.0.3为从库。</p>
<p>注意：<strong>Mycat主从分离只是在读的时候做了处理，写入数据的时候，只会写入到writehost，需要通过mycat的主从复制将数据复制到readhos</strong>t，这个问题当时候我纠结了好久，数据写入writehost后，readhost一直没有数据，以为是自己配置的问题，后面才发现Mycat就没有实现主从复制的功能，毕竟数据库本身自带的这个功能才是最高效稳定的。</p>
<blockquote>
<p>至于其他的场景，如同时主从和分表分库也是支持的了，只要了解这个实现以后再去修改配置，都是可以实现的。而热备及故障专业官方推荐使用haproxy配合一起使用，大家可以试试。</p>
</blockquote>
<h2 id="使用">使用</h2>
<p>Mycat的启动也很简单，启动命令在Bin目录：</p>
<pre><code>##启动
mycat start

##停止
mycat stop

##重启
mycat restart
</code></pre>
<p>如果在启动时发现异常，在logs目录中查看日志。</p>
<ul>
<li>wrapper.log 为程序启动的日志，启动时的问题看这个</li>
<li>mycat.log 为脚本执行时的日志，SQL脚本执行报错后的具体错误内容,查看这个文件。mycat.log是最新的错误日志，历史日志会根据时间生成目录保存。</li>
</ul>
<p>mycat启动后，执行命令不成功，可能实际上配置有错误，导致后面的命令没有很好的执行。</p>
<p>Mycat带来的最大好处就是使用是完全不用修改原有代码的，在mycat通过命令启动后，你只需要将数据库连接切换到Mycat的地址就可以了。如下面就可以进行连接了：</p>
<pre><code> mysql -h192.168.0.1 -P8806 -uroot -p123456
</code></pre>
<p>连接成功后可以执行sql脚本了。<br>
所以，可以直接通过sql管理工具（如：navicat、datagrip）连接，执行脚本。我一直用datagrip来进行日常简单的管理，这个很方便。</p>
<p>Mycat还有一个管理的连接，端口号是9906.</p>
<pre><code> mysql -h192.168.0.1 -P9906 -uroot -p123456
</code></pre>
<p>连接后可以根据管理命令查看Mycat的运行情况，当然，喜欢UI管理方式的人，可以安装一个Mycat-Web来进行管理，有兴趣自行搜索。</p>
<p>简而言之，开发中使用Mycat和直接使用Mysql机会没有差别。</p>
<h2 id="常见问题">常见问题</h2>
<p>使用Mycat后总会遇到一些坑，我将自己遇到的一些问题在这里列一下，希望能与大家有共鸣：</p>
<ul>
<li>
<p>Mycat是不是配置以后，就能完全解决分表分库和读写分离问题？<br>
Mycat配合数据库本身的复制功能，可以解决读写分离的问题，但是针对分表分库的问题，不是完美的解决。或者说，至今为止，业界没有完美的解决方案。<br>
分表分库写入能完美解决，但是，不能完美解决主要是联表查询的问题，Mycat支持两个表联表的查询，多余两个表的查询不支持。 其实，很多数据库中间件关于分表分库后查询的问题，都是需要自己实现的，而且节本都不支持联表查询，Mycat已经算做地非常先进了。<br>
分表分库的后联表查询问题，大家通过合理数据库设计来避免。</p>
</li>
<li>
<p>Mycat支持哪些数据库，其他平台如 .net、PHP能用吗？<br>
官方说了，支持的数据库包括MySQL、SQL Server、Oracle、DB2、PostgreSQL 等主流数据库，很赞。<br>
尽量用Mysql,我试过SQL Server，会有些小问题，因为部分语法有点差异。</p>
</li>
<li>
<p>Mycat 非JAVA平台如 .net、PHP能用吗？<br>
可以用。这一点MyCat做的也很棒。</p>
</li>
</ul>
<h2 id="reference">reference:</h2>
<blockquote>
<p>《Mycat权威指南》：  <a href="http://www.mycat.io/document/Mycat_V1.6.0.pdf">http://www.mycat.io/document/Mycat_V1.6.0.pdf</a></p>
</blockquote>
<blockquote>
<p>官网 ：<a href="http://www.mycat.io/">http://www.mycat.io/</a></p>
</blockquote>

