# 数组

## 1. 数组的创建

- **不完全初始化**

  ```c
  #include <stdio.h>
  int main()
  {
      int arr[10]={1,2,3};//剩下的元素默认初始化为0
      return 0;
  }
  ```

  ```c
  #include <stdio.h>
  int main()
  {
      char arr2[5]={'a','b'};//初始化了两个元素
      char arr3[5]="ab";//初始化了三个元素
      return 0;
  }
  ```

- **完全初始化**

- **二维数组的初始化：**

  ```c
  #define _CRT_SECURE_NO_WARNINGS
  #include <stdio.h>
  
  int main()
  {
  	int arr[2][3] = { {1,2,3},{4,5,6} };
  	int arr1[][3] = { {1,2,3},{4,5,6} };
  	return 0;
  }
  ```

## 2.数组在内存中的表现

**数组的每个元素在内存中是连续的，二维数组按行连续**

## 3. 数组名的意义

**一般情况下，数组名是数组的首地址**

**以下两种情况为例外：**

- **sizeof(arr)**

- **&arr**

  **第二个代表了取出整个数组的地址，这里可以由&arr+1看出，不同于arr+1, 前者加一的跨度是整个数组的长度，后者的跨度只有一个元素的长度**

## 4. 冒泡排序的实例

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
void swap(int * pa, int *pb)
{
	int temp = *pa;
	*pa = *pb;
	*pb = temp;
}
void bubble_sort(int arr[], int arr_len)
{
	for (int i = 0; i < arr_len - 1; i++)
	{
		int flag = 1;
		for (int j = 0; j < arr_len - 1 - i; j++)
		{
			if (arr[j] > arr[j + 1])
			{
				swap(&arr[j], &arr[j + 1]);
				flag = 0;
			}
		}
		if (flag == 1)
		{
			break;
		}
	}
}
int main()
{
	int arr[] = { 1,2,3,4,5,6,7,8,11 };
	int	arr_len = sizeof(arr) / sizeof(arr[0]);
	bubble_sort(arr, arr_len);
	for (int i = 0; i < arr_len; i++)
	{
		printf("%d ", arr[i]);
	}
	return 0;
}
```

**当基础的冒泡排序实现后，会存在一个问题，就是这个基础的算法太“老实”了，即便原给定数据顺序正确，它还要进行一一比对，这个是比较没有效率的，因此为了改进，这里加了一个flag来判定冒泡排序是否已经是所需要的顺序。**