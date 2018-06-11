

<h2 id="二分查找-vs-快速排序">二分查找 vs 快速排序</h2>
<p><strong>相同点</strong>:<br>
两者都用到分治的思想很容易搞混。</p>
<p><strong>不同点</strong>:<br>
binarySearch是用到分治但不一定一定要用递归去实现，可以通过循环迭代实现。<br>
由于循环相比递归少了很多内存分配和压栈的操作开销会少很多，所以binarySearch最好的实现方式是通过循环实现。</p>

<ul>
<li>循环迭代</li>
</ul>

~~~go
//二分查找迭代版本 LgN级别
func BinarySearch(arr []int, n int, searchVal int) int {
	//定义查找区间为[l,r]
	l, r := 0, n-1
	//历史上著名的bug l与r都是int的最大值得情况下则l+r越界
	//mid := (l+r)/2
	for l <= r {
		mid := l + (r-l)/2
		if arr[mid] == searchVal {
			return mid
		}
		if arr[mid] < searchVal {
			l = mid + 1

		} else {
			r = mid - 1
		}
	}
	//不存在此searchVal值
	return -1
}

~~~


<ul>
<li>递归版本</li>
</ul>

~~~go

//二分查找递归版本
func BinarySearchV2(arr []int, l, r, target int) int {
	if l <= r {
		mid := l + (r-l)/2
		if arr[mid] == target {
			return mid
		}
		if arr[mid] < target {
			return BinarySearchV2(arr, mid+1, r, target)
		} else {
			return BinarySearchV2(arr, l, mid-1, target)
		}
	}
	return -1
}

~~~

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

