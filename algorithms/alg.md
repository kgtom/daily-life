---


---

<h2 id="常见排序算法">常见排序算法</h2>
<pre class=" language-go"><code class="prism  language-go">
<span class="token keyword">package</span> main

  

<span class="token keyword">import</span> <span class="token punctuation">(</span>

<span class="token string">"fmt"</span>

<span class="token string">"sort"</span>

<span class="token punctuation">)</span>

  

<span class="token keyword">func</span>  <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

  

arr  <span class="token operator">:=</span> <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">{</span><span class="token number">2</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">5</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">}</span>

  

<span class="token comment">//arr2 := sort.IntSlice(arr)</span>

<span class="token comment">//bubbleSortBySortpkg(arr2)</span>

<span class="token comment">//selectionSort(arr)</span>

  

<span class="token comment">//selectionSortBySortpkg(arr2)</span>

<span class="token comment">//insertionSort(arr)</span>

<span class="token comment">//insertionSortBypkg(arr2)</span>

<span class="token function">mergeSort</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span>

<span class="token comment">//quickSort(arr)</span>

fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span>

<span class="token punctuation">}</span>

  

<span class="token comment">//1.冒泡：相邻两个数，两两比较，交互位置，一次循环下来，最大值就在最后</span>

<span class="token comment">//时间复杂度O(n^2) n：多次循环</span>

<span class="token keyword">func</span>  <span class="token function">bubbleSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

<span class="token keyword">for</span>  i  <span class="token operator">:=</span>  <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span>  <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>

<span class="token keyword">for</span>  j  <span class="token operator">:=</span> i <span class="token operator">+</span>  <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span>  <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>

<span class="token keyword">if</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&gt;</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>

arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

<span class="token keyword">func</span>  <span class="token function">bubbleSortBySortpkg</span><span class="token punctuation">(</span>arr sort<span class="token punctuation">.</span>Interface<span class="token punctuation">)</span> <span class="token punctuation">{</span>

  

<span class="token keyword">for</span>  i  <span class="token operator">:=</span>  <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>

<span class="token keyword">for</span>  j  <span class="token operator">:=</span> i <span class="token operator">+</span>  <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>

<span class="token keyword">if</span> arr<span class="token punctuation">.</span><span class="token function">Less</span><span class="token punctuation">(</span>i<span class="token punctuation">,</span> j<span class="token punctuation">)</span> <span class="token punctuation">{</span>

  

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

  

<span class="token comment">//2.选择：选取特定索引值与数组其它元素比较</span>

<span class="token comment">//时间复杂度O(n^2)</span>

<span class="token keyword">func</span>  <span class="token function">selectionSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

<span class="token keyword">for</span>  i  <span class="token operator">:=</span>  <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span>  <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>

k  <span class="token operator">:=</span> i

<span class="token keyword">for</span>  j  <span class="token operator">:=</span> i <span class="token operator">+</span>  <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span>  <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>

<span class="token keyword">if</span> arr<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">&gt;</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>

k  <span class="token operator">=</span> j

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>k<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>k<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span>

<span class="token comment">//fmt.Println("a:", arr)</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

<span class="token keyword">func</span>  <span class="token function">selectionSortBySortpkg</span><span class="token punctuation">(</span>arr sort<span class="token punctuation">.</span>Interface<span class="token punctuation">)</span> <span class="token punctuation">{</span>

r  <span class="token operator">:=</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span>

<span class="token keyword">for</span>  i  <span class="token operator">:=</span>  <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> r<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>

min  <span class="token operator">:=</span> i

<span class="token keyword">for</span>  j  <span class="token operator">:=</span> i <span class="token operator">+</span>  <span class="token number">1</span><span class="token punctuation">;</span> j <span class="token operator">&lt;</span> r<span class="token punctuation">;</span> j<span class="token operator">++</span> <span class="token punctuation">{</span>

<span class="token keyword">if</span> arr<span class="token punctuation">.</span><span class="token function">Less</span><span class="token punctuation">(</span>j<span class="token punctuation">,</span> min<span class="token punctuation">)</span> <span class="token punctuation">{</span>

min  <span class="token operator">=</span> j

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

arr<span class="token punctuation">.</span><span class="token function">Swap</span><span class="token punctuation">(</span>i<span class="token punctuation">,</span> min<span class="token punctuation">)</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

  

<span class="token comment">//3.插入：一条记录插入到已排好的有序表中</span>

<span class="token keyword">func</span>  <span class="token function">insertionSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

<span class="token keyword">var</span>  n  <span class="token operator">=</span>  <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span>

<span class="token keyword">for</span>  i  <span class="token operator">:=</span>  <span class="token number">1</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> n<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>

j  <span class="token operator">:=</span> i

<span class="token keyword">for</span> j <span class="token operator">&gt;</span>  <span class="token number">0</span> <span class="token punctuation">{</span>

<span class="token comment">// fmt.Println("i:", i, "j:", j, "arr[j-1]:", arr[j-1], "arr[j]:", arr[j])</span>

<span class="token keyword">if</span> arr<span class="token punctuation">[</span>j<span class="token number">-1</span><span class="token punctuation">]</span> <span class="token operator">&gt;</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token punctuation">{</span>

arr<span class="token punctuation">[</span>j<span class="token number">-1</span><span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>j<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>j<span class="token number">-1</span><span class="token punctuation">]</span>

<span class="token punctuation">}</span>

<span class="token comment">//fmt.Println("a", arr)</span>

j  <span class="token operator">=</span> j <span class="token operator">-</span>  <span class="token number">1</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

  

<span class="token punctuation">}</span>

<span class="token keyword">func</span>  <span class="token function">insertionSortBypkg</span><span class="token punctuation">(</span>arr sort<span class="token punctuation">.</span>Interface<span class="token punctuation">)</span> <span class="token punctuation">{</span>

  

<span class="token keyword">var</span>  n  <span class="token operator">=</span> arr<span class="token punctuation">.</span><span class="token function">Len</span><span class="token punctuation">(</span><span class="token punctuation">)</span>

<span class="token keyword">for</span>  i  <span class="token operator">:=</span>  <span class="token number">1</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> n<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>

  

<span class="token keyword">for</span>  j  <span class="token operator">:=</span> i<span class="token punctuation">;</span> j <span class="token operator">&gt;</span>  <span class="token number">0</span>  <span class="token operator">&amp;&amp;</span> arr<span class="token punctuation">.</span><span class="token function">Less</span><span class="token punctuation">(</span>j<span class="token punctuation">,</span> j<span class="token number">-1</span><span class="token punctuation">)</span><span class="token punctuation">;</span> j<span class="token operator">--</span> <span class="token punctuation">{</span>

arr<span class="token punctuation">.</span><span class="token function">Swap</span><span class="token punctuation">(</span>j<span class="token punctuation">,</span> j<span class="token number">-1</span><span class="token punctuation">)</span>

<span class="token punctuation">}</span>

  

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

  

<span class="token comment">//4.归并排序的核心思想是将两个有序的数列合并成一个大的有序的序列。通过递归，层层合并，即为归并,利用二分法实现的排序算法，时间复杂度为nlogn.</span>

<span class="token keyword">func</span>  <span class="token function">mergeSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

<span class="token keyword">var</span>  s  <span class="token operator">=</span>  <span class="token function">make</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token operator">/</span><span class="token number">2</span><span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">)</span>

<span class="token keyword">if</span>  <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span> <span class="token operator">&lt;</span>  <span class="token number">2</span> <span class="token punctuation">{</span>

<span class="token keyword">return</span>

<span class="token punctuation">}</span>

  

mid  <span class="token operator">:=</span>  <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span> <span class="token operator">/</span>  <span class="token number">2</span>

  

<span class="token comment">//mergeSort(arr[:mid])</span>

<span class="token comment">//mergeSort(arr[mid:])</span>

  

<span class="token keyword">if</span> arr<span class="token punctuation">[</span>mid<span class="token number">-1</span><span class="token punctuation">]</span> <span class="token operator">&lt;=</span> arr<span class="token punctuation">[</span>mid<span class="token punctuation">]</span> <span class="token punctuation">{</span>

<span class="token keyword">return</span>

<span class="token punctuation">}</span>

  

<span class="token function">copy</span><span class="token punctuation">(</span>s<span class="token punctuation">,</span> arr<span class="token punctuation">[</span><span class="token punctuation">:</span>mid<span class="token punctuation">]</span><span class="token punctuation">)</span>

  

l<span class="token punctuation">,</span> r  <span class="token operator">:=</span>  <span class="token number">0</span><span class="token punctuation">,</span> mid

  

<span class="token keyword">for</span>  i  <span class="token operator">:=</span>  <span class="token number">0</span><span class="token punctuation">;</span> <span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>

<span class="token keyword">if</span> s<span class="token punctuation">[</span>l<span class="token punctuation">]</span> <span class="token operator">&lt;=</span> arr<span class="token punctuation">[</span>r<span class="token punctuation">]</span> <span class="token punctuation">{</span>

arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">=</span> s<span class="token punctuation">[</span>l<span class="token punctuation">]</span>

l<span class="token operator">++</span>

  

<span class="token keyword">if</span> l <span class="token operator">==</span> mid <span class="token punctuation">{</span>

<span class="token keyword">break</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>

arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>r<span class="token punctuation">]</span>

r<span class="token operator">++</span>

<span class="token keyword">if</span> r <span class="token operator">==</span>  <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span> <span class="token punctuation">{</span>

<span class="token function">copy</span><span class="token punctuation">(</span>arr<span class="token punctuation">[</span>i<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">:</span><span class="token punctuation">]</span><span class="token punctuation">,</span> s<span class="token punctuation">[</span>l<span class="token punctuation">:</span>mid<span class="token punctuation">]</span><span class="token punctuation">)</span>

<span class="token keyword">break</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

<span class="token keyword">return</span>

<span class="token punctuation">}</span>

  

<span class="token comment">//5.快速排序通过一个切分元素将数组分为两个子数组，左子数组小于等于切分元素，右子数组大于等于切分元素，将这两个子数组排序也就将整个数组排序了</span>

<span class="token keyword">func</span>  <span class="token function">quickSort</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

  

<span class="token function">quickSort2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token function">len</span><span class="token punctuation">(</span>arr<span class="token punctuation">)</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">)</span>

<span class="token comment">//recurse(0, len(arr), arr)</span>

fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"quick :"</span><span class="token punctuation">,</span> arr<span class="token punctuation">)</span>

  

<span class="token punctuation">}</span>

  

<span class="token keyword">func</span>  <span class="token function">recurse</span><span class="token punctuation">(</span>left <span class="token builtin">int</span><span class="token punctuation">,</span> right <span class="token builtin">int</span><span class="token punctuation">,</span> arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

<span class="token keyword">if</span> left <span class="token operator">&lt;</span> right <span class="token punctuation">{</span>

pivot  <span class="token operator">:=</span> <span class="token punctuation">(</span>right <span class="token operator">+</span> left<span class="token punctuation">)</span> <span class="token operator">/</span>  <span class="token number">2</span>

<span class="token comment">//fmt.Println("pivot:", pivot, left, right)</span>

pivot  <span class="token operator">=</span>  <span class="token function">partition</span><span class="token punctuation">(</span>left<span class="token punctuation">,</span> right<span class="token punctuation">,</span> pivot<span class="token punctuation">,</span> arr<span class="token punctuation">)</span>

<span class="token function">recurse</span><span class="token punctuation">(</span>left<span class="token punctuation">,</span> pivot<span class="token punctuation">,</span> arr<span class="token punctuation">)</span>

<span class="token function">recurse</span><span class="token punctuation">(</span>pivot<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">,</span> right<span class="token punctuation">,</span> arr<span class="token punctuation">)</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

<span class="token keyword">func</span>  <span class="token function">partition</span><span class="token punctuation">(</span>left <span class="token builtin">int</span><span class="token punctuation">,</span> right <span class="token builtin">int</span><span class="token punctuation">,</span> pivot <span class="token builtin">int</span><span class="token punctuation">,</span> arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token builtin">int</span> <span class="token punctuation">{</span>

v  <span class="token operator">:=</span> arr<span class="token punctuation">[</span>pivot<span class="token punctuation">]</span>

right<span class="token operator">--</span>

arr<span class="token punctuation">[</span>pivot<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>right<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>right<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>pivot<span class="token punctuation">]</span>

  

<span class="token keyword">for</span>  i  <span class="token operator">:=</span> left<span class="token punctuation">;</span> i <span class="token operator">&lt;</span> right<span class="token punctuation">;</span> i<span class="token operator">++</span> <span class="token punctuation">{</span>

<span class="token keyword">if</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span> <span class="token operator">&lt;=</span> v <span class="token punctuation">{</span>

arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>left<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>left<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>i<span class="token punctuation">]</span>

left<span class="token operator">++</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

  

arr<span class="token punctuation">[</span>left<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>right<span class="token punctuation">]</span> <span class="token operator">=</span> arr<span class="token punctuation">[</span>right<span class="token punctuation">]</span><span class="token punctuation">,</span> arr<span class="token punctuation">[</span>left<span class="token punctuation">]</span>

<span class="token keyword">return</span> left

  

<span class="token punctuation">}</span>

  

<span class="token keyword">func</span>  <span class="token function">quickSort2</span><span class="token punctuation">(</span>sortArray <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> left<span class="token punctuation">,</span> right <span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

<span class="token keyword">if</span> left <span class="token operator">&lt;</span> right <span class="token punctuation">{</span>

key  <span class="token operator">:=</span> sortArray<span class="token punctuation">[</span><span class="token punctuation">(</span>left<span class="token operator">+</span>right<span class="token punctuation">)</span><span class="token operator">/</span><span class="token number">2</span><span class="token punctuation">]</span>

i  <span class="token operator">:=</span> left

j  <span class="token operator">:=</span> right

  

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

  

<span class="token function">quickSort2</span><span class="token punctuation">(</span>sortArray<span class="token punctuation">,</span> left<span class="token punctuation">,</span> i<span class="token number">-1</span><span class="token punctuation">)</span>

<span class="token function">quickSort2</span><span class="token punctuation">(</span>sortArray<span class="token punctuation">,</span> j<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">,</span> right<span class="token punctuation">)</span>

<span class="token punctuation">}</span>

<span class="token punctuation">}</span>

  

<span class="token comment">//http://www.cnblogs.com/agui521/p/6918229.html</span>

<span class="token comment">//https://github.com/gaopeng527/go_Algorithm/blob/master/sort.go</span>

<span class="token comment">//https://blog.csdn.net/wangshubo1989/article/details/75135119</span>

<span class="token comment">//https://github.com/arnauddri/algorithms/</span>
~~

<span class="token operator">&gt;</span> Written with <span class="token punctuation">[</span>StackEdit<span class="token punctuation">]</span><span class="token punctuation">(</span>https<span class="token punctuation">:</span><span class="token operator">/</span><span class="token operator">/</span>stackedit<span class="token punctuation">.</span>io<span class="token operator">/</span><span class="token punctuation">)</span><span class="token punctuation">.</span>
</code></pre>

