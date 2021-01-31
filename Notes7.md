# VS调试技巧

## Debug 和 Release

Release 版本更小，更优化，是用于测试发布的

## 快捷键

### F5 启动调试/跳到执行逻辑上的断点，不是物理上的断点

### F9 设置或切换断点

### F10 逐过程/把函数调用等直接跳过，不进行细节查看

### F11 逐语句,比逐语句更细，能进到函数细节

### Shift+F11 跳出函数细节

### CTRL + F5 开始执行不调试

## 关于栈区

### 1. 栈区的默认使用

**先使用高地址处的空间**

**再使用低地址处的空间**

### 2. 数组内存使用

**数组随着下标的增长，**

**地址是由低到高变化的**

```c
int main()
{
    int i = 0;
    int arr[10]={1,2,3,4,5,6,7,8,9,10};
    for(i = 0;i<=12;i++)
    {
        printf("hehe\n");
        arr[i]=0;
    }
    system("pause");
    return 0;
}
```

**这里的错误是变量堆栈时，越界修改值，误改了在栈底的变量i**

**越界多少会死循环取决于编译器**

以上是在debug的情况

但在release的情况下，这个事情变得有趣起来，release可以优化内存结构，调换栈区中变量的先后位置。

## Assert

实现字符串拷贝

```c
#include<stdio.h>
#include<assert.h>
char* my_strcpy(char * dest, const char * src)
{
    char * ret = dest;
    assert(dest!=NULL);//断言
    assert(src!=NULL);//断言
    //把src指向的字符串拷贝到dest指向的空间，包含斜杆0字符
    while(*dest++=*src++)
    {
        ;
    }
    return ret;
}
int main()
{
    char arr1[]="##############";
    char arr2[]="bit";
    my_strcpy(arr1,arr2);
    printf("%s\n",arr1);
    return 0;
}
```

## const

```c
int main()
{
    const int num = 10;
    int *p = &num;
    *p= 20;
    printf("%d\n",num);
    return 0;
}
```

```c
int main()
{
    const int num = 10;
    const int *p = &num;
    *p= 20;//此时非法
    printf("%d\n",num);
    return 0;
}
```

```c
int main()
{
    const int num = 10;
    int * const p = &num;
    *p= 20;//此时非法
    printf("%d\n",num);
    return 0;
}
```

const 放在*左边时，修饰的是num(\*p),不能改变num的值

const 放在*右边时，修饰的是指针变量本身，不能改变指针，但可以改变值。

## coding技巧

- **使用assert**
- **尽量使用const**
- **养成良好的编码风格**
- **添加必要的注释**
- **避免编码陷阱**

```c
#include<stdio.h>
#include<assert.h>
/*重新实现my_strlen*/
int my_strlen(const char* arr)
{
    int count = 0;
    assert(arr!=NULL);
    while(*arr++!='\0')
    {
        count++;
    }
    return count;
}

int main()
{
    char arr[]="abcdefg";
    int len = my_strlen(arr);
    printf("%d\n",len);
    return 0;
}
```

## 编程常见错误

- **编译型错误：**语法错误等

  **双击错误即可搞定**

- **链接型错误：**缺少库函数等，无法解析的外部符号

  **直接查函数或变量名字即可**

- **运行时错误：** 其他错误

  **借助调试，逐步定位问题，最难搞**