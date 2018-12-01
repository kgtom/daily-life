

### 二分查找 vs 快速排序
**相同点:**
 两者都用到分治的思想很容易搞混。
**不同点**
二分查找 是用到分治但不一定要用递归去实现，可以通过迭代实现。

由于循环迭代相比递归少了很多内存分配和压栈的操作,开销会少很多，所以 二分查找最好做法通过循环实现。

**循环迭代**

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


**递归版本**

~~~go

//二分查找递归版本
func BinarySearchV2(arr []int, l, r, target int) int {
	for l <= r {
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



** 快排--递归/迭代版本 **
[详见](https://github.com/kgtom/back-end/blob/master/ds-and-alg/algorithms/sorting/quickSort.go)

