# 指针

指针是存地址的变量

**指针在32位平台上是4个字节，在64位平台是8个字节**

## 1.指针类型有什么用？

```c
#include <stdio.h>
int main()
{
    int a = 10;
    char* pa = &a;
    *pa = 0;
    return 0;
}
```

 这个代码的运行结果是令人惊讶的，当我们采用*pa的时候，我们仅仅改变了a的第一个字节，将前两个字节变成0。后面的字节不动。

**指针类型决定了指针进行解引用操作时，能访问的空间的大小**

```c
#include <stdio.h>
int main()
{
    int a = 0x11223344;
    int* pa = &a;
    char* pc = &a;
    printf("%p\n",pa);
    printf("%p\n",pa+1);//比前面多了4个字节
    printf("%p\n",pc);
    printf("%p\n",pc+1);//比前面多了1个字节
    return 0;
}
```

**指针类型决定了指针走一步的步长**

## 2. 野指针

### 局部变量不初始化，默认随机值

```c
#include <stdio.h>
int main()
{
    int * p;
    *p=20;
    return 0;
}
```

### 指针越界访问

### 指针指向的内存空间释放

```c
#include <stdio.h>
int * test()
{
    int a = 10;
    return &a;
}
int main()
{
    int *p=test();
    (*p)++;
    printf("%d\n",*p);
    return 0;
}
```

这个也是野指针，因为局部变量在函数结束后就释放了，虽然传回了指针，但是这个指针不是之前与变量挂钩的那个指针了。

## 3. 如何规避野指针

### - 指针初始化

### - 小心指针越界

### -  指针指向空间释放即设置NULL

### - 指针使用之前检查有效性

### - 在不知道指针变量要初始化什么值时，需要置NULL

## 4. 指针运算

### 指针+-整数

### 指针+-指针（元素个数）

```c
#include <stdio.h>
int my_strlen(char *ap)
{
	char* start = ap;
	char* end = ap;
	while (*end != '\0')
	{
		end++;
	}
	return end - start;
}
int main()
{
	char arr[] = "bit";
	int len = my_strlen(arr);
	printf("%d\n", len);
}
```

### 指针的关系运算

```c
#include <stdio.h>
int main()
{
	int arr[10] = { 1,2,3,4,5,6,7,8,9,10 };
	int sz = sizeof(arr) / sizeof(arr[0]);
	for (int *p = &arr[0]; p <=&arr[sz - 1];)
	{
		*p++ = 0;	
	}
	printf("%d\n", arr[5]);
	return 0;
}
```

**最好从前往后遍历，这样符合C的标准；**

**标准规定，允许指向数组元素的指针与指向数组最后一个元素后面的那个内存位置的指针比较，但是不允许与指向第一个元素之前的那个内存位置的指针进行比较。**

**数组名：**

**sizeof(arr)和&arr不代表首元素地址**