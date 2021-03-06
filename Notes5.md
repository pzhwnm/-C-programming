# 操作符

## 移位操作符>><<

- **左移操作符**

  **左移操作符只有一种方式：右边补0，左边丢弃**

- **右移操作符**

  1. **算术操作符**

     **左边补符号位，右边丢弃**

  2. **逻辑操作符**

     **左边补0，右边丢弃**

e.g. 求一个整数二进制中的1的个数：

**方法1：**

```C
#include <stdio.h>
int main()
{
	int a = 10;
	int count = 0;
	for (int i = 0; i < 32; i++)
	{
		if (1&(a=a >> 1))
			count++;
	}
	printf("%d\n", count);
	return 0;
}
```

**这种方案的缺点是仍然需要遍历32次，更有效率的做法如下：**

**方法2：**

```c
#include <stdio.h>
int main()
{
    int a = 10;
    int count = 0;
   	while(a)
    {
        a=a&(a-1);
        count++;
    }
    printf("%d\n",count);
}
```

**这种做法的效率高，遍历的次数等于二进制整数中1的个数。**

## 按位取反操作符~

```c
#include <stdio.h>
int main()
{
    int a = 11;
    a = a|(1<<2);
    printf("%d\n",a);
    a = a&(~(1<<2));
    printf("%d\n",a);
}
```

这里的编码解码，适用于将二进制位0变成1的情况。另外，这里也可以用异或修改具体的值，这样不局限于0，1的情况。



## 逗号表达式

逗号表达式从左向右依次计算，只取最后一个结果:

```c
while(a=get_val(),count_val(a),a>0)
{
    //内容
}
```

# 类型转换

## 隐式类型转换

### 1. 整型提升

C的整型算术运算总是至少以缺省整型类型的精度来进行的

为了获得这个精度，表达式中的字符和短整型操作数在使用之前被转换为普通整型，这种转换被称为**整型提升**。

e.g.

```c
#include <stdio.h>
int main()
{
    char a = 3;
    char b = 127;
    char c = a + b;
    printf("%d\n",c);
    return 0;
}//输出-126
```

这个案例里，a和b要先转换成int, 然后进行加法，赋值的时候截断精度，打印的时候又进行一次整型提升。

**注意，整型提升是按照变量数据类型的符号位来提升的。**比如, 11000001->11111111111111111111111111000001

### 整型提升的意义

表达式的整型运算要在CPU的相应运算器件内执行，CPU内整型运算器ALU的操作数的字节长度一般就是int的字节长度，同时，也是CPU的通用寄存器的长度

因此，即使两个char类型相加，在CPU执行时实际上也要先转换为CPU内整型操作数的标准长度。

通用CPU是难以实现两个8比特字节直接相加运算的，所以表达式中各种长度可能小于int长度的整型值，都必须先转int或者unsigned int然后才能送入CPU执行运算。

### 能够进行整型提升的表达式

```c
int main()
{
    char a = 129;
    if (a==129)//条件判断整型提升
    {
        printf("yes\n");
    }
    printf("%d\n",a);//打印整型整型提升
    printf("%d\n",sizeof(+a))//操作符整型提升
    return 0;
}
```

### 2. 算术转换

（略）



## 有问题的表达式

## 1. ab+cd+ef

不能确定ab,cd,ef那个先执行

## 2. c + --c

不能确定c是先给定，还是先进行自减

## 3. func()-func()*func()

不能确定哪个函数先调用

## 4. （++i)+(++i)+(++i)

不能确定唯一计算路径

