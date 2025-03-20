 

一、了解
----

`stack`是 栈（**先进后出，后进先出**）（LIFO, Last-In-First-Out）的容器。

*   **stack的特性**：
    *   stack的**默认情况下是deque（队列）作为其底层容器**。
    *   但是也可以使用vector（动态数组）、list（双链表）来作为底层容器。在初始化里会写到如何把deque改成别的底层容器(**必须是顺序容器**)。
*   没有清空操作

> ### 常见问题
> 
> *   ### 1、[stack](https://so.csdn.net/so/search?q=stack&spm=1001.2101.3001.7020)的数据结构特点？
>     
>     *   （LIFO）先进后出，后进先出。
> *   ### 2、解释栈的两个基本操作是什么？
>     
>     *   **push和pop**。push是入栈，将元素压入栈顶，pop是出栈，从栈顶删除元素。满足LIFO特性。
> *   ### 3、什么是栈溢出？
>     
>     *   栈溢出是程序因**超出栈内存空间限制**而引发的错误。
>     *   比如：我想在已经存满元素的栈内存空间里继续入栈就会出现问题。或者，在没有元素的栈内存空间里删除元素也会出现问题。
> *   ### 4、怎样使用栈来判断一个字符串中的括号是否平衡？
>     
>     *   这是一道常见的算法题。[力扣的题20.有效字符](https://leetcode.cn/problems/valid-parentheses/)
>     *   步骤如下：
>         *   遍历字符串string s
>         *   将s中的`[ { (`插入stack中。
>         *   如果插入完后为空，返回false。
>         *   然后用 栈顶 去匹配 `) ] }`，如果相同就删除栈顶。
>         *   最后看栈是否为空，为空就是对的。否则是错的。  
>             该力扣题的答案：  
>             ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/df2bcce6b56f4bc0b81dbd846f4e550e.png)
> *   ### 5、如何用两个栈实现一个队列？
>     
> *   队列的特点是（FIFO）先进先出。
>     
> *   步骤：
>     
>     *   用一个栈做入队的操作，另一个做出队的操作。
>     *   把{1,2,3}插入栈1（入队操作）。当需要出队操作时，就把{1,2,3}出栈到栈2 。
>     *   栈2里进行出栈（出队操作）。  
>         ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fd2760c6f0004bf2aebd394c47041a80.png)

```cpp
//使用的头文件是
#include <stack>
```

二、初始化
-----

> stack < typename > name //默认deque  
> stack < typename , sequence > name//自定义容器来作为底层

```cpp
stack<int> s1;
```

```cpp
stack<int,vector<int>>s2;
stack<int,list<int>>s3;
```

三、常用方法函数
--------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/df2eece982e447659cf195a4bd61d884.png)

### 2、例子

首先是这里用到的头文件

```cpp
#include<iostream>
#include<cstdio>
#include<stack>
using namespace std;
```

> stack默认底层容器的情况下，不能直接使用for(auto : s)来遍历。

```cpp
int main(){
	//初始化
    stack<int>s;
    //插入元素
    s.push(1);
    s.push(2);
    s.push(3);
    s.push(4);
    //获取栈s的大小
    int num = s.size();
    printf("%d\n",num);
    //访问的方法:需要用到empty( )
    while(s.empty()==0){
    	//获取栈顶元素
        int data = s.top();
        printf("%d ",data);//4 3 2 1
        //删除栈顶元素
        s.pop();
    }
}

```

*   交换操作
    *   swap(other\_stack)
    *   交换两个stack的值

```cpp
int main(){
    stack<int>s1;
    stack<int>s2;
    s2.push(1);
    s1.swap(s2);
    
}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146048404?spm=1001.2014.3001.5502>，如有侵权，请联系删除。