---


---

<h2 id="背景">背景</h2>
<p>业务上量以后，对程序进行 profiling 性能诊断对很多后端程序员来说就是家常便饭。一个趁手的工具往往能让这个事情做起来事半功倍。</p>
<p>在这方面，go 有着天然的优势：继承  <a href="https://github.com/gperftools/gperftools">Google’s pprof C++ profiler</a>  的衣钵，从出生就有  <code>go tool pprof</code>  工具。并且，标准库里面提供  <a href="https://golang.org/pkg/runtime/pprof/">runtime/pprof</a>  和  <a href="https://golang.org/pkg/net/http/pprof/">net/http/pprof</a>  两个package, 使得 profiling 可编程化。</p>
<p>在非容器环境下，我们的研发同学喜欢使用  <a href="https://golang.org/pkg/net/http/pprof/">net/http/pprof</a>  来提供http接口供  <code>go tool pprof</code>  工具进行 profiling:</p>
<pre><code>import _ "net/http/pprof"

func main(){
    ...
    go func() {
        log.Println(http.ListenAndServe("localhost:6060", nil))
    }()
    ...
}

</code></pre>
<p>获取 CPU profile 数据：</p>
<pre><code>go tool pprof http://localhost:6060/debug/pprof/profile

</code></pre>
<p>但是，当架构逐步演进为微服务架构并使用k8s等容器化技术进行部署以后，这种这种方式面临的问题也越来越多：</p>
<ol>
<li>我们生产环境使用k8s进行容器编排和部署。service类型是 NodePort. 因此研发同学无法直接对某个 service 的特定 pod 进行 profiling. 之前的解决方式是：
<ol>
<li>如果要诊断的问题是这个service普遍存在的问题，则直接进行 profiling。</li>
<li>如果要诊断的问题只出现在这个service的某个特定的pod上，则由运维同学定位到该pod所处的宿主机后登录到该容器中进行profiling。耗时耗力，效率低。</li>
</ol>
</li>
<li>架构微服务化以后，服务数量呈量级增加。以前那种出现问题再去诊断服务现场的方式越来越难满足频率和数量越来越多的profiling需求（很多情况下，我们才做好profiling的准备，问题可能已经过去了……）。我们迫切的需要一种能够在程序出问题时，自动对程序进行profiling的方案，最大可能获取程序现场数据。</li>
<li>同时，我们希望这种自动profiling机制对程序性能影响尽可能小，并且可以与现有告警系统集成，直接将诊断结果通知到程序的owner.</li>
</ol>
<h2 id="方案设计与实现">方案设计与实现</h2>
<p><img src="https://ws1.sinaimg.cn/large/44cd29dagy1fra3iqxytej20mt0cjmxz.jpg" alt=""></p>
<ul>
<li>
<p>我们使用  <a href="https://github.com/kubernetes/heapster">heapster</a>  对k8s的容器集群进行监控。并将监控到的时序数据写入influxDB进行持久化。</p>
</li>
<li>
<p><code>gopprof</code>  是我们容器环境下对其他 go 服务进行性能诊断的核心服务：</p>
<ul>
<li>通过对influxDB中的监控数据分析，对于异常的pod自动进行 profiling. 当前设置的策略是如果该pod在两个1分钟分析周期内，资源使用率都超过设定的阈值0.8，则触发profiling。</li>
<li>将  <code>gopprof</code>  作为一个服务部署在k8s集群中主要是使其可以通过内网IP直接访问pod的 http profile接口，已实现对特定pod的profiling:</li>
</ul>
<pre><code>go tool pprof http://POD_LAN_IP:NodePort/debug/pprof/profile

</code></pre>
<ul>
<li><code>gopprof</code>  完成profiling后，会自动生成 profile svg 调用关系图，并将profile 数据和调用关系图上传云存储，并向服务的owner推送诊断结果通知：</li>
</ul>
<p><img src="https://ws1.sinaimg.cn/large/44cd29dagy1fra3yoe1c3j20wo0og78j.jpg" alt=""></p>
<ul>
<li>由于  <code>gopprof</code>  依赖工具  <code>go tool pprof</code>  和  <code>graphivz</code>, 因此<code>gopprof</code>的基础镜像需要预装这两个工具。参考<code>Dockerfile</code>：</li>
</ul>
<pre><code># base image contains golang env and graphivz

FROM ubuntu:16.04

MAINTAINER Daniel liudan@codoon.com

RUN apt-get update
RUN apt-get install wget -y
RUN wget -O go.tar.gz https://dl.google.com/go/go1.9.2.linux-amd64.tar.gz &amp;&amp; \
    tar -C /usr/local -xzf go.tar.gz &amp;&amp; \
    rm go.tar.gz

ENV PATH=$PATH:/usr/local/go/bin

RUN go version

RUN apt-get install graphviz -y

</code></pre>
<ul>
<li><code>gopprof</code>  向研发同学提供了对特定pod以及特定一组pod进行手动profiling的的接口。在解放运维同学生产力的同时，也让研发同学在出现难以复现的问题时，能够有更大可能性获取到程序现场。</li>
<li>在高可用方面，当前只支持部署一个  <code>gopprof</code>  pod, 服务可用性依赖于k8s的的auto restart. 后期如果有这方面的需求，可能会修改为依赖于etcd支持多个<code>gopprof</code>  pod部署。</li>
</ul>
</li>
</ul>
<h2 id="小结">小结</h2>
<p><code>gopprof</code>  服务已经在我们内部落地试运行了一段时间，整个上达到了我们的设计预期，并辅助我们发现和解决了一些之前没有意识到的性能问题。由于有一些内部代码依赖，暂时还无法开源出来。但是整个方案所依赖的组件都是通用的，因此你也可以很容易的实现这个方案。如果你对我们实现中的一些细节感兴趣，欢迎评论和留言。</p>
<blockquote>
<p>reference: <a href="https://liudanking.com/arch/%E5%AE%B9%E5%99%A8%E7%8E%AF%E5%A2%83%E4%B8%8B-go-%E6%9C%8D%E5%8A%A1%E6%80%A7%E8%83%BD%E8%AF%8A%E6%96%AD%E6%96%B9%E6%A1%88%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0/">addr:</a></p>
</blockquote>

