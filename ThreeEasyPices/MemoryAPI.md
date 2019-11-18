### Memory API

---

#### 0x00 Preface

​	We can know that the compiler allocated some rooms at stack to local variables and function invocation. Therefore, how can we, the programmer, invoke the Memory API in C/C++?

####0x01 Stack

​	Sometimes, called the stack as automatic memory.

​	OS为每一个程序分配一个看似连续的地址空间，编译器将局部变量以及函数掉用相关数据存储到Stack区域中，这里地址空间分配是由编译器完成的，还有释放地址空间都是隐式的。

​	由于我们知道，`stack and heap`的内存区域都是可以增长的，所以其中有很大一部分内存是未使用的（unused area or free area），但是系统已经分配给了进程，使进程有一定的弹性。

#### 0x02 heap

`heap`中的数据，是需要程序员自己去分配，这里是动态存储空间，而且需要程序员显式释放变量的存储空间。

#### 0x03 malloc

`#inlcude<stdlib.h>`   [1]

`void * malloc(size_t size)` [2]

[1] 为`malloc`所需要的头文件，［2］为函数原型。

将分配size比特大小的空间（heap）, 返回空指针。

一般用法：

```c
int *ptr = (int *)malloc(sizeof(int) * 100); //allocated 100 int type rooms
```



#### 0x04 free

`free`释放内存空间。

​	首先要注意到`free`参数并没有传递大小，那么如何释放的呢？

```c
int * p = (int *)malloc(sizeof(int) *6);
for(int i = 0 ; i < 6; ++i)
{
    *(p+i) = i;
}

++p;
++p;
free(p);

// what does it will happen?

/*output :
free(): invalid pointer
Aborted (core dumped)
*/	
```

​	分析程序：

​	传递给free的指针不是`malloc`分配的初始位置, 猜测：在malloc一个对象之后，其会在某个地方设置该对象分配的大小，一边free使用。在学完`gdb tool`之后再探。



 

​	