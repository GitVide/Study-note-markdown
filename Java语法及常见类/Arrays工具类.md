# Arrays工具类

## 一、包

java.util.Arrays

## 二、静态方法

1）**public static String toString(int[] a)**

2）**public static void sort(int[] a)**

3）**public static int binarySearch(int[] a, int key)**

4)  **Arrays.copyOf(int[] original, int newLength)**

复制指定的数组---效率低，会重新开辟新的数组空间original - 要复制的数组;

newLength - 要返回的副本的长度。

```java
package array.util;

import java.util.Arrays;

public class Test {

	public static void main(String[] args) {
		int[] array= {1,5,2,6,3,9,50,21,63,54,11};
        //toString()
		System.out.println(Arrays.toString(array));
        //sort()
		Arrays.sort(array);
		System.out.println(Arrays.toString(array));
        //binarySearch()
		System.out.println(Arrays.binarySearch(array, 6));
	}
}

```



## 三、自定义类

自定义类如果要调用sort方法：

必须实现Comparable接口，实现其中的compreTo方法

