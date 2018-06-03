---


---

<h2 id="数据拆分----数据库分库分表">数据拆分----数据库分库分表</h2>
<p><strong>拆分背景</strong></p>
<ol>
<li>单机数据库承载已达上限</li>
<li>先读写分离，一定程度上缓解了读写能力，但写数据库的压力无法扩展</li>
<li>单表记录条数&gt;200w，需要读写分离，冷热数据分开—&gt;分库分表</li>
<li>慢查询(索引失效、表设计不合理，多表join、sql优化)</li>
<li>tps高，但tps不高，需要读写分离，tps 高的话，需要分库分表</li>
</ol>
<p><strong>解决方案</strong></p>
<p>1.分库分表：<br>
一般作法按照Id哈希取模的方式拆分，还有结合数据结构和业务场景，有时候按照年份拆分。<br>
库：按照业务垂直拆分，例如 用户、支付、机票、商品等<br>
表：按照</p>
<blockquote>
<p>reference:<br>
《企业IT架构转型之道》<br>
<a href="https://blog.csdn.net/zbuger/article/details/51177601">https://blog.csdn.net/zbuger/article/details/51177601</a><br>
<a href="http://breezylee.iteye.com/blog/2348583">http://breezylee.iteye.com/blog/2348583</a></p>
</blockquote>

