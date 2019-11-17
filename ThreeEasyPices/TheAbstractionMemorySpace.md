

#  The Abstraction : Address Space

#### 0x00 Preface

​	OS:The Operating System allocates a particular area which is may not continuous to  a running program.

​	Program: There are isolating memory space address that is begin at 0 and end at  a paticular`size` for itself.

​	We call this technique is Memory Virtualization, which  let a running program felt that it occupied the entire memory  all the time.And then , OS create an easy to use abstraction of physical memory---address space;

#### 0x01 CONCEPT

​               *英文确实是一个坎，只能简简单单写一写前言，虽然我看的是英语原版，但是照抄概念跟没学没什么区别，用自己的话组织吧，英文有不行，只好用自己最爱的母语来写吧。*

​	操作系统分配给程序一个看似连续的空间，实际上在物理内存中是`fragment`。但是，进程自己不知道。它使用内存就像使用连续的一样。

​	比如设定一开始连续的地址为0， 最后一个地址N, 从0开始放置与程序有关的数据，比如由于程序代码是静态的，在进程运行过程中是不会增加的。那么就把`code`放在0开始，然后到`Code.size()-1`的位置结束。紧接着放置`Heap`存放动态内存数据，之后有一大段是未使用内存（free state），之后是`Stack`存放局部变量，函数调用等数据内容，还有其它区域比如`Static`,`const`

#### 0x02 Goals

- 透明度

  程序自己不知道，只知道自己使用的是连续的就足够了。

- 保护

  程序数据不能使其它程序进行更改，比如获取该程序某个变量地址等。

#### 0x03 Homework

- 查看关于free 工具 的man 手册

  - free 查看系统内存的使用情况以及swap区存储情况
  - 同样可以查看`/proc/meminfo`来获取相应的信息

- 练习free工具的使用

  - ```shell
    free -b -k -m -g --tebi --pebi ..... #使用不同的单位显示内存相应容量
    free -h  --human #使人们更容易认知的单位显示出来B，K，M，G...
    free -w --wide #它默认显示长度很小，如果使用这个将会让buff和cache分开显示
    ```

- 学习pmap工具

  - 报告进程内存映射

  - ```shell
    #usage
    $pmap [options] pid[]
    $pmap -x 1176
    $pmap -d 23722 | tail -n 1
    mapped: 13996K    writeable/private: 356K    shared: 0K
    #mapped 表示OS分配虚拟内存大小，
    #w/p进程实际占用内存
    #shared: 与其他进程共享内存大小
    #ssr 客户端
    mapped: 447168K    writeable/private: 60560K    shared: 16920K
     
    ```

