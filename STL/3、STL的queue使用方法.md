 

一、了解
----

`queue`是[单调队列](https://so.csdn.net/so/search?q=%E5%8D%95%E8%B0%83%E9%98%9F%E5%88%97&spm=1001.2101.3001.7020)。时间复杂度O（1）。

*   队尾（Rear）：元素**入队**的位置。
*   队头（Front）：元素**出队**的位置。
*   默认情况下，queue是基于deque（双端队列）实现的。也可以自己定义底层容器（使用vector或list）
*   没有清空操作

> ### 常见问题
> 
> *   ### 1、栈和队列的区别
>     
>     *   队列是先进先出（FIFO）的数据结构，而栈是后进先出（LIFO）的数据结构。在队列中，在队尾添加元素，在队头删除元素；而在栈中，在同一端添加和移除元素。
> *   ### 2、阻塞队列和非阻塞队列的区别
>     
>     *   阻塞队列
>         *   **队列为空时**，从队列中获取元素的线程会**被阻塞**，直到队列中有新元素。
>         *   **当队列已满时**，向队列中添加元素的线程会**被阻塞**，直到队列中有空的空间。
>     *   非阻塞队列
>         *   当队列为空时，从队列中获取元素的**线程会立即返回（或抛出异常）**。
>         *   当队列已满时，向队列中添加元素的**线程会立即返回（或抛出异常）**。

*   使用的头文件

```cpp
#include< queue >
```

二、初始化
-----

> queue< [typename](https://so.csdn.net/so/search?q=typename&spm=1001.2101.3001.7020) >name;//基于deque容器实现  
> queue< typename , sequence >name;//自定义底层实现容器

*   例子

```cpp
queue<int>q1;
queue<int,list<int>>q2;
```

三、常用方法函数
--------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/27afe704d1a843b9b051a0e4ce7d01b8.png)

> **注意**：没有clear

### 2、例子

首先是这里用到的头文件

```cpp
#include<iostream>
#include<cstdio>
#include<queue>
using namespace std;
```

#### 2.1、插入操作

*   q.push(val)
    *   队尾插入val的值

```cpp
int main(){
    queue<int>q;
    //插入
    q.push(1);
    q.push(2);
    q.push(3);
    q.push(4);

}

```

#### 2.2、删除操作

*   q.pop( )删除
    *   删除队头

```cpp
int main(){
    queue<int>q;
    //插入
    q.push(1);
    q.push(2);
    q.push(3);
    q.push(4);
    //删除队头
    q.pop();//删除1
    printf("%d",q.front());//2
}
```

#### 2.2、获取大小

*   q.size()
    *   当前有的元素个数

```cpp
int main(){
    queue<int>q;
    //插入
    q.push(1);
    q.push(2);
    q.push(3);
    q.push(4);
    int num = q.size();//元素数量
    printf("%d\n",num);
    
}

```

#### 2.3、访问元素

*   q.front()
    *   返回队头元素

```cpp
int main(){
    queue<int>q;
    //插入
    q.push(1);
    q.push(2);
    q.push(3);
    q.push(4);
    int data1 = q.front();//打印队头
    printf("队头:%d\n",data1);//1
    
    
}

```

*   q.back()
    *   返回队尾元素

```cpp
int main(){
    queue<int>q;
    //插入
    q.push(1);
    q.push(2);
    q.push(3);
    q.push(4);
    int data2 = q.back();//打印队尾
    printf("队尾:%d\n",data2);//4
    
}

```

#### 2.4、判断queue是否为空

*   q.empty()
    *   判断是否为空

```cpp
int main(){
    queue<int>q;
    int ans = q.empty();
    printf("%d",ans);//1
}
```

#### 2.5、交换操作

*   q.swap(other\_queue)
    *   交换q和other\_queue的值

```cpp
int main(){
    queue<int>q1;
    queue<int>q2;
    q2.push(1);
    q2.push(2);
    q2.swap(q1);
}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146051834?spm=1001.2014.3001.5502>，如有侵权，请联系删除。