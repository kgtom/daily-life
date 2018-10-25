

### 二分查找 vs 快速排序
**相同点:**
 两者都用到分治的思想很容易搞混。
**不同点**
binarySearch是用到分治但不一定一定要用递归去实现，可以通过循环迭代实现。

由于循环相比递归少了很多内存分配和压栈的操作,开销会少很多，所以binarySearch最好做法通过循环实现。

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

<ul>
<li>快排</li>
</ul>

** 快排--递归版本 **
~~~go
package main

import "fmt"

func main() {

	nums := []int{5, 7, 6, 4, 9, 8, 10}

	quickSort(nums, 0, len(nums)-1)
	fmt.Println("nums2:", nums)

}

//快排--递归版本
func quickSort(nums []int, left, right int) {

	//跳出递归
	if left >= right {
		return
	}
	pivotIdx := partition(nums, left, right)
	quickSort(nums, left, pivotIdx-1)  //左边递归
	quickSort(nums, pivotIdx+1, right) //右边递归
}

func partition(nums []int, left, right int) int {

	//以nums[left]为中轴
	pivotVal := nums[left]
	pivotIdx := left
	for left < right {

		//从右边开始，当小于pivotVal时 跳出，等待下面找个大的交换
		for nums[right] > pivotVal && left < right {
			right--
		}
		for nums[left] <= pivotVal && left < right {
			left++
		}
		fmt.Println(" nums:", nums, "left:", left, "right:", right)
		nums[left], nums[right] = nums[right], nums[left]
		fmt.Println(" nums:", nums)

	}
	fmt.Println("mid-pivotIdx:", pivotIdx)
	//把left新的中间  放在中间
	nums[left], nums[pivotIdx] = nums[pivotIdx], nums[left]
	return left
}

~~~





> reference:
	
* [csdn](https://blog.csdn.net/qhrqhrqhr/article/details/50975717)
* [csdn](https://github.com/bigbignerd/basicAlgorithm)



