---


---

<p><strong>Pg 没有 MySQL 的各种坑</strong></p>
<p>MySQL 的各种 text 字段有不同的限制, 要手动区分 small text, middle text, large text… Pg 没有这个限制, text 能支持各种大小.</p>
<p>按照 SQL 标准, 做 null 判断不能用 = null, 只能用 is null</p>
<blockquote>
<p>the result of any arithmetic comparison with NULL is also NULL</p>
</blockquote>
<p>但 pg 可以设置 transform_null_equals 把 = null 翻译成 is null 避免踩坑</p>
<p>不少人应该遇到过 MySQL 里需要 utf8mb4 才能显示 emoji 的坑, Pg 就没这个坑.</p>
<p>MySQL 的事务隔离级别 repeatable read 并不能阻止常见的并发更新, 得加锁才可以, 但悲观锁会影响性能, 手动实现乐观锁又复杂. 而 Pg 的列里有隐藏的乐观锁 version 字段, 默认的 repeatable read 级别就能保证并发更新的正确性, 并且又有乐观锁的性能. 附带一个各数据库对隔离级别的行为差异比较调查: <a href="https://link.zhihu.com/?target=http%3A//www.cs.umb.edu/%7Eponeil/iso.pdf">http://www.cs.umb.edu/~poneil/iso.pdf</a></p>
<p>MySQL 不支持多个表从同一个序列中取 id, 而 Pg 可以.</p>
<p>MySQL 不支持 OVER 子句, 而 Pg 支持. OVER 子句能简单的解决 “每组取 top 5” 的这类问题.</p>
<p>几乎任何数据库的子查询 (subquery) 性能都比 MySQL 好.</p>
<p>更多的坑:<br>
<a href="https://link.zhihu.com/?target=http%3A//blog.ionelmc.ro/2014/12/28/terrible-choices-mysql/">http://blog.ionelmc.ro/2014/12/28/terrible-choices-mysql/</a></p>
<p>不少人踩完坑了, 以为换个数据库还得踩一次, 所以很抗拒, 事实上不是!!!</p>
<p><strong>Pg 不仅仅是 SQL 数据库</strong></p>
<p>它可以存储 array 和 json, 可以在 array 和 json 上建索引, 甚至还能用表达式索引. 为了实现文档数据库的功能, 设计了 jsonb 的存储结构. 有人会说为什么不用 Mongodb 的 BSON 呢? Pg 的开发团队曾经考虑过, 但是他们看到 BSON 把 [“a”, “b”, “c”] 存成 {0: “a”, 1: “b”, 2: “c”} 的时候就决定要重新做一个 jsonb 了… 现在 jsonb 的性能已经优于 BSON.</p>
<p>现在往前端偏移的开发环境里, 用 Pg + PostgREST 直接生成后端 API 是非常快速高效的办法:<br>
<a href="https://link.zhihu.com/?target=https%3A//github.com/begriffs/postgrest">begriffs/postgrest · GitHub</a><br>
postgREST 的性能非常强悍, 一个原因就是 Pg 可以直接组织返回 json 的结果.</p>
<p>它支持服务器端脚本: TCL, Python, R, Perl, Ruby, MRuby … 自带 map-reduce 了.</p>
<p>它有地理信息处理扩展 (GIS 扩展不仅限于真实世界, 游戏里的地形什么的也可以), 可以用 Pg 搭寻路服务器和地图服务器:<br>
<a href="https://link.zhihu.com/?target=http%3A//postgis.net/">PostGIS — Spatial and Geographic Objects for PostgreSQL</a></p>
<p>它自带全文搜索功能 (不用费劲再装一个 elasticsearch 咯):<br>
<a href="https://link.zhihu.com/?target=https%3A//blog.lateral.io/2015/05/full-text-search-in-milliseconds-with-postgresql/">Full text search in milliseconds with PostgreSQL</a> 不过一些语言相关的支持还不太完善, 有个 bamboo 插件用调教过的 mecab 做中文分词, 如果要求比较高, 还是自己分了词再存到 tsvector 比较好.</p>
<p>它支持 trigram 索引.<br>
trigram 索引可以帮助改进全文搜索的结果: <a href="https://link.zhihu.com/?target=http%3A//www.postgresql.org/docs/9.3/static/pgtrgm.html">PostgreSQL: Documentation: 9.3: pg_trgm</a><br>
trigram 还可以实现高效的正则搜索 (原理参考 <a href="https://link.zhihu.com/?target=https%3A//swtch.com/%7Ersc/regexp/regexp4.html">https://swtch.com/~rsc/regexp/regexp4.html</a> )</p>
<p>MySQL 处理树状回复的设计会很复杂, 而且需要写很多代码, 而 Pg 可以高效处理树结构:<br>
<a href="https://link.zhihu.com/?target=http%3A//cramer.io/2010/05/30/scaling-threaded-comments-on-django-at-disqus/">Scaling Threaded Comments on Django at Disqus</a><br>
<a href="https://link.zhihu.com/?target=http%3A//www.slideshare.net/quipo/trees-in-the-database-advanced-data-structures">http://www.slideshare.net/quipo/trees-in-the-database-advanced-data-structures</a></p>
<p>它可以高效处理图结构, 轻松实现 “朋友的朋友的朋友” 这种功能:<br>
<a href="https://link.zhihu.com/?target=http%3A//www.slideshare.net/quipo/rdbms-in-the-social-networks-age">http://www.slideshare.net/quipo/rdbms-in-the-social-networks-age</a></p>
<p>它可以把 70 种外部数据源 (包括 Mysql, Oracle, CSV, hadoop …) 当成自己数据库中的表来查询:<br>
<a href="https://link.zhihu.com/?target=https%3A//wiki.postgresql.org/wiki/FDW%3Fnocache%3D1">Foreign data wrappers</a></p>
<p><strong>心动不如行动</strong></p>
<p><a href="https://link.zhihu.com/?target=https%3A//en.wikibooks.org/wiki/Converting_MySQL_to_PostgreSQL">Converting MySQL to PostgreSQL</a></p>
<blockquote>
<p>总结：根据业务选择最合适的，而不是选择最好的，最流行的。</p>
</blockquote>
<blockquote>
<p>reference：<br>
<a href="https://www.zhihu.com/question/20010554">https://www.zhihu.com/question/20010554</a></p>
</blockquote>

