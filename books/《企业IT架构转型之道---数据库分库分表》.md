---


---

<h2 id="数据拆分----数据库分库分表">数据拆分----数据库分库分表</h2>
<h3 id="一、拆分背景">一、<strong>拆分背景</strong></h3>
<ol>
<li>单机数据库承载已达上限</li>
<li>先读写分离，一定程度上缓解了读写能力(写入影响了查询了效率)，但写数据库的压力无法扩展</li>
<li>单表记录条数&gt;200w，需要读写分离，冷热数据分开—&gt;分库分表</li>
<li>慢查询(索引失效、表设计不合理，多表join、sql优化)</li>
<li>tps高，但tps不高，需要读写分离，tps 高的话，需要分库分表</li>
</ol>
<h3 id="二、解决方案">二、<strong>解决方案</strong></h3>
<p>即 <strong>分库分表</strong>：<br>
一般作法按照Id哈希取模的方式拆分，还有结合数据结构和业务场景，有时候按照年份拆分。<br>
分库：按照业务维度<strong>垂直拆分</strong>。例如 用户userDB、支付payDB、订单orderB、商品commodity等<br>
分表有两种：</p>
<ul>
<li>
<p>水平拆分：表的行数超过1000万行时，就会变慢，把一张的表的数据拆成多张表来存放。例如 user1 ,user2,user3。<br>
它是将同一个表中的记录拆分到多个结构相同的表中。<br>
当一个表的数据不断增多时，拆分 是必然的选择，它可以将数据分布到集群的不同节点上，从而缓存单个数据库的压力。<br>
<a href="https://github.com/CyC2018/Interview-Notebook/blob/master/pics/63c2909f-0c5f-496f-9fe5-ee9176b31aba.jpg"><img src="https://github.com/CyC2018/Interview-Notebook/raw/master/pics/63c2909f-0c5f-496f-9fe5-ee9176b31aba.jpg" alt=""></a></p>
</li>
<li>
<p>垂直拆分：将一张表按列切分成多个表，通常是按照列的关系密集程度进行切分。也可以利用垂直切分将经常被使用的列和不经常被使用的列切分到不同的表中。</p>
</li>
</ul>
<p><img src="https://github.com/CyC2018/Interview-Notebook/raw/master/pics/e130e5b8-b19a-4f1e-b860-223040525cf6.jpg" alt=""></p>
<p>** 水平拆分** 小技巧</p>
<ol>
<li>
<p>用ID取模的方法把数据分散到四张表内<code>Id%4+1 = [1,2,3,4]</code><br>
这里是个小哈希，然后查询,更新,删除也是通过取模的方法来查询</p>
<pre><code> ```go
 param[“id”] = 17,
 17%4 + 1 = 2,  
 tableName := 'users'.'2'
 Select * from users2 where id = 17;
 ```

 在insert时还需要一张临时表`uid_temp`来提供自增的ID,该表的唯一用处就是提供自增的ID;

 ```
 insert into uid_temp values(null);
 ```

 得到自增的ID后,又通过取模法进行分表插入;  
 **注意,进行水平拆分后的表,字段的列和类型和原表应该是相同的,但是要记得去掉auto_increment自增长**
</code></pre>
</li>
<li>
<p>有时候结合产品业务逻辑，从UI 做约束，例如先选择年份再查询</p>
</li>
<li>
<p>在做分析或者统计时，由于是自己人的需求,多点等待其实是没关系的,并且并发很低,这个时候可以用union把所有表都组合成一张视图来进行查询,然后再进行查询;</p>
<pre><code> ```
 Create view users as select from users1 union select from users2 union.........
 ```
</code></pre>
</li>
</ol>
<h3 id="三、注意事项">三、注意事项</h3>
<ol>
<li>数据尽可能平分</li>
<li>减少事务的边界，两种查询方式：</li>
</ol>
<ul>
<li>查询时带着分库分表键</li>
<li>没有带着分库分表键，这个时候需要全表扫描</li>
</ul>
<h2 id="下一章：异步化与缓存">下一章：异步化与缓存</h2>
<blockquote>
<p>reference:<br>
《企业IT架构转型之道》<br>
<a href="https://blog.csdn.net/zbuger/article/details/51177601">https://blog.csdn.net/zbuger/article/details/51177601</a><br>
<a href="http://breezylee.iteye.com/blog/2348583">http://breezylee.iteye.com/blog/2348583</a><br>
<a href="https://github.com/CyC2018/Interview-Notebook/blob/master/notes/MySQL.md#%E4%BA%94%E5%88%87%E5%88%86">https://github.com/CyC2018/Interview-Notebook/blob/master/notes/MySQL.md#五切分</a></p>
</blockquote>

