# Notes. 3

## 函数

### 1. 函数是什么？

- **维基百科定义：**函数就是一段子程序。它由一个或多个语句块组成，负责完成某项特定任务，相较于其他代码**，具备相对的独立性。**
- **C语言的函数分类：**
  1. **自定义函数**
  2. **库函数**

### 2. 库函数

- **为什么会有库函数？**

  重要参考网址：

  ​	- www.cplusplus.com

  ​	- www.cppreference.com

- **分类**

  1. **IO函数**
  2. **字符串操作函数**
  3. **字符操作函数**
  4. **内存操作函数**
  5. **时间/日期函数**
  6. **数学函数**
  7. **其他库函数**

  ```c
  #include <stdio.h>
  #include <string.h>
  int main()
  {
      char arr[]="password";
      memset(arr,"*",strlen(arr));
      printf("%s\n",arr);
      return 0;
  }
  ```

  

### 3. 自定义函数

```c
#include <stdio.h>
int max(int a, int b)
{
    if (a>b)
    {
        return a;
    }
    else
    {
        return b;
    }
}
int main()
{
    int a = 2;
    int b = 3;
    printf("max=%d\n",max(a,b));
    return 0;
}
```

函数失效的一个例子：

```c
#include <stdio.h>
void swap(int* x, int* y)
{
    
    int temp;
    temp = x;
    x=y;
    y=temp;
}
int main()
{
    int a = 10;
    int b = 20;
    swap(a,b);
	printf("a=%d,b=%d\n", a, b);
    return 0;
}//输出a=10,b=20
```

改后：

```c
#include <stdio.h>
void swap(int* x, int* y)
{
	int temp;
	temp = *x;
	*x = *y;
	*y = temp;
}
int main()
{
	int a = 10;
	int b = 20;
	swap(&a, &b);
	printf("a=%d,b=%d\n", a, b);
	return 0;
}//输出a=20,b=10
```



### 4.函数参数

- **形式参数**

  函数名后括号里的参数，在函数调用过程中才实例化。

  如果没有调用函数，形式参数在内存中不存在。

- **实际参数**

  真实传递给函数的参数

  可以是常量，变量，函数，表达式等。

**注意：当实际参数传给形参时，形参其实是实参的一份临时拷贝，对形参的修改是不会影响实参的。**

### 5. 函数调用

- **传值调用**

  **当实际参数传给形参时，形参其实是实参的一份临时拷贝，对形参的修改是不会影响实参的。**

- **传址调用**

  **把函数外部创建变量的内存地址传递给参数的一种调用函数的方式，这种方式可以让函数和函数外边的变量建立起真正的联系，也就是函数内部可以直接操作函数外部的变量。**

### 6. 函数的嵌套调用和链式访问

- **嵌套调用**      **——函数之间相互调用**
- **链式访问      ——把一个函数的返回值作为另一个函数的参数**

```c
#include <stdio.h>
int main()
{
	printf("%d", printf("%d", printf("%d", 43)));
	return 0;
}//答案是4321，原因是printf的返回值是打印的字符数，第一个printf返回2，因为两个字符‘4’，‘3’；第二个printf返回1，因为一个字符‘2’，最后返回1，因为打印1个字符‘2’
```



### 7. 函数的声明和定义

**一般来说函数的声明放在头文件里，函数的实现放在源文件，即.c文件里**

**引用自己写的头文件用双引号#include “xxx.h”**

```c
//头文件申明格式
#ifndefine __ADD_H__
#define __ADD_H__
int Add(int x,int y);
#endif
```



### 8. 函数递归

- **定义：程序调用自身的编程技巧**

- **可能存在问题：可能存在栈溢出的问题**
- **递归的两个必要条件：**
  	1. **存在限制条件，当满足这个限制条件的时候，递归便不再继续**
   	2. **每次递归调用之后越来越接近这个限制条件**

![](C:\Users\panziheng\Downloads\a1.png)

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
void one_by_one(int num) 
{
	if (num > 9)
	{
		one_by_one(num/10);
	}
	printf("%d\n", num % 10);
}
int main()
{
	unsigned int num;
	scanf("%d", &num);
	one_by_one(num);
	return 0;
}
```

**e.g. 不创建临时变量的情况下，写一个测量字符串长度的函数：**

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
int length_str(char * str)
{
	if (*str != '\0') 
	{
		str++;
		return 1+length_str(str);
	} 
	else
	{
		return 0;
	}
}
int main()
{
	int result = 0;
	char arr[] = "abcde";
	result = length_str(arr);
	printf("%d\n", result);
	return 0;
}
```

