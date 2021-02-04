# 指针详解

## 1. 字符指针

```c
int main()
{
    char arr[]="abcdef";
    char *p = arr;
    printf("%s\n",arr);
    printf("%s\n",p);
    return 0;
}//等价
```

```c
int main()
{
    char *p ="abcdefg";//常量字符串
    printf("%s\n",*p);
    return 0;
}
```

错误代码：

```c
int main()
{
    char *p = "abcdefg";
    *p='w';
    printf("%s\n",*p)
    return 0;
}
```

```c
int main()
{
    char arr1[]="abcd";
    char arr2[]="abcd";
    const char *p1 = "abcd";//这样写代码更加标准
    const char *p2 = "abcd";//这样写代码也更具有保护性，使得代码更加健壮
    printf("%d\n",arr1==arr2);
    printf("%d\n",p1==p2);
}
```

**为什么输出 0 和 1 呢？**

原因是常量字符串内存没必要存两份

## 2. 数组指针

不同于指针数组，数组指针的本质是指针；

```c
#include <stdio.h>
int main()
{
    int *p = NULL; //指向整型的指针,可以存放整型的地址
	char *pc = NULL;//指向字符的指针，可以存放字符的地址
    int arr[10] = {1,2,3,4,5,6,7,8,9,10};
    //arr-首元素地址
    //&arr[0] - 首元素地址
    //&arr-数组的地址
    int(*p)[10]= &arr;//指向数组的指针，可以存放数组的地址，注意这里的*不是解引用操作

    return 0;
}
```

存放指针数组的数组指针：

```c
int main()
{
    char * arr[5];
    char * (*pa)[5] = &arr;//这是一个存放指针数组的数组指针
    return 0;
}
```

基本用法：

```c
#include <stdio.h>

void print1(int arr[3][5], int x, int y)
{
	int i = 0;
	int j = 0;
	for (i = 0; i < x; i++)
	{
		for (j = 0; j < y; j++)
		{
			printf("%d\t", arr[i][j]);
		}
		printf("\n");
	}
}

void print2(int (*parr)[],int x,int y)
{
    int i = 0;
    for(i = 0 ; i < x; i++)
    {
        int j = 0;
        for (j = 0; j< y; j++)
        {
            printf("%d\t",*(*(parr+i)+j));
        }
    }
}
int main()
{
	int arr[10] = { 1,2,3,4,5,6,7,8,9,10 };
	int(*parr)[10] = &arr;
	/*用法1*/
	int i = 0;
	for (i = 0; i < 10; i++)
	{
		printf("%d\t", (*parr)[i]);
	}
	printf("\n");
	/*用法2*/
	for (i = 0; i < 10; i++)
	{
		printf("%d\t", *(*parr + i));
	}
	printf("\n");
	//这前两种用法都很别扭，很啰嗦
	/*真正用法:二维数组*/
	int arr_real[3][5] = { {1,2,3,4,5},{2,3,4,5,6},{3,4,5,6,7} };
	print1(arr_real, 3, 5);
	print2(arr_real, 3, 5);
    return 0;
}
```

```c
int arr[5];//一个长度为5的数组，每个元素的类型时int
int *parr[10];//一个指针数组，本质上是数组，每个元素的类型时int*存放的是指针
int (*parr2)[10];//一个数组指针，本质上是指针，是一个指向一整个长度为10的数组的指针
int (*parr3[10])[5];//一个存放数组指针的数组，这个数组有10个元素，每个元素存放一个数组指针，每个数组指针指向一个长度为5的数组
```



## 3. 指针数组

主体是数组，是用来存放指针的**数组**；

**Example 1:**

```c
#include <stdio.h>
int main()
{
    int a = 1;
    int b = 2;
    int c = 3;
    int d = 4;
    int * arr[] = {&a,&b,&c,&d};
    return 0;
}
```

**Example 2:**

```c
#include <stdio.h>
int main()
{
    int arr1[4] = {1,2,3,4};
    int arr2[4] = {5,6,7,8};
    int arr3[4] = {8,9,10,11};
    int * parr[] = {&arr1,&arr2,&arr3};
    int i = 0;
    for (i = 0; i< 3; i++)
    {
        int j = 0;
        for (j = 0 ; j < 5; j++)
        {
            printf("%d\n",*(parr[i]+j));
        }
    }
    return 0;
}
```



## 4. 数组传参和指针传参

### 一维数组传参

```c
#include <stdio.h>
void test(int arr[])//ok
{}
void test(int arr[10])//ok
{}
void test(int * arr)//ok
{}
void test2(int *arr[20])//ok
{}
void test2(int **arr)//ok,指针数组传参方式
{}
int main()
{
    int arr[10]={0};
    int *arr2[20]={0};
    test(arr);
    test2(arr2);
}
```

### 二维数组传参

```c
#include <stdio.h>
void test(int arr[3][5])//ok
{
}
void test2(int arr[][5])//ok
{
}
void test3(int(*p)[5])//ok
{
}
void test4(int **arr)//错误
{   
}
void test5(int *arr)//错误
{
}
void test6(int *arr[5])//错误
{
}
int main()
{
	int arr[3][5] = { 0 };
	test(arr);
	test2(arr);
    test3(arr);
    test4(arr);
    test5(arr);
    test6(arr);
	return 0;
}
```

```c
void test(char **p)//什么情况可以这么传参？
{
    
}
int main()
{
    int *ptr;
    int **pp = &ptr;
    test(&ptr);//1
    test(pp);//2
    int *arr[10];
    test(arr);//3,指针数组传参
    return 0;
}
```



## 5. 函数指针

 ```c
#include <stdio.h>
int add(int a, int b)
{
    int z = 0;
    z = x+y;
    return z;
}
int main()
{
    int a = 10;
    int b = 20;
    printf("%p\n",add);
    printf("%p\n",&add);
    return 0;
}
 ```

&add和add都是函数的地址，这点与数组的首元素定义不同。

### 如何存储函数地址

```c
#include <stdio.h>
int add(int a, int b)
{
    int z = 0;
    z = x+y;
    return z;
}
int main()
{
    int a = 10;
    int b = 20;
    printf("%p\n",add);
    printf("%p\n",&add);
    int (*pa)(int, int) = add;
    // int (*pa)(int a, int b) = add; //这个也行
    int ret = (*pa)(2,3);
    printf("%d\n",ret);
    return 0;
}
```

```c
void print(char *str)
{
    printf("%s\n",str);
}
int main()
{
    void (*p)(char *)=print;
    (*p)("this is ok");
    return 0;
}
```

```c
(*(void(*)())0)()//函数指针类型直接强制转换0为一个函数指针，对函数指针解引用得到一个函数，调用这个函数，该函数没有参数
void (* signal(int,void(*)(int)))(int)//signal返回一个函数指针类型。void(*signal...)(int)指的是一个函数指针类型。对于signal,它是一个拥有两个参数的函数，第二个参数是一个函数指针类型，该函数返回一个整型数据。
//本质上这样等同于
typedef void (* func_p)(int);
func_p signal(int, func_p);
```

```c
int main()
{
    int a = 10;
    int b = 20;
    int (*pa) (int , int )=add;
    printf("%d\n",(pa)(2,3));
    printf("%d\n",pa(2,3));
    printf("%d\n",(*pa)(2,3));
    printf("%d\n",(**pa)(2,3));
    printf("%d\n",(***pa)(2,3));
    return 0;
}//四种解引用的方式都可以，这里的*就是个摆设，函数名就是函数指针。
```



## 6. 函数指针数组

**int (*parr[10])(int x, int y)**

```c
#include <stdio.h>
int add(int x, int y)
{
    return x+y;
}
int sub(int x, int y)
{
    return x-y;
}
int mul(int x, int y)
{
    return x*y;
}
int Div(int x, int y)
{
    return x/y;
}
int main()
{
	int * arr[4];
    int(*pa)(int,int)=add;
    int(*parr[4])(int,int)={add,sub,mul,Div};//函数指针数组
    int i = 0;
    for(i = 0; i< 4; i++)
    {
        printf("%d\n",parr[i](2,3));
    }
    return 0;
}

```

二维也是很可以的：

```c
#include <stdio.h>
void print1(int a)
{
	printf("1\t");
}
void print2(int a)
{
	printf("%d\t",a);
}
int main()
{
	void(*func_p[3][2])(int a) = { {print1,print2},{print1,print2},{print1,print2} };
	func_p[1][1] = print1;
	for (int i = 0; i < 3; i++)
	{
		for (int j = 0; j < 2; j++)
		{
			func_p[i][j](i + j);
		}
		printf("\n");
	}
	return 0;
}
```



## 7. 指向函数指针数组的指针



## 8.回调函数

回调函数就是通过函数指针调用的函数。

