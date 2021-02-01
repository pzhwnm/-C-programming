# 数据在内存中的存储

## 1. 整型数据

- 整型数据在内存中以补码形式存储

  原因：由于CPU只有加法器，便于加减法的统一，简化底层运算电路逻辑

## 2.大小端字节序

- 大端字节序：低地址存放高数位，高地址存放低数位
- 小端字节序：高地址存放高数位，低地址存放低数位

x86架构下，基本采用小端字节序。

写一段代码，判断当前机器的字节序：

```c
#include <stdio.h>
int pow(const int a, const int n)
{
    int i = 0;
    int ret=1;
    for(i = 0; i<n; i++)
    {
        ret*=a;
    }
    return ret;
}
void byte_order(const int example)
{
    int ret = 0;
    int i = 0;
   
    char * p = &example;
    for(i=0;i<4;i++)
    {
        ret+=(*p)*pow(pow(2,8),i);
        p++;
    }
    if (example == ret)
    {
        printf("小端字节序\n");
    }
    else
    {
        printf("大端字节序\n");
    }
    
}
int main()
{
    int a = 20;
    byte_order(a);
    return 0;
}
```

优化后：

```c
int check()
{
    int a = 1;
    return *((char*)&a);
}
int main()
{
    if(1==check())
    {
        printf("小端字节序\n");
    }
    else
    {
        printf("大端字节序\n");
    }
}
```

## 3. 浮点数

```c
#include <stdio.h>
int main()
{
    int n = 9;
    float *pFloat=(float *)&n;
    printf("n的值为: %d\n",n);
    printf("pFloat的值为:%f\n",*pFloat);
    *pFloat = 9.0;
    printf("n的值为: %d\n",n);
    printf("pFloat的值为:%f\n",*pFloat);
    return 0;
}
```

这个例子告诉我们，浮点型的存储和整型的存储方式完全不同。

根据国际标准IEEE 754标准，任意一个二进制浮点数V都可以表示成下面的形式：

- （-1)^S\*M\* 2^E

- （-1)^S表示符号位，S=0，V是正数，S=1，V是负数

-   M表示有效数字，大于等于1，小于2

-   2^E表示指数位

  对于32位浮点数，最高的1位是符号位S，接着8位是指数E，剩下的23位为有效数字M。

  M的有效位可以舍去第一位

  E如果8位+127  e.g. -1 + 127=126

  E如果11位 + 1023  e.g. -1 +1023=1022

**E不全为0和不为全1的情况**

如上

**E全为0**

实际的幂为-127，无限接近于0；此时浮点数1-127与双精度1-1023为真实值，有效数字M不再加上第一位的1，而是还原为0.xxxxxxxxx的小数，这样做是为了表示+-0，以及接近于0 的很小的数字。

**E为全1**

实际的幂为128，接近于无限大