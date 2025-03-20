 

一、理解
----

`deque`:**双端队列**是一种在内存中存储元素的[数据结构](https://so.csdn.net/so/search?q=%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84&spm=1001.2101.3001.7020)，**它支持在队头和队尾都能进行插入和删除操作**。**可以保持迭代器有效性**。不会像vecotr一样迭代器失效。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/42522895c2ae4a3b8511c0b2c2b82add.png)

*   常见的实现方式
    *   基于动态数组vector时
        
        *   优点：**内存分块连续，支持随机访问**。
        *   缺点：扩容时，需重新分配内存并复制数据。如果需要对中间数据进行操作，就效率很低。
    *   基于双向链表list时
        
        *   优点：**插入和删除操作在任意位置均高效（O(1)）**。
        *   缺点：**内存不连续，无法随机访问**，需额外存储前后节点指针。

> ### 常见问题
> 
> *   ### 1、解释 [deque](https://so.csdn.net/so/search?q=deque&spm=1001.2101.3001.7020) 和 vector 的主要区别是什么？
>     
>     *   `内存分配`：`vector` 使用**单一连续的内存空间**来存储元素，而 `deque` **使用多个分散的内存块**。vector只能在尾部操作，而deque可以在头和尾进行操作。
>     *   `插入效率`：`vector`在尾部操作很方便，但是其他地方很麻烦，因为需要移动大量元素。`deque`在头和尾很方便，但是中间效率低，因为要移动多个内存块中的元素。
>     *   `随机访问`：`vector`连续的空间可以**随机访问**，但是`deque`是分散的内存块就导致**随机访问性能稍低**。
>     *   `内存消耗`：`vector`由于**连续的内存**，**扩展时就消耗高**，但是`deque`是**分散的内存块**，**内存消耗低**。
> *   ### 2、可以用 deque 实现一个固定大小的滑动窗口（[理解滑动窗口算法](https://www.bilibili.com/video/BV1nT411p7L4/?spm_id_from=333.337.search-card.all.click&vd_source=c6d25b8c148a15b1c45b8da5d613a042)）吗？
>     
>     *   实现思路：
>         *   初始化：创建一个 std::deque 对象，并设置窗口大小。
>         *   添加元素：当新元素加入时，如果窗口已满（即 deque 的大小等于窗口大小），则移除最旧的元素（从队列前端移除）。
>         *   获取窗口内容：可以通过 std::deque 直接访问当前窗口内的所有元素。
>     *   [参考图力扣滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)
> *   ### 3、在 deque 的前端插入一个元素时，迭代器会发生什么？
>     
>     *   在**前端插入**元素通常\*\*会导致所有迭代器、引用和指针失效。\*\*因为 deque 可能会在内部进行重新分配，特别是当没有足够的前端空间来容纳新元素时。**后端插入**时，**只有end()迭代器失效**。这与vector是不同的。
> *   ### 4、在 deque 中使用 \[\] 操作符和 at() 方法有何区别？
>     
>     *   **\[ \]不会检查边界**。
>     *   **at()会检查边界，并且越界时会抛出异常**。
> *   ### 5、 解释 deque 的内部工作机制。它是如何实现两端插入和删除操作的高效性的？
>     
>     *   在前端插入：如果前端还有空间，就直接使用，否则会创建新的空间，存放数据。
>     *   在后端插入：去前端一样的原理。
>     *   在前端删除：删除后，如果这块是空的，就释放它。
>     *   在后端删除：与前端一样。

```
//使用的头文件
#include< queue >
```

二、初始化
-----

```
1、deque<T>d;//创建一个空的
2、deque<T>d(n);//创建一个包含 n 个默认初始化元素的 deque。
3、deque<T>d(n,value);//创建一个包含 n 个值为 value 的元素的 deque。
4、deque<T>d(begin,end);//创建一个包含范围 [begin, end) 内元素的 deque。
5、deque<T>d(other_deque);//创建一个 deque，并拷贝 other_deque 的内容。
6、deque<T>d={ elem1,elem2......};//初始化列表创建
```

```cpp
int main(){
    deque<int>q1;
    deque<int>q2(10);
    deque<int>q3(10,1);
    deque<int>q4(q3.begin(),q3.end());
    deque<int>q5(q3);
    deque<int>q6{1,2,3,4};//或者deque<int>q7={1,2,3,4};
}
```

三、常用方法函数
--------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c3f196d1834f4e3a919cc09f5f07578c.png#pic_center)

### 2、例子

首先是这里用到的头文件

```
#include<deque>
#include<algorithm>
#include<cstdio>
using namespace std;
```

#### 2.1、元素访问

*   q\[下标\]

```cpp

int main(){
    deque<int>q2(10);
    int data =q2[0];
    printf("%d",data);//0
}

```

*   q.at(下标)

```cpp
int main(){
    deque<int>q2(10);
    int data =q2.at(0);
    printf("%d",data);//0
}
```

*   q.front()

```cpp
int main(){
	deque<int>q6{1,2,3,4};
    int front =q6.front();
    printf("%d\n",front);//1
}
```

*   q.back()

```cpp
int main(){
	deque<int>q6{1,2,3,4};
    int back = q6.back();
    printf("%d\n",back);//4
}
```

#### 2.2、容量操作

*   q.size()

```cpp
int main(){

    deque<int>q{1,2,3,4};
    int count = q.size();
    printf("%d",count);//4
}
```

*   q.empty()

```cpp
int main(){

    deque<int>q{1,2,3,4};
    if(q.empty()==0){
        printf("q不是空的");
    }
}
```

*   q.resize(n)

```cpp
int main(){

    deque<int>q{1,2,3,4};
    q.resize(10);
    for(auto i: q){
        printf("%d ",i);//1 2 3 4 0 0 0 0 0 0
    }
}
```

*   q.resize(n,new\_value)

```cpp
int main(){

    deque<int>q{1,2,3,4};
    q.resize(10,100);
    for(auto i: q){
        printf("%d ",i);//1 2 3 4 100 100 100 100 100 100
    }
}
```

#### 2.3、插入和删除操作

*   q.push\_front()

```cpp
int main(){

    deque<int>q;
    q.push_front(1);
    for(auto i: q){
        printf("%d ",i);//1
    }
}
```

*   q.push\_back()

```cpp
int main(){

    deque<int>q;
    q.push_front(1);
    q.push_back(2);
    for(auto i: q){
        printf("%d ",i);//1 2
    }
}
```

*   q.pop\_front()

```cpp
int main(){

    deque<int>q={1,2,3};
    q.pop_front();
    for(auto i: q){
        printf("%d ",i);// 2 3
    }
}

```

*   q.pop\_back()

```cpp
int main(){

    deque<int>q={1,2,3};
    q.pop_back();
    for(auto i: q){
        printf("%d ",i);//1 2 
    }
}
```

*   q.insert(pos,value)

```cpp
int main(){

    deque<int>q={1,2,3};
    q.insert(q.begin(),0);
    for(auto i: q){
        printf("%d ",i);//0 1 2
    }
}
```

*   q.insert(pos,n,value)

```cpp
int main(){
    deque<int>tmp{10,11,12};
    deque<int>q={1,2,3};
    q.insert(q.begin(),3,10);
    for(auto i: q){
        printf("%d ",i);//10 10 10 1 2 3
    }
}
```

*   q.insert(pos,first,last)

```cpp
int main(){
    deque<int> d = {1, 2, 3};
    vector<int> v = {10, 20};
    auto it = d.insert(d.begin() + 1, v.begin(), v.end());  // 在索引 1 处插入 {10, 20}
// d 变为 {1, 10, 20, 2, 3}
// it 指向 10
}
```

*   q.insert(pos,{初始化列表})

```cpp
int main(){
    deque<int> q = {1, 2, 3};

    q.insert(q.begin(),{11,12});
    for(auto i:q){
        cout<< i<<" ";//11 12 1 2 3
    }
}
```

#### 2.4、迭代器

*   q.begin()

```cpp
int main(){
    deque<int> q = {1, 2, 3};
    deque<int>::iterator it = q.begin();
    cout<<*it<<endl;//1
    
}
```

*   q.end()

```cpp
int main(){
    deque<int> q = {1, 2, 3};
    deque<int>::iterator it = q.end()-1;
    cout<<*it<<endl;//3
    
}
```

#### 2.5、交换操作

*   q.swap(other\_deque)

```cpp
int main(){
    deque<int> q = {1, 2, 3,4};
    deque<int>q1 = {4,5,6};
    q.swap(q1);
    cout<<"q  :";
    for(auto i:q){
        cout<<i<<" ";
    }
    cout<<endl;
    cout<<"q1: ";
    for(auto i:q1){
        cout<<i<<" ";
    }
    //q  :4 5 6
    //q1: 1 2 3 4
}
```

*   swap(q1,q2)

```cpp
int main(){
    deque<int> q = {1, 2, 3,4};
    deque<int>q1 = {4,5,6};
    swap(q,q1);
    cout<<"q  :";
    for(auto i:q){
        cout<<i<<" ";
    }
    cout<<endl;
    cout<<"q1: ";
    for(auto i:q1){
        cout<<i<<" ";
    }
    //q  :4 5 6
    //q1: 1 2 3 4
}
```

#### 2.6、替换操作

*   q.assign(n,value)

```cpp
int main(){
    deque<int> q = {1, 2, 3,4};
    q.assign(10,1);
    for(auto i:q){
        cout<<i<<" ";//1 1 1 1 1 1 1 1 1 1
    }

}
```

*   q.assign(begin(),end())

```cpp
int main(){
    deque<int> q = {1, 2, 3,4};
    deque<int> q1 = {11, 22, 33,44};
    q.assign(q1.begin(),q1.end());
    for(auto i:q){
        cout<<i<<" ";//11 22 33 44
    }

}

```

*   q.assign({初始化列表})

```cpp
int main(){
    deque<int> q = {1, 2, 3,4};
    q.assign({11,12,13});
    for(auto i:q){
        cout<<i<<" ";//11 12 13
    }

}
```

#### 2.7、清空操作

*   q.clear()

```cpp
int main(){
    deque<int> q = {1, 2, 3,4};
    q.clear();
    for(auto i:q){
        cout<<i<<" ";//
    }

}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146057927?spm=1001.2014.3001.5502>，如有侵权，请联系删除。