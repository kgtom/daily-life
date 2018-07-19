---


---

<h2 id="面向对象设计的原则solid">面向对象设计的原则(SOLID)</h2>

<table>
<thead>
<tr>
<th></th>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td>S</td>
<td>The Single Responsibility Principle</td>
<td>单一职责原则</td>
</tr>
<tr>
<td>O</td>
<td>The Open Closed Principle</td>
<td>开放封闭原则</td>
</tr>
<tr>
<td>L</td>
<td>The Liskov Substitution Principle</td>
<td>里氏替换原则</td>
</tr>
<tr>
<td>I</td>
<td>The Interface Segregation Principle</td>
<td>接口分离原则</td>
</tr>
<tr>
<td>D</td>
<td>The Dependency Inversion Principle</td>
<td>依赖倒置原则</td>
</tr>
</tbody>
</table><h3 id="s">1.S</h3>
<blockquote>
<p>概念：对象的职责应该是单一的，方法、类、接口、模块只做自己本职工作，其它工作由其它角色做，各司其职，高内聚的一种体现。</p>
</blockquote>
<blockquote>
<p>例子：程序员，可以修电脑，也可以写代码。这个时候 我们需要拆分，写代码的活给程序员，修电脑给运营哥哥。</p>
</blockquote>
<h3 id="o">2.O</h3>
<blockquote>
<p>概念：对内部修改是关闭的，对扩展是开放的。即不修改类的代码，而是通过扩展增加类的功能，<br>
例子：笔记本电源是三孔插座，墙上的插座是两孔的，这个时候需要扩展插座，使用适配器模式，新增一个插线板提供三孔插座。</p>
</blockquote>
<blockquote>
<p>常用设计模式：适配器模式、策略模式、状态模式</p>
</blockquote>
<h3 id="l">3.L</h3>
<blockquote>
<p>概念：子类可以替换父类的功能，这种继承关系会产生高耦合，多数使用组合代替继承</p>
</blockquote>
<blockquote>
<p>例子：鸟类有个fly(), 麻雀其子类，但鸵鸟不是其子类，违反L，因为鸵鸟不会飞</p>
</blockquote>
<h3 id="isp">4.ISP</h3>
<blockquote>
<p>概念：接口分离，不要胖接口，要明确小接口，各司其职。</p>
</blockquote>
<h3 id="d">5.D</h3>
<blockquote>
<p>概念：高层不依赖底层<br>
例子：高层台灯 和 底层 电路，需要插头、插座，而不是两者直接相连<br>
基于接口编程</p>
</blockquote>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

