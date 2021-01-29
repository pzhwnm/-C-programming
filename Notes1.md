# **Notes.1**

### 1. C 语言的数据类型

- **char**                **sizeof(char)-->1**
- **short**              **sizeof(short)-->2**
- **int**                   **sizeof(int)-->4**
- **long**                **sizeof(long)-->4**
- **long long**       **sizeof(long long)-->8**                                       **--C99类型**
- **float**               **sizeof(float)-->4**
- **double**           **sizeof(double)-->8**

### 计算机单位

- **bit**
- **1 Byte = 8 bit**
- **1 KB = 1024 byte**
- **1 MB = 1024 KB**
- **GB = 1024 MB**
- **TB = 1024 GB**
- **PB = 1024 TB**

### Print数据格式

- **%d  -->打印十进制整形数据**
- **%c  -->打印字符格式数据**
- **%f   -->打印浮点数据** 
- **%p  -->打印地址**
- **%x  -->打印十六进制**
- **%o  -->打印八进制数据**
- **%lf  -->打印双精度浮点数据**

### Q: 为什么需要数据类型？

- **需要向内存申请空间**

### C标准类型宽度界定

- e.g. **sizeof(long)>=sizeof(int)**

### 输入输出函数

- **printf("%d",num)**

- **scanf("%d, %c", &num, &characters)**  - &: 取地址符号，将输入的数据放在num的地址处，将输入的数据放在character的地址处

  **注意：  scanf是C语言提供的，scanf_s不是C语言提供的，是VS编译器提供的。如果采用scanf_s，可能使得程序失去可移植性。 如果编译不通过，在源文件第一行加上：#define _CRT_SECURE_NO_WARNINGS 1**

## 2. **变量**

### 变量的分类

- **全局变量**：定义在代码块{}之外的变量

- **局部变量**：定义在代码块{}内部的变量

  **注释: 当全局变量和全局变量命名相同时，局部变量优先。**

  ​          **局部变量只能在局部变量所在代码块{}使用。**

e.g. 

```c
#include <stdio.h>
int a = 10;
int main()
{
    int a = 10;
    printf("a: %d\n",a);
    return 0;
}
```

### 变量的作用域与生命周期

- **作用域：变量哪里可以用，哪里就是作用域。**

  1. **局部变量作用域：**所在代码块内部。

  2. **全局变量作用域：**当前代码中的任意位置

     ​						       也可以是整个工程，如在另一个代码文件中定义的变量， 在当前代码文件中要用**extern 类型 变量名**申明，								e.g. extern int global;

- **生命周期： ** **全局变量**的生命周期是整个程序的生命周期，和main()的生命周期相同

  ​				      **局部变量**的生命周期是当前代码块的开始和结束

## 3. 常量

### 常量分类

- **字面常量**: 直接写出的值 e.g.  1,2,3,4,“a”,"b",...

- **const修饰的常变量**

  以下代码会报错：

  错误一

  ```c
  #include <stdio.h>
  int main()
  {
      const int num = 4;
      printf("%d\n",num);
      num = 8;
      printf("%d\n",num);
      return 0;
  }
  ```

  错误二

  ```c
  #include <stdio.h>
  int main()
  {
  	const int n = 10;
      int arr[n] = {0};
      return 0;
  }
  ```

  对于错误二，n是具有常属性的变量，依旧是变量，不能用在定长数组中。

- **#define 定义的标识符常量**

  ```c
  #include <stdio.h>
  #define MAX 10
  int main()
  {
      int arr[MAX] = {0};
      return 0;
  }
  ```

  以上代码可以编译通过。

- **枚举常量**

  名词解释--枚举：可以一一列举的值

  **枚举关键字：enum**

  ```c
  #include <stdio.h>
  enum Sex
  {
      MALE,//枚举常量
      FEMALE,//枚举常量
      SECRET//枚举常量
  };
  int main()
  {
      enum Sex s = MALE;
      printf("%d\n",MALE);
      printf("%d\n",FEMALE);
      printf("%d\n",SECRET);
  	return 0;
  }
  ```

  枚举常量(MALE,FEMALE,SECRET)是不能改的，但是由枚举关键字enum创建的变量(s)是可以改的。

  

## 4. 字符串

### 概念：由双引号括起来的字符

```c
#include <stdio.h>
int main()
{
    char arr1[] = "abc";//方式1：隐藏了"\0"(\0的值是0)
    char arr2[] = {'a','b','c'};//方式2:需要加0才正常
    printf("%s\n",arr1);//打印字符串用的是%s
    printf("%s\n",arr2);
    return 0;
}
```

上述代码中， arr2有乱码，原因是最后没加零。

### ASCII编码

每个字符有分别对应的值。

e.g. a-97;A-65

### strlen() --计算字符串长度

```c
#include <stdio.h>
int main()
{
    char arr1[] = "abc";
    char arr2[] = {'a','b','c'};
    printf("%d\n",strlen(arr1));
    printf("%d\n",strlen(arr2));
    return 0;
}
```

strlen()不计算\0，如果没有\0，如arr2里计算长度，要找随机的一个\0，所以arr2的长度是个随机值。

### 转义字符

e.g. **\n \t \a** 

e.g.

```c
int main()
{
    printf("c:\test\32\test.c");//这个不行，是转义字符的原因
    printf("\n")
    printf("c:\\test\\32\\test.c");//这个可以，相当于转义\
    return 0;
}
```

在一些老的编译器上，甚至有可能出现三字母词的异常解析。

比如：**（are you ok??)\n** 其中，**??+)**是个**三字母词**，被解析成**]**

**为了防止这种三字母词的解析，在转义字符里才存在\?**

\ddd 表示八进制数 e.g. \32 这个转义字符的含义是，将32这个八进制数转换为十进制数26后，对应的26的字符'->'

\xdd 表示十六进制数 e.g. \x61 = (d) 97 = ‘a’

### 注释

- **C风格**      /*               */  

  缺陷：不能嵌套 

- **C++风格** //

## 5. 结构语句

### 选择语句

```c
#include <stdio.h>
int main()
{
    int input = 0;
    printf("你要好好学习吗？(0/1)>");
    scanf("%d",&input);
    if(input == 1){
        printf("给你offer\n");
    }
    else
    {
     	printf("卖红薯\n");   
    }
}
```

### 循环语句

- while循环
- for循环
- do while循环

**e.g. 以while为例：**

```c
#include <stdio.h>
int main()
{
    int line = 0;
    while(line<20000)
    {
    line++;
    printf("敲一行代码,敲到第%d行\n",line);
    }
    printf("共有%d行",line);
    printf("可以有offer了\n");
    return 0;
}
```

## 6. 函数

- **自定义函数**
- **库函数**

```c
#include <stdio.h>
Add(int x, int y)
{
    int z = x + y;
    return z;
}
int main()
{
    int a = 10;
    int b = 33;
    int sum = 0;
    sum = Add(a,b);
    printf("a + b = %d", sum);
    return 0;
}
```

## 7. 数组

```c
#include <stdio.h>
int main()
{
    int i = 0;
    int arr[10] = {10,2,3,4,5,6,7,8,9,10};
    arr[0] = 1;
    while(i<10)
    {
    	printf("i = %d, arr[%d] = %d\n",i,i, arr[i]);
        i++;
    }
    return 0;
}
```

## 8. 操作符

- **算术操作符**

  **"+";	"-";	"*";	“/";	“%”；**

- **移位操作符**

  **“>>”;	"<<";**

- **位操作符**

  **"&";	"^";	"|";**

- **赋值操作符**

  **“=”；	“+=”；	”-=“；	”*=“；	”/=“;	"&=";	"^=";	"|=";	">>=";	"<<=";**

- **单目操作符**

  **“！”；	“-”；	“+”；	“&”；	“sizeof”；	“~”[按位取反]；	“--”；	“++”；	“*”[间接访问或解除引用操作符]；	“（类型）”[强制类型转换]；**	

```c
#include <stdio.h>
int main()
{
   int a = 1;
   a <<= 1;
   printf("%d",a);
   return 0;
}
```

- **原码,反码,补码**

  负数在内存中存储的是二进制的补码；

  使用的，打印的是这个数的原码

  补码-1=反码

  反码除了符号位按位取反

```c
#include <stdio.h>
int main()
{
	int a = 0;
    int b = ~a;
    printf("%d\n",b);//由于0按位取反全是1，内存里是补码，还原成原码需要-1取反。
    return 0;
}
```

- **前置和后置++**

  e.g. **a++ 后置：先使用再++**

  ```c
  #include <stdio.h>
  int main()
  {
      int a = 10;
      int b = a++;
      printf("a=%d,b=%d",a,b);//a=11,b=10
      return 0;
  }
  ```


## 9. 常见关键字

- **auto**

  ```c
  #include <stdio.h>
  int main()
  {
      int a = 10;//局部变量都是自动变量，所以auto都省略了
      return 0;
  }
  ```

- **break**

- **case**

- **char**

- **const**

- **continue**

- **default**

- **do double**

- **else**

- **enum**

- **extern**

- **float**

- **for**

- **goto**

- **if**

- **int**

- **long**

- **register[寄存器关键字]**

  计算机存储数据[按速度(反向)和空间(正向)排序]：

    1. 硬盘

    2. 内存

    3. 高速缓存「内存和寄存器之间」

    4. 寄存器

       由于在硬件发展中，内存的访问速度逐渐跟不上CPU访问速度，所以需要3，4； 现在cpu先从寄存器取数据

       ```c
       #include <stdio.h>
       int main()
       {
       	register int a = 10;//建议把a定义成寄存器变量，但是未必能成功，取决于编译器。
       	return 0;
       }
       ```

- **return**

- **short**

- **signed**

- **unsigned**

- **sizeof**

- **static**         

  1. [static修饰局部变量，变量在其生命周期不销毁，但并没有改变它的作用范围，并不意味着变成全局变量]
  2. [static修饰全局变量，改变的是变量的作用域，变量出了源文件就失效]
  3. [static修饰函数改变了函数的链接属性，外部链接属性->内部链接属性，类似全局变量改变作用域]

  ```c
  #include <stdio.h>
  void test()
  {
      int a = 1;
      a++;
      printf("a=%d\n",a);
  }
  int main()
  {
      int i = 0;
      while (i<5)
      {
          test();
          i++;
      }
      return 0;
  }//上述代码输出5个2
  ```

  当采用static后：

  ```c
  #include <stdio.h>
  void test()
  {
      static int a = 1;//改为静态变量
      a++;
      printf("a=%d\n",a);
  }
  int main()
  {
      int i = 0;
      while (i<5)
      {
          test();
          i++;
      }
      return 0;
  }//上述代码输出2,3,4,5,6
  ```

  当static修饰全局变量时：

  ​	源文件1:

  ```c
  int g_val = 2020;
  ```

  ​	源文件2:

  ```c
  #include <stdio.h>
  int main()
  {
      extern int g_val;
      printf("%d\n",g_val);
      return 0;
  }//这种情况会报错。
  ```

  

- **struct[结构体关键字]**

- **swich**

- **typedef[类型重定义]**

  ```c
  #include <stdio.h>
  int main()
  {
  	typedef unsigned int u_int;//其实是用于给变量起别名
      unsigned int num = 20;
      u_int num2 = 20;
      return 0;
  }
  ```

- **union[共用体]**

- **void**

- **volatile[体现C语言段位的关键字]**

- **while**

## #10. define定义的常量和宏

1. 定义标识符常量

```c
#include <stdio.h>
#define MAX 100
int main()
{
    int a = MAX;
    retrun 0;
}
```

2. 定义宏，宏是带参数的标识符常量

```c
#include <stdio.h>
#define MAX(X,Y) (X>Y?X:Y)
int main()
{
    int a = 10;
    int b = 20;
    int max = MAX(a,b);
    printf("max = %d\n",max);
    return 0;
}
```

## 11. 指针

### 1. 如何产生地址/如何编地址

**32位：**32根地址线/数据线，能产生$2^{32}$个地址。e.g. 一个地址索引到的内存单元，如果是1个字节，那么内存是4G. 所以32位系统的寻址能力最高4G，在8G内存上装32位系统是对内存空间的浪费。

**64位：**64根地址线，现实应用中只有40根，即支持内存的上限是1T。寻址能力理论最高1T，但是实际上由于别的种种原因，目前Windows 7 64位版最大仅能使用192GB内存，Windows 8 64位版最大仅能使用512GB内存。不过尽管系统所能支持的内存有这么大，但主板和CPU的限制导致一般的电脑所支持的内存最大只有16GB而已。

```c
#include <stdio.h>
int main()
{
	int a = 10;
	int* p = &a;//int* 是指针的类型 指针类型可以是char* int* double*等。
	printf("%p\n", &a);
	printf("%p\n", p);//打印地址用%p
	*p = 20;//解引用操作
	printf("%d\n", a);
	return 0;
}
```

### 2. 指针变量的大小

```c
#include <stdio.h>
int main()
{
    printf("%d\n",sizeof(char *));
    printf("%d\n",sizeof(short *));
    printf("%d\n",sizeof(int *));
   	printf("%d\n",sizeof(double *));
    return 0;
}//输出的结果都一样，都是地址的字节长度，该长度取决于机器是64位还是32位。如果64位的话，长度应该是8字节（尽管实际应用上看只有5个字节被利用），32位长度应该是4字节
```

注：已知地址直接取值的方法是极其危险的。

### 3.例子

```c
#include <stdio.h>
int main()
{
    double d = 3.14;
    double* pd = &d;
    printf("%lf\n",d);
    printf("%d\n",pd);
    return 0;
}
```

## 12. 结构体

结构体用于描述复杂对象，是自己创造出来的一种类型。

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>

struct Book
{
	char name[20];
	short price;
};//封号不可缺少，是结束定义的标志
int main()
{
	struct Book b1 = { "C语言程序设计",55 };
	printf("书名 = %s\n", b1.name);
	printf("价格 = %d\n", b1.price);
	b1.price = 17;
	printf("改后价格 = %d\n", b1.price);
	//b1.name="c++";这样改不行，因为name本质上是一个地址，要改的话可以这样:
	strcpy(b1.name, "C++");
	printf("改后书名: %s\n", b1.name);

	struct Book * pb = &b1;//结构体的指针格式
	printf("书名 = %s\n", (*pb).name);
	printf("价格 = %d\n", (*pb).price);//太罗嗦，需要新的操作符

	printf("%s\n", pb->name);
	printf("%d\n", pb->price);
	return 0;
}
```

**“.”**	点操作符用于**结构体变量**访问结构体成员

**->**	箭头操作符用于**结构体指针**访问结构体成员

