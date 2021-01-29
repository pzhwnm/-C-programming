# Notes. 2

## 分支和循环语句

C语言是一门结构化的程序设计语言,那什么地方体现了结构化呢？

- **顺序结构**
- **选择结构**
- **循环结构**

### 分支语句

- **if**

```c
#include <stdio.h>
int main()
{
    int age = 10;
    if(age < 18)
        printf("未成年\n");
    else if(age >= 18 && age <28)
        printf("青年\n");
    else if(age >= 28 && age <60)
        printf("中年人\n");
    else
        printf("老年人\n");
    return 0;
}
```

**注意：当if或者else下有两条及以上的语句，要使用代码块{}，否则会报错；还有一点，else 只匹配最近的if**

**bug 提醒**

```c
#include <stdio.h>
int main()
{
    int num = 4;
    if(num = 5)
        printf("呵呵");//这里是可以输出的，因为条件语句中的条件表达式可以是赋值语句。
    return 0;
}
```

**防止bug的代码习惯可以如下：**

```c
#include <stdio.h>
int main()
{
    int num = 4;
    if(5 == num)//将数字放在前面，变量放在后面
        printf("呵呵");
    return 0;
}
```

- **switch**

  switch里面如果没有break，就会运行一组的case。实际上break是用来划分不同的case的。比如case 1,2,3,4,5为一组，6,7另一组。

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
int main()
{
	int day = 0;
	int n = 1;
	scanf("%d", &day);
	switch (day)
	{
		case 1:
		case 2:
		case 3:
		case 4:
		case 5:
			printf("工作日\n");
			break;
		case 6:
		case 7:
			printf("工作日\n");
			break;
		default:
			printf("输入错误\n");
			break;
	}
	return 0;
```

​		switch语言允许嵌套

```c
#include <stdio.h>
int main()
{
    int n = 1;
    int m = 2;
    switch(n)
    {
    	case 1:
            m++;
        case 2:
            n++;
        case 3:
            switch(n)//嵌套switch
            {
                case 1:
                    n++;
                case 2:
                    m++;
                    n++;
                    break;
            }
        case 4:
            m++;
            break;
        default:
            break;
    }
    printf("m = %d, n = %d\n",m,n);
    return 0;
}
```

### 循环语句

- **while**

  ```c
  #include <stdio.h>
  int main()
  {
      int i = 1;
      while(i<=10)
      {
          if(i==5)
          {
              continue;
          }
          printf("%d",i);
          i++
      }
      return 0;
  }//执行结果是1,2,3,4而不是1,2,3,4,6,7,8,9,10，会死循环，i++放在前面能解决这个东西。阅读程序马虎不得。
  ```

  一个例子：

  ```c
  #define _CRT_SECURE_NO_WARNINGS
  #include <stdio.h>
  int main()
  {
  	int ch = 0;
  	while ((ch=getchar())!= EOF)
  	{
  		putchar(ch);
  	}
  	return 0;
  }
  ```

  **另一个带bug的例子(作为上述例子的一个应用场景)：**

  ```c
  #define _CRT_SECURE_NO_WARNINGS
  #include <stdio.h>
  int main()
  {
  	char ret = 0;
  	char password[20] = { 0 };
  	printf("please enter your password:>");
  	scanf("%s", password);
  	printf("please confirm your submit.");
  	printf("(Y/N)");
  	ret = getchar();
  	if (ret == 'Y')
  		printf("confirmation completed!");
  	else
  		printf("confrimation failed!");
  	return 0;
  }
  ```

  **这里的bug原因在于getchar()和scanf()均为输入函数，每个输入函数都是从输入缓冲区里读东西的，输入缓冲区里的东西, scanf会拿走除了\n的其他字符，然后 getchar 从缓冲区里读\n. 输入函数并不直接从键盘读取字符，而是直接从缓冲区内读字符。**

  **按以下修改bug后依旧不行：**

  ```c
  #define _CRT_SECURE_NO_WARNINGS
  #include <stdio.h>
  int main()
  {
  	char ret = 0;
  	char password[20] = { 0 };
  	printf("please enter your password:>");
  	scanf("%s", password);
      getchar();//加个getchar,这样就ok了吗
  	printf("please confirm your submit.");
  	printf("(Y/N)");
  	ret = getchar();
  	if (ret == 'Y')
  		printf("confirmation completed!");
  	else
  		printf("confrimation failed!");
  	return 0;
  }
  ```

  **出了什么问题呢，问题在于当我们输入“123456     abcd”时程序依旧有问题，因为scanf会先读取123456，剩下abcd给getchar，第一个getchar就读取了一个空格然后走人，第二个getchar(ret的)读走a就走人。**

  **那怎么办？**

  **我们可以在ret的那个getchar操作前不断的读取字符将输入缓冲区清空。可以按照如下解决方案：**

  ```c
  #define _CRT_SECURE_NO_WARNINGS
  #include <stdio.h>
  int main()
  {
  	char ret = 0;
  	char password[20] = { 0 };
  	printf("please enter your password:>");
  	scanf("%s", password);
      while((ch=getchar())!='\n')
      {
          ;
      }//用这个解决方案是ok的
  	printf("please confirm your submit.");
  	printf("(Y/N)");
  	ret = getchar();
  	if (ret == 'Y')
  		printf("confirmation completed!");
  	else
  		printf("confrimation failed!");
  	return 0;
  }
  ```

  

- **for**

  ```c
  #include <stdio.h>
  int main()
  {
      int i = 0;
      for(i=1;i<=10;i++)
      {
          if (i==5)
          {
              continue;
          }
          printf("%d",i);
      }
      return 0;
  }//输出12346789
  ```

  **与while的第一个例子不同，for此处用continue不会死循环。**

  **一些其他规范：**

  - **不可在for循环体内改变循环变量，防止for循环失去控制。e.g. 有可能死循环**
  -  **建议for循环的循环控制变量采取前闭后开区间的写法。e.g. for(i=0;i<10;i++) 这里的10可以代表10词循环有物理意义。**

  **for循环变种**

  **变种1：**

  ```c
  #include <stdio.h>
  int main()
  {
      for(;;)
      {
      	printf("hehe\n");    
      }
      return 0;
  }
  ```

  for循环判断部分省略代表条件恒为真，死循环。

  但随便省略会有bug,例如：

  ```c
  #include <stdio.h>
  int main()
  {
      int i = 0;
      int j = 0;
      for (;i<10;i++)
      {
          for (; j<10;j++)
          {
              printf("hehe\n");
          }
      }
      return 0;
  }//这里没有打印100个hehe,而是打印了10个。
  ```

  **变种2**

  ```c
  #include <stdio.h>
  int main()
  {
      int x,y;
      for(x=0,y=0;x<2&&y<5;++x,y++)
      {
          printf('hehe\n');
      }
      return 0;
  }
  ```

  可能的bug:

  ```c
  #include <stdio.h>
  int main()
  {
      int i = 0;
      int k = 0;
      for (i=0,k=0;k=0;i++,k++)
      {
          k++;
      }
      return 0;
  }//此代码循环不执行。
  ```

- **do while** 

  ```c
  #include <stdio.h>
  int main()
  {
      int i = 1;
      do
      {
      	printf("%d",i);
          i++
      }
      while(i<=10);
      return 0;
  }//先执行再判断
  ```

  