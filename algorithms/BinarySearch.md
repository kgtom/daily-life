

<h2 id="二分查找-vs-快速排序">二分查找 vs 快速排序</h2>
<p><strong>相同点</strong>:<br>
两者都用到分治的思想很容易搞混。</p>
<p><strong>不同点</strong>:<br>
binarySearch是用到分治但不一定一定要用递归去实现，可以通过循环迭代实现。<br>
由于循环相比递归少了很多内存分配和压栈的操作开销会少很多，所以binarySearch最好的实现方式是通过循环实现。</p>
<p>*循环迭代</p>
<pre class=" language-go"><code class="prism  language-go"><span class="token comment">//二分查找迭代版本 LgN级别  </span>
<span class="token keyword">func</span> <span class="token function">BinarySearch</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> n <span class="token builtin">int</span><span class="token punctuation">,</span> searchVal <span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token builtin">int</span> <span class="token punctuation">{</span>  
   <span class="token comment">//定义查找区间为[l,r]  </span>
  l<span class="token punctuation">,</span> r <span class="token operator">:=</span> <span class="token number">0</span><span class="token punctuation">,</span> n<span class="token number">-1</span>  
  <span class="token comment">//历史上著名的bug l与r都是int的最大值得情况下则l+r越界  </span>
 <span class="token comment">//mid := (l+r)/2  </span>
  <span class="token keyword">for</span> l <span class="token operator">&lt;=</span> r <span class="token punctuation">{</span>  
      mid <span class="token operator">:=</span> l <span class="token operator">+</span> <span class="token punctuation">(</span>r<span class="token operator">-</span>l<span class="token punctuation">)</span><span class="token operator">/</span><span class="token number">2</span>  
  
  <span class="token keyword">if</span> arr<span class="token punctuation">[</span>mid<span class="token punctuation">]</span> <span class="token operator">==</span> searchVal <span class="token punctuation">{</span>  
         <span class="token keyword">return</span> mid  
      <span class="token punctuation">}</span>  
      <span class="token keyword">if</span> arr<span class="token punctuation">[</span>mid<span class="token punctuation">]</span> <span class="token operator">&lt;</span> searchVal <span class="token punctuation">{</span>  
         l <span class="token operator">=</span> mid <span class="token operator">+</span> <span class="token number">1</span>  
  
  <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
         r <span class="token operator">=</span> mid <span class="token operator">-</span> <span class="token number">1</span>  
  <span class="token punctuation">}</span>  
   <span class="token punctuation">}</span>  
   <span class="token comment">//不存在此searchVal值  </span>
  <span class="token keyword">return</span> <span class="token operator">-</span><span class="token number">1</span>  
<span class="token punctuation">}</span>

</code></pre>
<p>*递归版本</p>
<pre class=" language-go"><code class="prism  language-go"><span class="token comment">//二分查找递归版本  </span>
<span class="token keyword">func</span> <span class="token function">BinarySearchV2</span><span class="token punctuation">(</span>arr <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token builtin">int</span><span class="token punctuation">,</span> l<span class="token punctuation">,</span> r<span class="token punctuation">,</span> target <span class="token builtin">int</span><span class="token punctuation">)</span> <span class="token builtin">int</span> <span class="token punctuation">{</span>  
   <span class="token keyword">if</span> l <span class="token operator">&lt;=</span> r <span class="token punctuation">{</span>  
      mid <span class="token operator">:=</span> l <span class="token operator">+</span> <span class="token punctuation">(</span>r<span class="token operator">-</span>l<span class="token punctuation">)</span><span class="token operator">/</span><span class="token number">2</span>  
  <span class="token keyword">if</span> arr<span class="token punctuation">[</span>mid<span class="token punctuation">]</span> <span class="token operator">==</span> target <span class="token punctuation">{</span>  
         <span class="token keyword">return</span> mid  
      <span class="token punctuation">}</span>  
      <span class="token keyword">if</span> arr<span class="token punctuation">[</span>mid<span class="token punctuation">]</span> <span class="token operator">&lt;</span> target <span class="token punctuation">{</span>  
         <span class="token keyword">return</span> <span class="token function">BinarySearchV2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> mid<span class="token operator">+</span><span class="token number">1</span><span class="token punctuation">,</span> r<span class="token punctuation">,</span> target<span class="token punctuation">)</span>  
      <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>  
         <span class="token keyword">return</span> <span class="token function">BinarySearchV2</span><span class="token punctuation">(</span>arr<span class="token punctuation">,</span> l<span class="token punctuation">,</span> mid<span class="token number">-1</span><span class="token punctuation">,</span> target<span class="token punctuation">)</span>  
      <span class="token punctuation">}</span>  
   <span class="token punctuation">}</span>  
   <span class="token keyword">return</span> <span class="token operator">-</span><span class="token number">1</span>  
<span class="token punctuation">}</span>

</code></pre>
<ul>
<li>快排</li>
</ul>

~~~go
//第一种实现
func quickSort(arr []int) {

	quickSort2(arr, 0, len(arr)-1)

	fmt.Println("quick :", arr)

}

func quickSort2(arr []int, left, right int) {
	fmt.Println("left:", left, "right:", right)
	if left >= right {
		//起始位置大于或等于终止位置，说明不再需要排序
		return
	}

	//i := partition(arr, left, right)

	i := left

	j := right

	key := arr[(left+right)/2]

	for {

		for arr[i] < key {

			i++
		}

		for arr[j] > key {

			j--
		}

		if i >= j {

			break
		}
		arr[i], arr[j] = arr[j], arr[i]
		//fmt.Println("i:", i,"j:",j,"arr:",arr)
	}

	quickSort2(arr, left, i-1)

	quickSort2(arr, i+1, right)
	//fmt.Println("end")

}

~~~
<blockquote>
<p>reference:<br>
<a href="https://blog.csdn.net/qhrqhrqhr/article/details/50975717">https://blog.csdn.net/qhrqhrqhr/article/details/50975717</a><br>
<a href="https://github.com/bigbignerd/basicAlgorithm">https://github.com/bigbignerd/basicAlgorithm</a></p>
</blockquote>

