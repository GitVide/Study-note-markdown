# 二分查找（Binary Search）

## 一、算法

二分法检索（Binary Search）又称折半检索，其基本思想是设数组中的元素从小到大有序的存放在数组中，首先将给定值key与数组中间位置上元素的关键码（key）比较，如果相等，则检索成功；若key较小，则在数组前半部分继续进行二分法检索；若key大，则在数组后半部分中继续进行二分法检索。

这样，经过一次比较就缩小一半的检索区间，如此进行下去，直到检索成功或失败。

## 二、实现

```java
package binarySearch;

import java.util.Arrays;

public class BinarySearchTest {

	public static void main(String[] args) {
		int[] a = { 2, 5, 9, 1, 3, 4, 6, 10, 7, 8, 56, 100, 50, 23, 98, 500, 24, 13 };
		Arrays.sort(a);
		System.out.println(Arrays.toString(a));
		System.out.println(binarysearch(a, 13));
	}

	public static int binarysearch(int nums[], int value) {
		
		int low = 0;
		int high = nums.length;
		int mid;
		while (low <= high) {
			mid = (low + high) / 2;
			if (nums[mid] == value)
				return mid;
			if (nums[mid] > value)
				high = mid - 1;
			if (nums[mid] < value)
				low = mid + 1;
		}
		return -1;
	}

}

```

## 三、复杂度分析

最坏的情况是要查找的key在有序数组首位或者末尾的情况，这种情况下将花费logn的时间，

所以时间复杂度为O(logn)。

空间复杂度：只用了三个指针，空间复杂度为O(1)。