---


---

<h2 id="nsq理论实践篇">nsq理论实践篇</h2>
<h3 id="、为什么要使用消息队列">1、为什么要使用消息队列?</h3>
<p><strong>回答</strong>:基于最主要的应用场景，即六个字:<strong>解耦、异步、削峰</strong><br>
与其他MQ 对比：</p>
<p><img src="http://5b0988e595225.cdn.sohucs.com/images/20170823/e6988c5048a9499ab24d13232d31f09f.jpeg" alt=""></p>
<p>图 22</p>
<p>接着，我们简单对比一下开源的其他 MQ系统的特点。如图 22 所示，我对比了三个常用的 MQ系统，绿色部分是我们改造之后的特性，原本没有。开源生态 NSQ 做得还可以，虽然没有 Kafka 那么强大，但是它本身的文档和客户端比较丰富。可靠性这一块，Kafka 本身是没有强保证，它可能在各种测试情况下丢数据，所以一般用于日志系统，日志系统丢几条数据是没有关系的。Kafka 最新的版本会有一些改进了，但是毕竟还没有发布，没办法说它的可靠性有多高。</p>
<p>Kafka 批量写入的性能非常高，但是实际业务大部分情况下，一个客户端不可能批量写入，大部分都是几百个客户端同时写入，这种情况下每个客户端只有几条消息。所以这种情况下的性能比较，Kafka 和其它两个产品还是有一定差距。</p>
<p>RocketMQ 本身的灵活性比较小，因为它不能按照 Topic 配置参数，比如 A Topic 是 2 个副本，B Topic 是 4 个副本。不能这样配，只能大家都是三个副本，参数也都是一样的，它的灵活性比较低。</p>
<p>RocketMQ 有提供严格顺序的功能，但是在我们测试过程中，可能出现乱序的情况。当然，我们用的开源版，和生产版本可能不太一样。堆积能力和 Topic 设计有关系的，官方也说它堆积能力没有那么好。消息跟踪就是我们做的 Tracing 系统，RocketMQ 和 NSQ 都做得还不错。</p>
<h3 id="nsq组件构成">2.nsq组件构成</h3>
<p>nsq有三个组件以及辅助的几个工具构成。<br>
<strong>nsqd</strong><br>
nsqd 是一个守护进程，负责接收，排队，投递消息给客户端。<br>
它可以独立运行，不过通常它是由 nsqlookupd 实例所在集群配置的（它在这能声明 topics 和 channels，以便大家能找到）。</p>
<ul>
<li>服务启动后有两个端口：一个给客户端，另一个是 HTTP API。还能够开启HTTPS。</li>
<li>同一台服务器启动多个nsqd，要注意端口和数据路径必须不同，包括：–lookupd-tcp-address、 -tcp-address、–data-path。</li>
<li>删除topic、channel需要http api调用。</li>
</ul>
<p><strong>nsqlookupd</strong><br>
nsqlookupd 是守护进程，负责管理拓扑信息并提供最终一致性的发现服务。客户端通过查询 nsqlookupd 来发现指定话题（topic）的生产者，并且 nsqd 节点广播话题（topic）和通道（channel）信息。</p>
<ul>
<li>该服务运行后有两个端口：TCP 接口，nsqd 用它来广播；<em>HTTP 接口，客户端用它来发现和管理。</em></li>
<li>在生产环境中，为了高可用，最好部署三个nsqlookupd服务。</li>
</ul>
<p><strong>nsqadmin</strong><br>
nsqadmin 是一套 WEB UI，用来汇集集群的实时统计，并执行不同的管理任务。<br>
运行后，能够通过4171端口查看并管理topic和channel。</p>
<ul>
<li>通常只需要运行一个。</li>
</ul>
<p><strong>utilities</strong><br>
常见基础功能、数据流处理工具，如nsq_stat、nsq_tail、nsq_to_file、nsq_to_http、nsq_to_nsq、to_nsq</p>
<h3 id="nsq组件构成-1">3.nsq组件构成</h3>
<p><strong>Topic 和 Channel</strong></p>
<p>其实nsqd相当于kafka当中的分区，channel和consumers客户端的多个连接 相当于kafka的消费组，但nsq比kafka使用方式便捷概念上更容易理解<br>
抛开与kafka的对比，nsq的topic 可以设置多个channel，因为有可能有多个业务方需要定值topic的消息，这样互不影响，<br>
当然一个消息会发送topic下的所有channel,然后会分配到不同客户端的连接上，如下图。</p>
<p><img src="https://img-blog.csdn.net/20180118191611509?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2s5MzYzMjE3MzI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt=""></p>
<h3 id="nsq集群部署">3.nsq集群部署</h3>
<p><strong>消息创建与接收</strong></p>
<ul>
<li>发布者<br>
消息发布，只能面向具体的nsqd服务进行。在API中对应的是nsq.Producer,直接初始化，就可以用了，非常简单：</li>
</ul>
<pre class=" language-go"><code class="prism  language-go">    config <span class="token operator">:=</span> nsq<span class="token punctuation">.</span><span class="token function">NewConfig</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
    p<span class="token punctuation">,</span> err <span class="token operator">:=</span> nsq<span class="token punctuation">.</span><span class="token function">NewProducer</span><span class="token punctuation">(</span><span class="token string">"127.0.0.1:4150"</span><span class="token punctuation">,</span> config<span class="token punctuation">)</span> 
    <span class="token keyword">if</span> err <span class="token operator">!=</span> <span class="token boolean">nil</span> <span class="token punctuation">{</span>
        <span class="token function">panic</span><span class="token punctuation">(</span>err<span class="token punctuation">)</span>
    <span class="token punctuation">}</span>
    <span class="token comment">//发布一条消息</span>
    p<span class="token punctuation">.</span><span class="token function">Publish</span><span class="token punctuation">(</span><span class="token string">"test"</span><span class="token punctuation">,</span> <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token function">byte</span><span class="token punctuation">(</span>time<span class="token punctuation">.</span><span class="token function">Now</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">String</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">)</span>
</code></pre>
<p>代码中有两个含义非常重要：</p>
<ol>
<li>一个topic的发布者只对应一个具体的NSQD，但可以多个发布者同时向一个NSQD发送消息，他们是N:1的关系。</li>
<li>NSQD与topic是1：N的关系。</li>
</ol>
<blockquote>
<p>代码中的<strong>config</strong>是连接配置，作为发布者，不用刻意修改，在集群中足够使用。</p>
</blockquote>
<ul>
<li>
<p>消费者<br>
消费者的理解要复杂一些，集群中最容易碰到无法接受到多节点消息的问题。结合官方多个文档及踩过的坑，需要注意：</p>
<ol>
<li>consumer要接收消息，是要连接到具体的nsqd服务的。通常我们能通过封装好的方法，基于lookupd服务来获取所有的nsqd服务地址并连接。</li>
<li>一个消费者订阅的topic分布在哪些nsqd服务中，则会直接连接。**nsqd之间是绝对不会互传topic的具体数据的。**下图描绘了consumer与nsqd的关系：<br>
<img src="http://static.zybuluo.com/alex023/ty53gwofr2o5qv84nnw5ehlh/consumer.png" alt="consumer.png-76.7kB"></li>
<li>当多个nsqd服务都有相同的topic的时候，consumer要修改默认设置<strong>config.MaxInFlight</strong>才能连接。</li>
<li>consumer与topic没有直接联系，而是通过具体的channel接受数据。如果<strong>consumer退出，channel不会自动删除</strong>。 如果不再需要，需要通过http端口删除channel，否则很可能会导致磁盘空间不足。</li>
</ol>
</li>
</ul>
<p>只要注意这几点，就很容易写出基本符合业务的代码：</p>
<pre class=" language-go"><code class="prism  language-go">    config<span class="token operator">:=</span>nsq<span class="token punctuation">.</span><span class="token function">NewConfig</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
    <span class="token comment">//最大允许向两台NSQD服务器接受消息，默认是1，要特别注意</span>
    config<span class="token punctuation">.</span>MaxInFlight<span class="token operator">=</span><span class="token number">2</span>
    c1<span class="token punctuation">,</span> err1 <span class="token operator">:=</span> nsq<span class="token punctuation">.</span><span class="token function">NewConsumer</span><span class="token punctuation">(</span><span class="token string">"test"</span><span class="token punctuation">,</span> <span class="token string">"test-channel1"</span><span class="token punctuation">,</span> nsq<span class="token punctuation">.</span><span class="token function">NewConfig</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token comment">// 新建一个消费者</span>
    <span class="token keyword">if</span> err1 <span class="token operator">!=</span> <span class="token boolean">nil</span> <span class="token punctuation">{</span>
        <span class="token function">panic</span><span class="token punctuation">(</span>err1<span class="token punctuation">)</span>
    <span class="token punctuation">}</span>
    <span class="token comment">//对消息进行处理的具体方法</span>
    receive<span class="token operator">:=</span><span class="token keyword">func</span><span class="token punctuation">(</span>msg <span class="token operator">*</span>nsq<span class="token punctuation">.</span>Message<span class="token punctuation">)</span><span class="token builtin">error</span><span class="token punctuation">{</span>
        fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token function">string</span><span class="token punctuation">(</span>msg<span class="token punctuation">.</span>Body<span class="token punctuation">)</span>
        <span class="token keyword">return</span> <span class="token boolean">nil</span>
    <span class="token punctuation">}</span>
    <span class="token comment">// 添加消息处理的具体实现</span>
    c1<span class="token punctuation">.</span><span class="token function">AddHandler</span><span class="token punctuation">(</span>nsq<span class="token punctuation">.</span><span class="token function">HandlerFunc</span><span class="token punctuation">(</span>receive<span class="token punctuation">)</span><span class="token punctuation">)</span> 
    <span class="token comment">//将消费者连接到具体的NSQD</span>
    <span class="token comment">//if err := c1.ConnectToNSQD("127.0.0.1:4150"); err != nil { </span>
    <span class="token comment">//  panic(err)</span>
    <span class="token comment">//}</span>
    <span class="token comment">//或者，如果启动了Lookupd服务，可通过nsqlookupd再分发给具体的nsqd</span>
    <span class="token keyword">if</span> err <span class="token operator">:=</span> c1<span class="token punctuation">.</span><span class="token function">ConnectToNSQLookupd</span><span class="token punctuation">(</span><span class="token string">"127.0.0.1:4161"</span><span class="token punctuation">)</span><span class="token punctuation">;</span> err <span class="token operator">!=</span> <span class="token boolean">nil</span> <span class="token punctuation">{</span>
        <span class="token function">panic</span><span class="token punctuation">(</span>err<span class="token punctuation">)</span>
    <span class="token punctuation">}</span>
</code></pre>
<blockquote>
<p>当消费者解析数据抛出错误后，channel会requene，但间隔时间将会越来越长。</p>
</blockquote>
<h3 id="常见问题">4.常见问题</h3>
<ul>
<li>nsqd启动时，端口和数据存放要不同</li>
<li>消息发送必须指定具体的某个nsqd；而消费则可以通过lookupd获取再重定向</li>
<li>消费者接受数据时，要设置 config.MaxInFlight</li>
<li>channel在消费者退出后并不会删除，需要特别注意。如果紧紧是想利用nsq作为消息广播，不考虑离线数据保存，不妨考虑nats。</li>
<li>channel的名字，有很多限制，基本ASSCI字符+数字，以及点号”.”,下划线”_”。中文（其他非英语文字应该也不行）、以及空格、冒号”:”、横线”-“等都不得出现。</li>
</ul>
<blockquote>
<p>reference:<br>
<a href="http://www.sohu.com/a/166703870_657921">http://www.sohu.com/a/166703870_657921</a><br>
<a href="https://blog.csdn.net/kk936321732/article/details/79099796">https://blog.csdn.net/kk936321732/article/details/79099796</a><br>
<a href="https://blog.csdn.net/qq_26981997/article/details/56673522">https://blog.csdn.net/qq_26981997/article/details/56673522</a></p>
</blockquote>

