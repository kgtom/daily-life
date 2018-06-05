---


---

<p><strong>使用EXPLAIN</strong></p>
<p>PostgreSQL为每个收到的查询设计一个查询规划。选择正确的匹配查询结构和数据属性的规划对执行效率是至关重要要的，所以系统包含一个复杂的规划器来试图选择好的规划。你可以使用EXPLAIN命令查看查询规划器创建的任何查询。阅读查询规划是一门艺术，需要掌握一定的经验，本节试图涵盖一些基础知识。</p>
<p>以下的例子来自PostgreSQL 9.3开发版。</p>
<p><strong>EXPLAIN基础</strong></p>
<p>查询规划是以规划为节点的树形结构。树的最底节点是扫描节点：他返回表中的原数据行。</p>
<p>不同的表有不同的扫描节点类型：顺序扫描，索引扫描和位图索引扫描。</p>
<p>也有非表列源，如VALUES子句并设置FROM返回，他们有自己的扫描类型。</p>
<p>如果查询需要关联，聚合，排序或其他操作，会在扫描节点之上增加节点执行这些操作。通常有不只一种可能的方式做这些操作，所以可能出现不同的节点类型。</p>
<p>EXPLAIN的输出是每个树节点显示一行，内容是基本节点类型和执行节点的消耗评估。可能会楚翔其他行，从汇总行节点缩进显示节点的其他属性。第一行（最上节点的汇总行）是评估执行计划的总消耗，这个值越小越好。</p>
<p>下面是一个简单的例子：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT * FROM tenk1;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Seq Scan on tenk1 (cost=0.00…458.00 rows=10000 width=244)</p>
</li>
</ol>
<p>因为这个查询没有WHERE子句，所以必须扫描表中的所有行，所以规划器选择使用简单的顺序扫描规划。括号中的数字从左到右依次是：</p>
<ul>
<li>评估开始消耗。这是可以开始输出前的时间，比如排序节点的排序的时间。</li>
<li>评估总消耗。假设查询从执行到结束的时间。有时父节点可能停止这个过程，比如LIMIT子句。</li>
<li>评估查询节点的输出行数，假设该节点执行结束。</li>
<li>评估查询节点的输出行的平均字节数。</li>
</ul>
<p>这个消耗的计算依赖于规划器的设置参数，这里的例子都是在默认参数下运行。</p>
<p>需要知道的是：上级节点的消耗包括其子节点的消耗。这个消耗值只反映规划器关心的内容，一般这个消耗不包括将数据传输到客户端的时间。</p>
<p>评估的行数不是执行和扫描查询节点的数量，而是节点返回的数量。它通常会少于扫描数量，因为有WHERE条件会过滤掉一些数据。理想情况顶级行数评估近似于实际返回的数量</p>
<p>回到刚才的例子，表tenk1有10000条数据分布在358个磁盘页，评估时间是（磁盘页<em>seq_page_cost）+（扫描行</em>cpu_tuple_cost）。默认seq_page_cost是1.0，cpu_tuple_cost是0.01，所以评估值是(358 * 1.0) + (10000 * 0.01) = 458</p>
<p>现在我们将查询加上WHERE子句：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT * FROM tenk1 WHERE unique1 &lt; 7000;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Seq Scan on tenk1 (cost=0.00…483.00 rows=7001 width=244)</p>
</li>
<li>
<p>Filter: (unique1 &lt; 7000)</p>
</li>
</ol>
<p>查询节点增加了“filter”条件。这意味着查询节点为扫描的每一行数据增加条件检查，只输入符合条件数据。评估的输出记录数因为where子句变少了，但是扫描的数据还是10000条，所以消耗没有减少，反而增加了一点cup的计算时间。</p>
<p>这个查询实际输出的记录数是7000，但是评估是个近似值，多次运行可能略有差别，这中情况可以通过ANALYZE命令改善。</p>
<p>现在再修改一下条件</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT * FROM tenk1 WHERE unique1 &lt; 100;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Bitmap Heap Scan on tenk1 (cost=5.07…229.20 rows=101 width=244)</p>
</li>
<li>
<p>Recheck Cond: (unique1 &lt; 100)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…5.04 rows=101 width=0)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 100)</p>
</li>
</ol>
<p>查询规划器决定使用两步规划：首先子查询节点查看索引找到符合条件的记录索引，然后外层查询节点将这些记录从表中提取出来。分别提取数据的成本要高于顺序读取，但因为不需要读取所有磁盘页，所以总消耗比较小。（其中Bitmap是系统排序的一种机制）</p>
<p>现在，增加另一个查询条件：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT * FROM tenk1 WHERE unique1 &lt; 100 AND stringu1 = ‘xxx’;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Bitmap Heap Scan on tenk1 (cost=5.04…229.43 rows=1 width=244)</p>
</li>
<li>
<p>Recheck Cond: (unique1 &lt; 100)</p>
</li>
<li>
<p>Filter: (stringu1 = ‘xxx’::name)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…5.04 rows=101 width=0)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 100)</p>
</li>
</ol>
<p>增加的条件stringu1='xxx’减少了输出记录数的评估，但没有减少时间消耗，应为系统还是要查询相同数量的记录。请注意stringu1不是索引条件。</p>
<p>如果在不同的字段上有独立的索引，规划器可能选择使用AND或者OR组合索引：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT * FROM tenk1 WHERE unique1 &lt; 100 AND unique2 &gt; 9000;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Bitmap Heap Scan on tenk1 (cost=25.08…60.21 rows=10 width=244)</p>
</li>
<li>
<p>Recheck Cond: ((unique1 &lt; 100) AND (unique2 &gt; 9000))</p>
</li>
<li>
<p>-&gt; BitmapAnd (cost=25.08…25.08 rows=10 width=0)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…5.04 rows=101 width=0)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 100)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique2 (cost=0.00…19.78 rows=999 width=0)</p>
</li>
<li>
<p>Index Cond: (unique2 &gt; 9000)</p>
</li>
</ol>
<p>这个查询条件的两个字段都有索引，索引不需要filre。</p>
<p>下面我们来看看LIMIT的影响：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT * FROM tenk1 WHERE unique1 &lt; 100 AND unique2 &gt; 9000 LIMIT 2;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Limit (cost=0.29…14.48 rows=2 width=244)</p>
</li>
<li>
<p>-&gt; Index Scan using tenk1_unique2 on tenk1 (cost=0.29…71.27 rows=10 width=244)</p>
</li>
<li>
<p>Index Cond: (unique2 &gt; 9000)</p>
</li>
<li>
<p>Filter: (unique1 &lt; 100)</p>
</li>
</ol>
<p>这条查询的where条件和上面的一样，只是增加了LIMIT，所以不是所有数据都需要返回，规划器改变了规划。在索引扫描节点总消耗和返回记录数是运行玩查询之后的数值，但Limit节点预期时间消耗是15，所以总时间消耗是15.增加LIMIT会使启动时间小幅增加（0.25-&gt;0.29）。</p>
<p>来看一下通过索引字段的表连接：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT *</p>
</li>
<li>
<p>FROM tenk1 t1, tenk2 t2</p>
</li>
<li>
<p>WHERE t1.unique1 &lt; 10 AND t1.unique2 = t2.unique2;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Nested Loop (cost=4.65…118.62 rows=10 width=488)</p>
</li>
<li>
<p>-&gt; Bitmap Heap Scan on tenk1 t1 (cost=4.36…39.47 rows=10 width=244)</p>
</li>
<li>
<p>Recheck Cond: (unique1 &lt; 10)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…4.36 rows=10 width=0)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 10)</p>
</li>
<li>
<p>-&gt; Index Scan using tenk2_unique2 on tenk2 t2 (cost=0.29…7.91 rows=1 width=244)</p>
</li>
<li>
<p>Index Cond: (unique2 = t1.unique2)</p>
</li>
</ol>
<p>这个规划中有一个内连接的节点，它有两个子节点。节点摘要行的缩进反映了规划树的结构。最外层是一个连接节点，子节点是一个Bitmap扫描。外部节点位图扫描的消耗和记录数如同我们使用SELECT…WHERE unique1 &lt; 10，因为这时t1.unique2 = t2.unique2还不相关。接下来为每一个从外部节点得到的记录运行内部查询节点。这里外部节点得到的数据的t1.unique2值是可用的，所以我们得到的计划和SELECT…WHEREt2.unique2=constant的情况类似。（考虑到缓存的因素评估的消耗可能要小一些）</p>
<p>外部节点的消耗加上循环内部节点的消耗（39.47+10*7.91）再加一点CPU时间就得到规划的总消耗。</p>
<p>再看一个例子：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT *</p>
</li>
<li>
<p>FROM tenk1 t1, tenk2 t2</p>
</li>
<li>
<p>WHERE t1.unique1 &lt; 10 AND t2.unique2 &lt; 10 AND t1.hundred &lt; t2.hundred;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Nested Loop (cost=4.65…49.46 rows=33 width=488)</p>
</li>
<li>
<p>Join Filter: (t1.hundred &lt; t2.hundred)</p>
</li>
<li>
<p>-&gt; Bitmap Heap Scan on tenk1 t1 (cost=4.36…39.47 rows=10 width=244)</p>
</li>
<li>
<p>Recheck Cond: (unique1 &lt; 10)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…4.36 rows=10 width=0)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 10)</p>
</li>
<li>
<p>-&gt; Materialize (cost=0.29…8.51 rows=10 width=244)</p>
</li>
<li>
<p>-&gt; Index Scan using tenk2_unique2 on tenk2 t2 (cost=0.29…8.46 rows=10 width=244)</p>
</li>
<li>
<p>Index Cond: (unique2 &lt; 10)</p>
</li>
</ol>
<p>条件t1.hundred&lt;t2.hundred不在tenk2_unique2索引中，所以这个条件出现在连接节点中。这将减少连接节点的评估输出记录数，但不会改变子节点的扫描数。</p>
<p>注意这次规划器选择使用Meaterialize节点，将条件加入内部节点，这以为着内部节点的索引扫描只做一次，即使嵌套循环需要读取这些数据10次，Meterialize节点将数据保存在内存中，每次循环都从内存中读取数据。</p>
<p>如果我们稍微改变一下查询，会看到完全不同的规划：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT *</p>
</li>
<li>
<p>FROM tenk1 t1, tenk2 t2</p>
</li>
<li>
<p>WHERE t1.unique1 &lt; 100 AND t1.unique2 = t2.unique2;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Hash Join (cost=230.47…713.98 rows=101 width=488)</p>
</li>
<li>
<p>Hash Cond: (t2.unique2 = t1.unique2)</p>
</li>
<li>
<p>-&gt; Seq Scan on tenk2 t2 (cost=0.00…445.00 rows=10000 width=244)</p>
</li>
<li>
<p>-&gt; Hash (cost=229.20…229.20 rows=101 width=244)</p>
</li>
<li>
<p>-&gt; Bitmap Heap Scan on tenk1 t1 (cost=5.07…229.20 rows=101 width=244)</p>
</li>
<li>
<p>Recheck Cond: (unique1 &lt; 100)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…5.04 rows=101 width=0)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 100)</p>
</li>
</ol>
<p>这里规划器选择使用hash join，将一个表的数据存入内存中的哈希表，然后扫描另一个表并和哈希表中的每一条数据进行匹配。</p>
<p>注意缩进反应的规划结构。在tenk1表上的bitmap扫描结果作为Hash节点的输入建立哈希表。然后Hash Join节点读取外层子节点的数据，再循环检索哈希表的数据。</p>
<p>另一个可能的连接类型是merge join：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN SELECT *</p>
</li>
<li>
<p>FROM tenk1 t1, onek t2</p>
</li>
<li>
<p>WHERE t1.unique1 &lt; 100 AND t1.unique2 = t2.unique2;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Merge Join (cost=198.11…268.19 rows=10 width=488)</p>
</li>
<li>
<p>Merge Cond: (t1.unique2 = t2.unique2)</p>
</li>
<li>
<p>-&gt; Index Scan using tenk1_unique2 on tenk1 t1 (cost=0.29…656.28 rows=101 width=244)</p>
</li>
<li>
<p>Filter: (unique1 &lt; 100)</p>
</li>
<li>
<p>-&gt; Sort (cost=197.83…200.33 rows=1000 width=244)</p>
</li>
<li>
<p>Sort Key: t2.unique2</p>
</li>
<li>
<p>-&gt; Seq Scan on onek t2 (cost=0.00…148.00 rows=1000 width=244)</p>
</li>
</ol>
<p>Merge Join需要已经排序的输入数据。在这个规划中按正确顺序索引扫描tenk1的数据，但是对onek表执行排序和顺序扫描，因为需要在这个表中查询多条数据。因为索引扫描需要访问不连续的磁盘，所以索引扫描多条数据时会频繁使用排序顺序扫描（Sequential-scan-and-sort）。</p>
<p>有一种方法可以看到不同的规划，就是强制规划器忽略任何策略。例如，如果我们不相信排序顺序扫描（sequential-scan-and-sort）是最好的办法，我们可以尝试这样的做法：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>SET enable_sort = off;</p>
</li>
<li>
<p>EXPLAIN SELECT *</p>
</li>
<li>
<p>FROM tenk1 t1, onek t2</p>
</li>
<li>
<p>WHERE t1.unique1 &lt; 100 AND t1.unique2 = t2.unique2;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Merge Join (cost=0.56…292.65 rows=10 width=488)</p>
</li>
<li>
<p>Merge Cond: (t1.unique2 = t2.unique2)</p>
</li>
<li>
<p>-&gt; Index Scan using tenk1_unique2 on tenk1 t1 (cost=0.29…656.28 rows=101 width=244)</p>
</li>
<li>
<p>Filter: (unique1 &lt; 100)</p>
</li>
<li>
<p>-&gt; Index Scan using onek_unique2 on onek t2 (cost=0.28…224.79 rows=1000 width=244)</p>
</li>
</ol>
<p>显示测结果表明，规划器认为索引扫描比排序顺序扫描消耗高12%。当然下一个问题就是规划器的评估为什么是正确的。我们可以通过EXPLAIN ANALYZE进行考察。</p>
<p><strong>EXPLAIN ANALYZE</strong></p>
<p>通过EXPLAIN ANALYZE可以检查规划器评估的准确性。使用ANALYZE选项，EXPLAIN实际运行查询，显示真实的返回记录数和运行每个规划节点的时间，例如我们可以得到下面的结果：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN ANALYZE SELECT *</p>
</li>
<li>
<p>FROM tenk1 t1, tenk2 t2</p>
</li>
<li>
<p>WHERE t1.unique1 &lt; 10 AND t1.unique2 = t2.unique2;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Nested Loop (cost=4.65…118.62 rows=10 width=488) (actual time=0.128…0.377 rows=10 loops=1)</p>
</li>
<li>
<p>-&gt; Bitmap Heap Scan on tenk1 t1 (cost=4.36…39.47 rows=10 width=244) (actual time=0.057…0.121 rows=10 loops=1)</p>
</li>
<li>
<p>Recheck Cond: (unique1 &lt; 10)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…4.36 rows=10 width=0) (actual time=0.024…0.024 rows=10 loops=1)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 10)</p>
</li>
<li>
<p>-&gt; Index Scan using tenk2_unique2 on tenk2 t2 (cost=0.29…7.91 rows=1 width=244) (actual time=0.021…0.022 rows=1 loops=10)</p>
</li>
<li>
<p>Index Cond: (unique2 = t1.unique2)</p>
</li>
<li>
<p>Total runtime: 0.501 ms</p>
</li>
</ol>
<p>注意，实际时间(actual time)的值是已毫秒为单位的实际时间，cost是评估的消耗，是个虚拟单位时间，所以他们看起来不匹配。</p>
<p>通常最重要的是看评估的记录数是否和实际得到的记录数接近。在这个例子里评估数完全和实际一样，但这种情况很少出现。</p>
<p>某些查询规划可能执行多次子规划。比如之前提过的内循环规划（nested-loop），内部索引扫描的次数是外部数据的数量。在这种情况下，报告显示循环执行的总次数、平均实际执行时间和数据条数。这样做是为了和评估值表示方式一至。由循环次数和平均值相乘得到总消耗时间。</p>
<p>某些情况EXPLAIN ANALYZE会显示额外的信息，比如sort和hash节点的时候：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN ANALYZE SELECT *</p>
</li>
<li>
<p>FROM tenk1 t1, tenk2 t2</p>
</li>
<li>
<p>WHERE t1.unique1 &lt; 100 AND t1.unique2 = t2.unique2 ORDER BY t1.fivethous;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Sort (cost=717.34…717.59 rows=101 width=488) (actual time=7.761…7.774 rows=100 loops=1)</p>
</li>
<li>
<p>Sort Key: t1.fivethous</p>
</li>
<li>
<p>Sort Method: quicksort Memory: 77kB</p>
</li>
<li>
<p>-&gt; Hash Join (cost=230.47…713.98 rows=101 width=488) (actual time=0.711…7.427 rows=100 loops=1)</p>
</li>
<li>
<p>Hash Cond: (t2.unique2 = t1.unique2)</p>
</li>
<li>
<p>-&gt; Seq Scan on tenk2 t2 (cost=0.00…445.00 rows=10000 width=244) (actual time=0.007…2.583 rows=10000 loops=1)</p>
</li>
<li>
<p>-&gt; Hash (cost=229.20…229.20 rows=101 width=244) (actual time=0.659…0.659 rows=100 loops=1)</p>
</li>
<li>
<p>Buckets: 1024 Batches: 1 Memory Usage: 28kB</p>
</li>
<li>
<p>-&gt; Bitmap Heap Scan on tenk1 t1 (cost=5.07…229.20 rows=101 width=244) (actual time=0.080…0.526 rows=100 loops=1)</p>
</li>
<li>
<p>Recheck Cond: (unique1 &lt; 100)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…5.04 rows=101 width=0) (actual time=0.049…0.049 rows=100 loops=1)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 100)</p>
</li>
<li>
<p>Total runtime: 8.008 ms</p>
</li>
</ol>
<p>排序节点（Sort）显示排序类型（一般是在内存还是在磁盘）和使用多少内存。哈希节点（Hash）显示哈希桶和批数以及使用内存的峰值。</p>
<p>另一种额外信息是过滤条件过滤掉的记录数：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN ANALYZE SELECT * FROM tenk1 WHERE ten &lt; 7;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Seq Scan on tenk1 (cost=0.00…483.00 rows=7000 width=244) (actual time=0.016…5.107 rows=7000 loops=1)</p>
</li>
<li>
<p>Filter: (ten &lt; 7)</p>
</li>
<li>
<p>Rows Removed by Filter: 3000</p>
</li>
<li>
<p>Total runtime: 5.905 ms</p>
</li>
</ol>
<p>这个值在join节点上尤其有价值。"Rows Removed"只有在过滤条件过滤掉数据时才显示。</p>
<p>类似条件过滤的情况也会在"lossy"索引扫描时发生，比如这样一个查询，一个多边形含有的特定的点：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN ANALYZE SELECT * FROM polygon_tbl WHERE f1 @&gt; polygon ‘(0.5,2.0)’;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Seq Scan on polygon_tbl (cost=0.00…1.05 rows=1 width=32) (actual time=0.044…0.044 rows=0 loops=1)</p>
</li>
<li>
<p>Filter: (f1 @&gt; ‘((0.5,2))’::polygon)</p>
</li>
<li>
<p>Rows Removed by Filter: 4</p>
</li>
<li>
<p>Total runtime: 0.083 ms</p>
</li>
</ol>
<p>规划器认为（正确的）这样的表太小以至于不需要索引扫描，所以采用顺序扫描所有行经行条件检查。</p>
<p>但是，如果我们强制使用索引扫描，将会看到：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>SET enable_seqscan TO off;</p>
</li>
<li>
<p>EXPLAIN ANALYZE SELECT * FROM polygon_tbl WHERE f1 @&gt; polygon ‘(0.5,2.0)’;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Index Scan using gpolygonind on polygon_tbl (cost=0.13…8.15 rows=1 width=32) (actual time=0.062…0.062 rows=0 loops=1)</p>
</li>
<li>
<p>Index Cond: (f1 @&gt; ‘((0.5,2))’::polygon)</p>
</li>
<li>
<p>Rows Removed by Index Recheck: 1</p>
</li>
<li>
<p>Total runtime: 0.144 ms</p>
</li>
</ol>
<p>这里我们可以看到索引返回一条候选数据，但被过滤条件拒绝。这是因为GiST索引在多边形包含检测上是松散的"lossy"：它实际返回哪些和多边形交叠的数据，然后我们还需要针对这些数据做包含检测。</p>
<p>EXPLAIN还有BUFFERS选项可以和ANALYZE一起使用，来得到更多的运行时间分析：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>EXPLAIN (ANALYZE, BUFFERS) SELECT * FROM tenk1 WHERE unique1 &lt; 100 AND unique2 &gt; 9000;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Bitmap Heap Scan on tenk1 (cost=25.08…60.21 rows=10 width=244) (actual time=0.323…0.342 rows=10 loops=1)</p>
</li>
<li>
<p>Recheck Cond: ((unique1 &lt; 100) AND (unique2 &gt; 9000))</p>
</li>
<li>
<p>Buffers: shared hit=15</p>
</li>
<li>
<p>-&gt; BitmapAnd (cost=25.08…25.08 rows=10 width=0) (actual time=0.309…0.309 rows=0 loops=1)</p>
</li>
<li>
<p>Buffers: shared hit=7</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…5.04 rows=101 width=0) (actual time=0.043…0.043 rows=100 loops=1)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 100)</p>
</li>
<li>
<p>Buffers: shared hit=2</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique2 (cost=0.00…19.78 rows=999 width=0) (actual time=0.227…0.227 rows=999 loops=1)</p>
</li>
<li>
<p>Index Cond: (unique2 &gt; 9000)</p>
</li>
<li>
<p>Buffers: shared hit=5</p>
</li>
<li>
<p>Total runtime: 0.423 ms</p>
</li>
</ol>
<p>Buffers提供的数据可以帮助确定哪些查询是I/O密集型的。</p>
<p>请注意EXPLAIN ANALYZE实际运行查询，任何实际影响都会发生。如果要分析一个修改数据的查询又不想改变你的表，你可以使用roll back命令进行回滚，比如：</p>
<p>Sql代码 <img src="http://toplchx.iteye.com/images/icon_star.png" alt="收藏代码"></p>
<ol>
<li>
<p>BEGIN;</p>
</li>
<li>
<p>EXPLAIN ANALYZE UPDATE tenk1 SET hundred = hundred + 1 WHERE unique1 &lt; 100;</p>
</li>
<li>
<p>QUERY PLAN</p>
</li>
<li>
<hr>
</li>
<li>
<p>Update on tenk1 (cost=5.07…229.46 rows=101 width=250) (actual time=14.628…14.628 rows=0 loops=1)</p>
</li>
<li>
<p>-&gt; Bitmap Heap Scan on tenk1 (cost=5.07…229.46 rows=101 width=250) (actual time=0.101…0.439 rows=100 loops=1)</p>
</li>
<li>
<p>Recheck Cond: (unique1 &lt; 100)</p>
</li>
<li>
<p>-&gt; Bitmap Index Scan on tenk1_unique1 (cost=0.00…5.04 rows=101 width=0) (actual time=0.043…0.043 rows=100 loops=1)</p>
</li>
<li>
<p>Index Cond: (unique1 &lt; 100)</p>
</li>
<li>
<p>Total runtime: 14.727 ms</p>
</li>
<li>
<p>ROLLBACK;</p>
</li>
</ol>
<p>当查询是INSERT,UPDATE或DELETE命令时，在顶级节点是实施对表的变更。在这下面的节点实行定位旧数据计算新数据的工作。所以我们看到一样的bitmap索引扫描，并返回给Update节点。值得注意的是虽然修改数据的节点可能需要相当长的运行时间（在这里它消耗了大部分的时间），规划器却没有再评估时间中添加任何消耗，这是因为更新工作对于任何查询规划都是一样的，所以并不影响规划器的决策。</p>
<p>EXPLAIN ANALYZE的"Total runtime"包括执行启动和关闭时间，以及运行被激发的任何处触发器的时间，但不包括分析、重写或规划时间。执行时间包括BEFORE触发器，但不包括AFTER触发器，因为AFTER是在查询运行结束之后才触发的。每个触发器（无论BEFORE还是AFTER）的时间也会单独显示出来。注意，延迟的触发器在事务结束前都不会被执行，所以EXPLAIN ANALYZE不会显示。</p>
<blockquote>
<p>reference:<br>
<a href="https://blog.csdn.net/wjy397/article/details/71412742">https://blog.csdn.net/wjy397/article/details/71412742</a></p>
</blockquote>

