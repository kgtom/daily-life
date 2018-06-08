---


---

<h2 id="常见排序算法">常见排序算法</h2>
<h2 id="冒泡">1.冒泡</h2>
<pre class=" language-go"><code class="prism  language-go">
  
<span class="token comment">//1.冒泡：相邻两个数，两两比较，交互位置，一次循环下来，最大值就在最后  </span>
  
<span class="token comment">//时间复杂度O(n^2) n：多次循环  </span>
  
<span class="token keyword">func</span> <span class="token function">bubbleSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">if</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&gt;</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>  
  
            arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
  
         <span class="token punctuation">}</span>  
  
      <span class="token punctuation">}</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">bubbleSortBySortpkg</span><span class="token punctuation">(</span>arr sort<span class="token punctuation">.</span>Interface<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">if</span> arr<span class="token punctuation">.</span><span class="token function">Less</span><span class="token punctuation">(</span>i<span class="token punctuation">,</span> j<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
         <span class="token punctuation">}</span>  
  
      <span class="token punctuation">}</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>
</code></pre>
<h3 id="选择：选取特定索引值与数组其它元素比较">2.选择：选取特定索引值与数组其它元素比较</h3>
<pre class=" language-go"><code class="prism  language-go">
<span class="token comment">//2.选择：选取特定索引值与数组其它元素比较  </span>
  
<span class="token comment">//时间复杂度O(n^2)  </span>
  
<span class="token keyword">func</span> <span class="token function">selectionSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      k <span class="token operator">:=</span> i  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">if</span> arr<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">&gt;</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>  
  
            k <span class="token operator">=</span> j  
  
         <span class="token punctuation">}</span>  
  
      <span class="token punctuation">}</span>  
  
      arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>k<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
  
      <span class="token comment">//fmt.Println("a:", arr)  </span>
  
  <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">selectionSortBySortpkg</span><span class="token punctuation">(</span>arr sort<span class="token punctuation">.</span>Interface<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   r <span class="token operator">:=</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> r<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      min <span class="token operator">:=</span> i  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> r<span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">if</span> arr<span class="token punctuation">.</span><span class="token function">Less</span><span class="token punctuation">(</span>j<span class="token punctuation">,</span> min<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
            min <span class="token operator">=</span> j  
  
         <span class="token punctuation">}</span>  
  
      <span class="token punctuation">}</span>  
  
      arr<span class="token punctuation">.</span><span class="token function">Swap</span><span class="token punctuation">(</span>i<span class="token punctuation">,</span> min<span class="token punctuation">)</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>
</code></pre>
<h3 id="插入：一条记录插入到已排好的有序表中相邻两两交换较慢，改进方案：希尔排序">3.插入：一条记录插入到已排好的有序表中(相邻两两交换,较慢，改进方案：希尔排序)</h3>
<pre class=" language-go"><code class="prism  language-go"><span class="token comment">//3.插入：一条记录插入到已排好的有序表中(相邻两两交换,较慢，改进方案：希尔排序)  </span>
  
<span class="token keyword">func</span> <span class="token function">insertionSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">var</span> n <span class="token operator">=</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">1</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> n<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      j <span class="token operator">:=</span> i <span class="token comment">//第 j 元素是通过不断向左比较并交换  </span>
  
  <span class="token keyword">for</span> j <span class="token operator">&gt;</span> <span class="token number">0</span> <span class="token punctuation">{</span>  
  
         <span class="token comment">//fmt.Println("i:", i, "j:", j, "arr[j-1]:", arr[j-1], "arr[j]:", arr[j])  </span>
  
  <span class="token keyword">if</span> arr<span class="token punctuation">[</span>j<span class="token number">-1</span><span class="token punctuation">]</span> <span class="token operator">&gt;</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>  
  
            arr<span class="token punctuation">[</span>j<span class="token number">-1</span><span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token number">-1</span><span class="token punctuation">]</span>  
  
         <span class="token punctuation">}</span>  
  
         <span class="token comment">//fmt.Println("a", arr)  </span>
  
  j <span class="token operator">=</span> j <span class="token operator">-</span> <span class="token number">1</span>  
  
  <span class="token punctuation">}</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">insertionSortBypkg</span><span class="token punctuation">(</span>arr sort<span class="token punctuation">.</span>Interface<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">var</span> n <span class="token operator">=</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">1</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> n<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i<span class="token punctuation">;</span> j <span class="token operator">&gt;</span> <span class="token number">0</span> <span class="token operator">&amp;&amp;</span> arr<span class="token punctuation">.</span><span class="token function">Less</span><span class="token punctuation">(</span>j<span class="token punctuation">,</span> j<span class="token number">-1</span><span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">--</span> <span class="token punctuation">{</span>  
  
         arr<span class="token punctuation">.</span><span class="token function">Swap</span><span class="token punctuation">(</span>j<span class="token punctuation">,</span> j<span class="token number">-1</span><span class="token punctuation">)</span>  
  
      <span class="token punctuation">}</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>

</code></pre>
<h3 id="希尔排序">4.希尔排序</h3>
<pre class=" language-go"><code class="prism  language-go">
<span class="token comment">// 4.希尔排序：选择合适的插入排序对间隔 h,然后交换不相邻元素  </span>
<span class="token keyword">func</span> <span class="token function">ShellSort</span><span class="token punctuation">(</span>a <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   n <span class="token operator">:=</span> <span class="token function">len</span><span class="token punctuation">(</span>a<span class="token punctuation">)</span>  
   h <span class="token operator">:=</span> <span class="token number">1</span>  
  <span class="token keyword">for</span> h <span class="token operator">&lt;</span> n<span class="token operator">/</span><span class="token number">3</span> <span class="token punctuation">{</span> <span class="token comment">//寻找合适的间隔h  </span>
  h <span class="token operator">=</span> <span class="token number">3</span><span class="token operator">*</span>h <span class="token operator">+</span> <span class="token number">1</span>  
  <span class="token punctuation">}</span>  
   <span class="token keyword">for</span> h <span class="token operator">&gt;=</span> <span class="token number">1</span> <span class="token punctuation">{</span>  
      <span class="token comment">//将数组变为间隔h个元素有序  </span>
   <span class="token keyword">for</span> i <span class="token operator">:=</span> h<span class="token punctuation">;</span> i <span class="token operator">&lt;</span> n<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
         <span class="token comment">//间隔h插入排序  </span>
		  j <span class="token operator">:=</span> i  
         fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"h:"</span><span class="token punctuation">,</span> h<span class="token punctuation">,</span> <span class="token string">"j:"</span><span class="token punctuation">,</span> j<span class="token punctuation">,</span> <span class="token string">"a[j]:"</span><span class="token punctuation">,</span> a<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> <span class="token string">"a[j-h]:"</span><span class="token punctuation">,</span> a<span class="token punctuation">[</span>i<span class="token operator">-</span>h<span class="token punctuation">]</span><span class="token punctuation">,</span> a<span class="token punctuation">)</span>  
         <span class="token keyword">for</span> <span class="token punctuation">;</span> j <span class="token operator">&gt;=</span> h <span class="token operator">&amp;&amp;</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">&lt;</span> a<span class="token punctuation">[</span>j<span class="token operator">-</span>h<span class="token punctuation">]</span><span class="token punctuation">;</span> j <span class="token operator">-=</span> h <span class="token punctuation">{</span>  
            <span class="token comment">//swap(a, j, j-h)  </span>
			  a<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> a<span class="token punctuation">[</span>j<span class="token operator">-</span>h<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>j<span class="token operator">-</span>h<span class="token punctuation">]</span><span class="token punctuation">,</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span>  
            fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"swap:"</span><span class="token punctuation">,</span> a<span class="token punctuation">)</span>  
         <span class="token punctuation">}</span>  
      <span class="token punctuation">}</span>  
      h <span class="token operator">/=</span> <span class="token number">3</span>  
  <span class="token punctuation">}</span>  
   fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"h:"</span><span class="token punctuation">,</span> h<span class="token punctuation">)</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">swap</span><span class="token punctuation">(</span>slice <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> i <span class="token builtin">int</span><span class="token punctuation">,</span> j <span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
   slice<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> slice<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> slice<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> slice<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
<span class="token punctuation">}</span>
</code></pre>
<h3 id="归并排序">5.归并排序</h3>
<pre class=" language-go"><code class="prism  language-go"><span class="token comment">//5.归并排序的核心思想是将两个有序的数列合并成一个大的有序的序列。通过递归，层层合并，即为归并,利用二分法实现的排序算法，时间复杂度为nlogn.  </span>
  
<span class="token keyword">func</span> <span class="token function">MergeSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
   temp <span class="token operator">:=</span> <span class="token function">make</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token comment">//辅助切片  </span>
  <span class="token function">mergeSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">,</span> temp<span class="token punctuation">)</span>  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">mergeSort2</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> low<span class="token punctuation">,</span> high <span class="token builtin">int</span><span class="token punctuation">,</span> temp <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">if</span> low <span class="token operator">&lt;</span> high <span class="token punctuation">{</span>  
  
      mid <span class="token operator">:=</span> <span class="token punctuation">(</span>low <span class="token operator">+</span> high<span class="token punctuation">)</span> <span class="token operator">/</span> <span class="token number">2</span>  
  fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"low:"</span><span class="token punctuation">,</span> low<span class="token punctuation">,</span> <span class="token string">"mid:"</span><span class="token punctuation">,</span> mid<span class="token punctuation">)</span>  
      <span class="token function">mergeSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> low<span class="token punctuation">,</span> mid<span class="token punctuation">,</span> temp<span class="token punctuation">)</span>    <span class="token comment">//分  </span>
  <span class="token function">mergeSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> mid<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">,</span> high<span class="token punctuation">,</span> temp<span class="token punctuation">)</span> <span class="token comment">//分  </span>
  
  <span class="token function">merge</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> low<span class="token punctuation">,</span> mid<span class="token punctuation">,</span> high<span class="token punctuation">,</span> temp<span class="token punctuation">)</span> <span class="token comment">//合  </span>
  
  <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">merge</span><span class="token punctuation">(</span>a <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> first<span class="token punctuation">,</span> middle<span class="token punctuation">,</span> end <span class="token builtin">int</span><span class="token punctuation">,</span> temp <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   i<span class="token punctuation">,</span> m<span class="token punctuation">,</span> j<span class="token punctuation">,</span> n<span class="token punctuation">,</span> k <span class="token operator">:=</span> first<span class="token punctuation">,</span> middle<span class="token punctuation">,</span> middle<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">,</span> end<span class="token punctuation">,</span> <span class="token number">0</span>  
  <span class="token keyword">for</span> i <span class="token operator">&lt;=</span> m <span class="token operator">&amp;&amp;</span> j <span class="token operator">&lt;=</span> n <span class="token punctuation">{</span>  
      <span class="token keyword">if</span> a<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&lt;=</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>  
         temp<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
         k<span class="token operator">++</span>  
         i<span class="token operator">++</span>  
      <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
         temp<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span>  
         k<span class="token operator">++</span>  
         j<span class="token operator">++</span>  
      <span class="token punctuation">}</span>  
   <span class="token punctuation">}</span>  
   <span class="token keyword">for</span> i <span class="token operator">&lt;=</span> m <span class="token punctuation">{</span>  
      temp<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
      k<span class="token operator">++</span>  
      i<span class="token operator">++</span>  
   <span class="token punctuation">}</span>  
   <span class="token keyword">for</span> j <span class="token operator">&lt;=</span> n <span class="token punctuation">{</span>  
      temp<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span>  
      k<span class="token operator">++</span>  
      j<span class="token operator">++</span>  
   <span class="token punctuation">}</span>  
   <span class="token keyword">for</span> ii <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> ii <span class="token operator">&lt;</span> k<span class="token punctuation">;</span> ii<span class="token operator">++</span> <span class="token punctuation">{</span>  
      a<span class="token punctuation">[</span>first<span class="token operator">+</span>ii<span class="token punctuation">]</span> <span class="token operator">=</span> temp<span class="token punctuation">[</span>ii<span class="token punctuation">]</span>  
   <span class="token punctuation">}</span>  
   fmt<span class="token punctuation">.</span><span class="token function">Printf</span><span class="token punctuation">(</span><span class="token string">"sort: arr: %v\n"</span><span class="token punctuation">,</span> a<span class="token punctuation">)</span>  
<span class="token punctuation">}</span>

</code></pre>
<h3 id="快排">6.快排</h3>
<pre class=" language-go"><code class="prism  language-go"><span class="token comment">//6.快速排序通过一个切分元素将数组分为两个子数组，左子数组小于等于切分元素，右子数组大于等于切分元素，将这两个子数组排序也就将整个数组排序了  </span>
  
<span class="token comment">//第二种实现  </span>
<span class="token keyword">func</span> <span class="token function">QuickSort8</span><span class="token punctuation">(</span>list <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
   <span class="token keyword">if</span> <span class="token function">len</span><span class="token punctuation">(</span>list<span class="token punctuation">)</span> <span class="token operator">&lt;=</span> <span class="token number">1</span> <span class="token punctuation">{</span>  
      <span class="token keyword">return</span> <span class="token comment">//退出条件  </span>
  <span class="token punctuation">}</span>  
   i<span class="token punctuation">,</span> j <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>list<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span>  
  index <span class="token operator">:=</span> <span class="token number">1</span> <span class="token comment">//表示第一次比较的索引位置.  </span>
  key <span class="token operator">:=</span> list<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token comment">//第一次比较的参考值.这里选择第一个数  </span>
  <span class="token keyword">if</span> list<span class="token punctuation">[</span>index<span class="token punctuation">]</span> <span class="token operator">&gt;</span> key <span class="token punctuation">{</span>  
      list<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> list<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> list<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> list<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
      j<span class="token operator">--</span> <span class="token comment">//表示取大值跟末尾的数替换位置,使大于参考值的数在后面  </span>
  <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
      list<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> list<span class="token punctuation">[</span>index<span class="token punctuation">]</span> <span class="token operator">=</span> list<span class="token punctuation">[</span>index<span class="token punctuation">]</span><span class="token punctuation">,</span> list<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
      i<span class="token operator">++</span> <span class="token comment">//表示取小的值跟前面的替换位置,使小于参考值的数在前面  </span>
  index<span class="token operator">++</span>  
   <span class="token punctuation">}</span>  
   <span class="token function">QuickSort8</span><span class="token punctuation">(</span>list<span class="token punctuation">[</span><span class="token punctuation">:</span>i<span class="token punctuation">]</span><span class="token punctuation">)</span>  
   <span class="token function">QuickSort8</span><span class="token punctuation">(</span>list<span class="token punctuation">[</span>i<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">:</span><span class="token punctuation">]</span><span class="token punctuation">)</span>  
<span class="token punctuation">}</span>  
  
  
<span class="token comment">//第一种实现  </span>
<span class="token keyword">func</span> <span class="token function">quickSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token function">quickSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">)</span>  
  
   <span class="token comment">//recurse(0, len(arr), arr)  </span>
  
  fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"quick :"</span><span class="token punctuation">,</span> arr<span class="token punctuation">)</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">quickSort2</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> left<span class="token punctuation">,</span> right <span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
   fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"left:"</span><span class="token punctuation">,</span> left<span class="token punctuation">,</span> <span class="token string">"right:"</span><span class="token punctuation">,</span> right<span class="token punctuation">)</span>  
   <span class="token keyword">if</span> left <span class="token operator">&gt;=</span> right <span class="token punctuation">{</span>  
      <span class="token comment">//起始位置大于或等于终止位置，说明不再需要排序  </span>
  <span class="token keyword">return</span>  
  <span class="token punctuation">}</span>  
  
   <span class="token comment">//i := partition(arr, left, right)  </span>
  
  i <span class="token operator">:=</span> left  
  
   j <span class="token operator">:=</span> right  
  
   key <span class="token operator">:=</span> arr<span class="token punctuation">[</span><span class="token punctuation">(</span>left<span class="token operator">+</span>right<span class="token punctuation">)</span><span class="token operator">/</span><span class="token number">2</span><span class="token punctuation">]</span>  
  
   <span class="token keyword">for</span> <span class="token punctuation">{</span>  
  
      <span class="token keyword">for</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&lt;</span> key <span class="token punctuation">{</span>  
  
         i<span class="token operator">++</span>  
  
      <span class="token punctuation">}</span>  
  
      <span class="token keyword">for</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">&gt;</span> key <span class="token punctuation">{</span>  
  
         j<span class="token operator">--</span>  
  
      <span class="token punctuation">}</span>  
  
      <span class="token keyword">if</span> i <span class="token operator">&gt;=</span> j <span class="token punctuation">{</span>  
  
         <span class="token keyword">break</span>  
  
  <span class="token punctuation">}</span>  
  
      arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
      <span class="token comment">//fmt.Println("i:", i,"j:",j,"arr:",arr)  </span>
  <span class="token punctuation">}</span>  
  
   <span class="token function">quickSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> left<span class="token punctuation">,</span> i<span class="token number">-1</span><span class="token punctuation">)</span>  
  
   <span class="token function">quickSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> i<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">,</span> right<span class="token punctuation">)</span>  
   <span class="token comment">//fmt.Println("end")  </span>
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">partition</span><span class="token punctuation">(</span>sortArray <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> left<span class="token punctuation">,</span> right <span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token builtin">int</span> <span class="token punctuation">{</span>  
   i <span class="token operator">:=</span> left  
  
   j <span class="token operator">:=</span> right  
  
   <span class="token keyword">if</span> left <span class="token operator">&lt;</span> right <span class="token punctuation">{</span>  
  
      key <span class="token operator">:=</span> sortArray<span class="token punctuation">[</span><span class="token punctuation">(</span>left<span class="token operator">+</span>right<span class="token punctuation">)</span><span class="token operator">/</span><span class="token number">2</span><span class="token punctuation">]</span>  
  
      <span class="token keyword">for</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">for</span> sortArray<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&lt;</span> key <span class="token punctuation">{</span>  
  
            i<span class="token operator">++</span>  
  
         <span class="token punctuation">}</span>  
  
         <span class="token keyword">for</span> sortArray<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">&gt;</span> key <span class="token punctuation">{</span>  
  
            j<span class="token operator">--</span>  
  
         <span class="token punctuation">}</span>  
  
         <span class="token keyword">if</span> i <span class="token operator">&gt;=</span> j <span class="token punctuation">{</span>  
  
            <span class="token keyword">break</span>  
  
  <span class="token punctuation">}</span>  
  
         sortArray<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> sortArray<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> sortArray<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> sortArray<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
  
      <span class="token punctuation">}</span>  
      fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"i:"</span><span class="token punctuation">,</span> i<span class="token punctuation">)</span>  
  
   <span class="token punctuation">}</span>  
   <span class="token keyword">return</span> i  
<span class="token punctuation">}</span>

</code></pre>
<h3 id="完整源码">完整源码</h3>
<pre class=" language-go"><code class="prism  language-go"><span class="token comment">/*  
总结：  
快速排序是最快的通用排序算法，它的内循环的指令很少，而且它还能利用缓存，因为它总是顺序地访问数据。它的运行时间近似为 ~cNlogN，这里的 c 比其他线性对数级别的排序算法都要小  
*/</span>  
  
<span class="token keyword">package</span> main  
  
<span class="token keyword">import</span> <span class="token punctuation">(</span>  
   <span class="token string">"fmt"</span>  
  
 <span class="token string">"sort"</span><span class="token punctuation">)</span>  
  
<span class="token keyword">func</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   arr <span class="token operator">:=</span> <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">{</span><span class="token number">6</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">8</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">}</span>  
  
   <span class="token comment">//arr2 := sort.IntSlice(arr)  </span>
  
 <span class="token comment">//bubbleSortBySortpkg(arr2)  </span>
 <span class="token comment">//selectionSort(arr)  </span>
 <span class="token comment">//selectionSortBySortpkg(arr2)  </span>
 <span class="token comment">//insertionSort(arr)  </span>
 <span class="token comment">//insertionSortBypkg(arr2)  </span>
 <span class="token comment">//MergeSort(arr)  </span>
  <span class="token function">quickSort</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span>  
   <span class="token comment">//QuickSort8(arr)  </span>
 <span class="token comment">//ShellSort(arr)  </span>
  fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"res:"</span><span class="token punctuation">,</span> arr<span class="token punctuation">)</span>  
  
<span class="token punctuation">}</span>  
  
  
  
<span class="token comment">//1.冒泡：相邻两个数，两两比较，交互位置，一次循环下来，最大值就在最后  </span>
  
<span class="token comment">//时间复杂度O(n^2) n：多次循环  </span>
  
<span class="token keyword">func</span> <span class="token function">bubbleSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">if</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&gt;</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>  
  
            arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
  
         <span class="token punctuation">}</span>  
  
      <span class="token punctuation">}</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">bubbleSortBySortpkg</span><span class="token punctuation">(</span>arr sort<span class="token punctuation">.</span>Interface<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">if</span> arr<span class="token punctuation">.</span><span class="token function">Less</span><span class="token punctuation">(</span>i<span class="token punctuation">,</span> j<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
         <span class="token punctuation">}</span>  
  
      <span class="token punctuation">}</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token comment">//2.选择：选取特定索引值与数组其它元素比较  </span>
  
<span class="token comment">//时间复杂度O(n^2)  </span>
  
<span class="token keyword">func</span> <span class="token function">selectionSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      k <span class="token operator">:=</span> i  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">if</span> arr<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">&gt;</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>  
  
            k <span class="token operator">=</span> j  
  
         <span class="token punctuation">}</span>  
  
      <span class="token punctuation">}</span>  
  
      arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>k<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
  
      <span class="token comment">//fmt.Println("a:", arr)  </span>
  
  <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">selectionSortBySortpkg</span><span class="token punctuation">(</span>arr sort<span class="token punctuation">.</span>Interface<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   r <span class="token operator">:=</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> r<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      min <span class="token operator">:=</span> i  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> r<span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">if</span> arr<span class="token punctuation">.</span><span class="token function">Less</span><span class="token punctuation">(</span>j<span class="token punctuation">,</span> min<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
            min <span class="token operator">=</span> j  
  
         <span class="token punctuation">}</span>  
  
      <span class="token punctuation">}</span>  
  
      arr<span class="token punctuation">.</span><span class="token function">Swap</span><span class="token punctuation">(</span>i<span class="token punctuation">,</span> min<span class="token punctuation">)</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token comment">//3.插入：一条记录插入到已排好的有序表中(相邻两两交换,较慢，改进方案：希尔排序)  </span>
  
<span class="token keyword">func</span> <span class="token function">insertionSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">var</span> n <span class="token operator">=</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">1</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> n<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      j <span class="token operator">:=</span> i <span class="token comment">//第 j 元素是通过不断向左比较并交换  </span>
  
  <span class="token keyword">for</span> j <span class="token operator">&gt;</span> <span class="token number">0</span> <span class="token punctuation">{</span>  
  
         <span class="token comment">//fmt.Println("i:", i, "j:", j, "arr[j-1]:", arr[j-1], "arr[j]:", arr[j])  </span>
  
  <span class="token keyword">if</span> arr<span class="token punctuation">[</span>j<span class="token number">-1</span><span class="token punctuation">]</span> <span class="token operator">&gt;</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>  
  
            arr<span class="token punctuation">[</span>j<span class="token number">-1</span><span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token number">-1</span><span class="token punctuation">]</span>  
  
         <span class="token punctuation">}</span>  
  
         <span class="token comment">//fmt.Println("a", arr)  </span>
  
  j <span class="token operator">=</span> j <span class="token operator">-</span> <span class="token number">1</span>  
  
  <span class="token punctuation">}</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">insertionSortBypkg</span><span class="token punctuation">(</span>arr sort<span class="token punctuation">.</span>Interface<span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">var</span> n <span class="token operator">=</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span>  
  
   <span class="token keyword">for</span> i <span class="token operator">:=</span> <span class="token number">1</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> n<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
  
      <span class="token keyword">for</span> j <span class="token operator">:=</span> i<span class="token punctuation">;</span> j <span class="token operator">&gt;</span> <span class="token number">0</span> <span class="token operator">&amp;&amp;</span> arr<span class="token punctuation">.</span><span class="token function">Less</span><span class="token punctuation">(</span>j<span class="token punctuation">,</span> j<span class="token number">-1</span><span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">--</span> <span class="token punctuation">{</span>  
  
         arr<span class="token punctuation">.</span><span class="token function">Swap</span><span class="token punctuation">(</span>j<span class="token punctuation">,</span> j<span class="token number">-1</span><span class="token punctuation">)</span>  
  
      <span class="token punctuation">}</span>  
  
   <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token comment">//4.归并排序的核心思想是将两个有序的数列合并成一个大的有序的序列。通过递归，层层合并，即为归并,利用二分法实现的排序算法，时间复杂度为nlogn.  </span>
  
<span class="token keyword">func</span> <span class="token function">MergeSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
   temp <span class="token operator">:=</span> <span class="token function">make</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token comment">//辅助切片  </span>
  <span class="token function">mergeSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">,</span> temp<span class="token punctuation">)</span>  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">mergeSort2</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> low<span class="token punctuation">,</span> high <span class="token builtin">int</span><span class="token punctuation">,</span> temp <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token keyword">if</span> low <span class="token operator">&lt;</span> high <span class="token punctuation">{</span>  
  
      mid <span class="token operator">:=</span> <span class="token punctuation">(</span>low <span class="token operator">+</span> high<span class="token punctuation">)</span> <span class="token operator">/</span> <span class="token number">2</span>  
  fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"low:"</span><span class="token punctuation">,</span> low<span class="token punctuation">,</span> <span class="token string">"mid:"</span><span class="token punctuation">,</span> mid<span class="token punctuation">)</span>  
      <span class="token function">mergeSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> low<span class="token punctuation">,</span> mid<span class="token punctuation">,</span> temp<span class="token punctuation">)</span>    <span class="token comment">//分  </span>
  <span class="token function">mergeSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> mid<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">,</span> high<span class="token punctuation">,</span> temp<span class="token punctuation">)</span> <span class="token comment">//分  </span>
  
  <span class="token function">merge</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> low<span class="token punctuation">,</span> mid<span class="token punctuation">,</span> high<span class="token punctuation">,</span> temp<span class="token punctuation">)</span> <span class="token comment">//合  </span>
  
  <span class="token punctuation">}</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">merge</span><span class="token punctuation">(</span>a <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> first<span class="token punctuation">,</span> middle<span class="token punctuation">,</span> end <span class="token builtin">int</span><span class="token punctuation">,</span> temp <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   i<span class="token punctuation">,</span> m<span class="token punctuation">,</span> j<span class="token punctuation">,</span> n<span class="token punctuation">,</span> k <span class="token operator">:=</span> first<span class="token punctuation">,</span> middle<span class="token punctuation">,</span> middle<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">,</span> end<span class="token punctuation">,</span> <span class="token number">0</span>  
  <span class="token keyword">for</span> i <span class="token operator">&lt;=</span> m <span class="token operator">&amp;&amp;</span> j <span class="token operator">&lt;=</span> n <span class="token punctuation">{</span>  
      <span class="token keyword">if</span> a<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&lt;=</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>  
         temp<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
         k<span class="token operator">++</span>  
         i<span class="token operator">++</span>  
      <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
         temp<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span>  
         k<span class="token operator">++</span>  
         j<span class="token operator">++</span>  
      <span class="token punctuation">}</span>  
   <span class="token punctuation">}</span>  
   <span class="token keyword">for</span> i <span class="token operator">&lt;=</span> m <span class="token punctuation">{</span>  
      temp<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
      k<span class="token operator">++</span>  
      i<span class="token operator">++</span>  
   <span class="token punctuation">}</span>  
   <span class="token keyword">for</span> j <span class="token operator">&lt;=</span> n <span class="token punctuation">{</span>  
      temp<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span>  
      k<span class="token operator">++</span>  
      j<span class="token operator">++</span>  
   <span class="token punctuation">}</span>  
   <span class="token keyword">for</span> ii <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">;</span> ii <span class="token operator">&lt;</span> k<span class="token punctuation">;</span> ii<span class="token operator">++</span> <span class="token punctuation">{</span>  
      a<span class="token punctuation">[</span>first<span class="token operator">+</span>ii<span class="token punctuation">]</span> <span class="token operator">=</span> temp<span class="token punctuation">[</span>ii<span class="token punctuation">]</span>  
   <span class="token punctuation">}</span>  
   fmt<span class="token punctuation">.</span><span class="token function">Printf</span><span class="token punctuation">(</span><span class="token string">"sort: arr: %v\n"</span><span class="token punctuation">,</span> a<span class="token punctuation">)</span>  
<span class="token punctuation">}</span>  
  
<span class="token comment">//5.快速排序通过一个切分元素将数组分为两个子数组，左子数组小于等于切分元素，右子数组大于等于切分元素，将这两个子数组排序也就将整个数组排序了  </span>
  
<span class="token comment">//第二种实现  </span>
<span class="token keyword">func</span> <span class="token function">QuickSort8</span><span class="token punctuation">(</span>list <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
   <span class="token keyword">if</span> <span class="token function">len</span><span class="token punctuation">(</span>list<span class="token punctuation">)</span> <span class="token operator">&lt;=</span> <span class="token number">1</span> <span class="token punctuation">{</span>  
      <span class="token keyword">return</span> <span class="token comment">//退出条件  </span>
  <span class="token punctuation">}</span>  
   i<span class="token punctuation">,</span> j <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>list<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span>  
  index <span class="token operator">:=</span> <span class="token number">1</span> <span class="token comment">//表示第一次比较的索引位置.  </span>
  key <span class="token operator">:=</span> list<span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span> <span class="token comment">//第一次比较的参考值.这里选择第一个数  </span>
  <span class="token keyword">if</span> list<span class="token punctuation">[</span>index<span class="token punctuation">]</span> <span class="token operator">&gt;</span> key <span class="token punctuation">{</span>  
      list<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> list<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> list<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> list<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
      j<span class="token operator">--</span> <span class="token comment">//表示取大值跟末尾的数替换位置,使大于参考值的数在后面  </span>
  <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
      list<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> list<span class="token punctuation">[</span>index<span class="token punctuation">]</span> <span class="token operator">=</span> list<span class="token punctuation">[</span>index<span class="token punctuation">]</span><span class="token punctuation">,</span> list<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
      i<span class="token operator">++</span> <span class="token comment">//表示取小的值跟前面的替换位置,使小于参考值的数在前面  </span>
  index<span class="token operator">++</span>  
   <span class="token punctuation">}</span>  
   <span class="token function">QuickSort8</span><span class="token punctuation">(</span>list<span class="token punctuation">[</span><span class="token punctuation">:</span>i<span class="token punctuation">]</span><span class="token punctuation">)</span>  
   <span class="token function">QuickSort8</span><span class="token punctuation">(</span>list<span class="token punctuation">[</span>i<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">:</span><span class="token punctuation">]</span><span class="token punctuation">)</span>  
<span class="token punctuation">}</span>  
  
  
<span class="token comment">//第一种实现  </span>
<span class="token keyword">func</span> <span class="token function">quickSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   <span class="token function">quickSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">)</span>  
  
   <span class="token comment">//recurse(0, len(arr), arr)  </span>
  
  fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"quick :"</span><span class="token punctuation">,</span> arr<span class="token punctuation">)</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">quickSort2</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> left<span class="token punctuation">,</span> right <span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
   fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"left:"</span><span class="token punctuation">,</span> left<span class="token punctuation">,</span> <span class="token string">"right:"</span><span class="token punctuation">,</span> right<span class="token punctuation">)</span>  
   <span class="token keyword">if</span> left <span class="token operator">&gt;=</span> right <span class="token punctuation">{</span>  
      <span class="token comment">//起始位置大于或等于终止位置，说明不再需要排序  </span>
  <span class="token keyword">return</span>  
  <span class="token punctuation">}</span>  
  
   <span class="token comment">//i := partition(arr, left, right)  </span>
  
  i <span class="token operator">:=</span> left  
  
   j <span class="token operator">:=</span> right  
  
   key <span class="token operator">:=</span> arr<span class="token punctuation">[</span><span class="token punctuation">(</span>left<span class="token operator">+</span>right<span class="token punctuation">)</span><span class="token operator">/</span><span class="token number">2</span><span class="token punctuation">]</span>  
  
   <span class="token keyword">for</span> <span class="token punctuation">{</span>  
  
      <span class="token keyword">for</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&lt;</span> key <span class="token punctuation">{</span>  
  
         i<span class="token operator">++</span>  
  
      <span class="token punctuation">}</span>  
  
      <span class="token keyword">for</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">&gt;</span> key <span class="token punctuation">{</span>  
  
         j<span class="token operator">--</span>  
  
      <span class="token punctuation">}</span>  
  
      <span class="token keyword">if</span> i <span class="token operator">&gt;=</span> j <span class="token punctuation">{</span>  
  
         <span class="token keyword">break</span>  
  
  <span class="token punctuation">}</span>  
  
      arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
      <span class="token comment">//fmt.Println("i:", i,"j:",j,"arr:",arr)  </span>
  <span class="token punctuation">}</span>  
  
   <span class="token function">quickSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> left<span class="token punctuation">,</span> i<span class="token number">-1</span><span class="token punctuation">)</span>  
  
   <span class="token function">quickSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> i<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">,</span> right<span class="token punctuation">)</span>  
   <span class="token comment">//fmt.Println("end")  </span>
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">partition</span><span class="token punctuation">(</span>sortArray <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> left<span class="token punctuation">,</span> right <span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token builtin">int</span> <span class="token punctuation">{</span>  
   i <span class="token operator">:=</span> left  
  
   j <span class="token operator">:=</span> right  
  
   <span class="token keyword">if</span> left <span class="token operator">&lt;</span> right <span class="token punctuation">{</span>  
  
      key <span class="token operator">:=</span> sortArray<span class="token punctuation">[</span><span class="token punctuation">(</span>left<span class="token operator">+</span>right<span class="token punctuation">)</span><span class="token operator">/</span><span class="token number">2</span><span class="token punctuation">]</span>  
  
      <span class="token keyword">for</span> <span class="token punctuation">{</span>  
  
         <span class="token keyword">for</span> sortArray<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&lt;</span> key <span class="token punctuation">{</span>  
  
            i<span class="token operator">++</span>  
  
         <span class="token punctuation">}</span>  
  
         <span class="token keyword">for</span> sortArray<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">&gt;</span> key <span class="token punctuation">{</span>  
  
            j<span class="token operator">--</span>  
  
         <span class="token punctuation">}</span>  
  
         <span class="token keyword">if</span> i <span class="token operator">&gt;=</span> j <span class="token punctuation">{</span>  
  
            <span class="token keyword">break</span>  
  
  <span class="token punctuation">}</span>  
  
         sortArray<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> sortArray<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> sortArray<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> sortArray<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
  
      <span class="token punctuation">}</span>  
      fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"i:"</span><span class="token punctuation">,</span> i<span class="token punctuation">)</span>  
  
   <span class="token punctuation">}</span>  
   <span class="token keyword">return</span> i  
<span class="token punctuation">}</span>  
  
<span class="token comment">// 希尔排序：选择合适的插入排序对间隔 h,然后交换不相邻元素  </span>
<span class="token keyword">func</span> <span class="token function">ShellSort</span><span class="token punctuation">(</span>a <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
  
   n <span class="token operator">:=</span> <span class="token function">len</span><span class="token punctuation">(</span>a<span class="token punctuation">)</span>  
   h <span class="token operator">:=</span> <span class="token number">1</span>  
  <span class="token keyword">for</span> h <span class="token operator">&lt;</span> n<span class="token operator">/</span><span class="token number">3</span> <span class="token punctuation">{</span> <span class="token comment">//寻找合适的间隔h  </span>
  h <span class="token operator">=</span> <span class="token number">3</span><span class="token operator">*</span>h <span class="token operator">+</span> <span class="token number">1</span>  
  <span class="token punctuation">}</span>  
   <span class="token keyword">for</span> h <span class="token operator">&gt;=</span> <span class="token number">1</span> <span class="token punctuation">{</span>  
      <span class="token comment">//将数组变为间隔h个元素有序  </span>
  <span class="token keyword">for</span> i <span class="token operator">:=</span> h<span class="token punctuation">;</span> i <span class="token operator">&lt;</span> n<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>  
         <span class="token comment">//间隔h插入排序  </span>
         j <span class="token operator">:=</span> i  
         fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"h:"</span><span class="token punctuation">,</span> h<span class="token punctuation">,</span> <span class="token string">"j:"</span><span class="token punctuation">,</span> j<span class="token punctuation">,</span> <span class="token string">"a[j]:"</span><span class="token punctuation">,</span> a<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> <span class="token string">"a[j-h]:"</span><span class="token punctuation">,</span> a<span class="token punctuation">[</span>i<span class="token operator">-</span>h<span class="token punctuation">]</span><span class="token punctuation">,</span> a<span class="token punctuation">)</span>  
         <span class="token keyword">for</span> <span class="token punctuation">;</span> j <span class="token operator">&gt;=</span> h <span class="token operator">&amp;&amp;</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">&lt;</span> a<span class="token punctuation">[</span>j<span class="token operator">-</span>h<span class="token punctuation">]</span><span class="token punctuation">;</span> j <span class="token operator">-=</span> h <span class="token punctuation">{</span>  
            <span class="token comment">//swap(a, j, j-h)  </span>
        a<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> a<span class="token punctuation">[</span>j<span class="token operator">-</span>h<span class="token punctuation">]</span> <span class="token operator">=</span> a<span class="token punctuation">[</span>j<span class="token operator">-</span>h<span class="token punctuation">]</span><span class="token punctuation">,</span> a<span class="token punctuation">[</span>j<span class="token punctuation">]</span>  
            fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"swap:"</span><span class="token punctuation">,</span> a<span class="token punctuation">)</span>  
         <span class="token punctuation">}</span>  
      <span class="token punctuation">}</span>  
      h <span class="token operator">/=</span> <span class="token number">3</span>  
  <span class="token punctuation">}</span>  
   fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"h:"</span><span class="token punctuation">,</span> h<span class="token punctuation">)</span>  
  
<span class="token punctuation">}</span>  
  
<span class="token keyword">func</span> <span class="token function">swap</span><span class="token punctuation">(</span>slice <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> i <span class="token builtin">int</span><span class="token punctuation">,</span> j <span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>  
   slice<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> slice<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> slice<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> slice<span class="token punctuation">[</span>i<span class="token punctuation">]</span>  
<span class="token punctuation">}</span>

</code></pre>
<blockquote>
<p>reference:<br>
<a href="http://www.cnblogs.com/agui521/p/6918229.html">http://www.cnblogs.com/agui521/p/6918229.html</a></p>
</blockquote>
<p><a href="https://github.com/gaopeng527/go_Algorithm/blob/master/sort.go">https://github.com/gaopeng527/go_Algorithm/blob/master/sort.go</a></p>
<p><a href="https://blog.csdn.net/wangshubo1989/article/details/75135119">https://blog.csdn.net/wangshubo1989/article/details/75135119</a></p>
<p><a href="https://github.com/arnauddri/algorithms/">https://github.com/arnauddri/algorithms/</a></p>

