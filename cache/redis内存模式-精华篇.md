---


---

<h2 id="本章学习知识点">本章学习知识点</h2>
<p>在了解Redis的5（字符串、哈希、列表、集合、有序集合）种对象类型的用法和特点的基础上，进一步了解Redis的内存模型，对Redis的使用有很大帮助，例如：</p>
<p>1、估算Redis内存使用量。选择合适的机器配置，可以在满足需求的情况下节约成本。</p>
<p>2、优化内存占用。了解Redis内存模型可以选择更合适的数据类型和编码，更好的利用Redis内存。</p>
<p>3、分析解决问题。当Redis出现阻塞、内存占用等问题时，尽快发现导致问题的原因，便于分析解决问题。</p>
<p>这篇文章主要介绍Redis的内存模型（以3.0为例），<strong>包括    Redis占用内存的情况及如何查询、不同的对象类型在内存中的编码方式、内存分配器(jemalloc)、简单动态字符串(SDS)、RedisObject等 ；</strong> 然后在此基础上介绍几个Redis内存模型的应用。</p>
<blockquote>
<p>Reference: <a href="https://juejin.im/entry/5ae2c177f265da0b722ad90b#t1">addr</a></p>
</blockquote>

