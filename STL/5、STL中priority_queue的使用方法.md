 

一、了解
----

[priority\_queue](https://so.csdn.net/so/search?q=priority_queue&spm=1001.2101.3001.7020) 用于实现**优先队列（堆）**。它**基于 std::vector（默认使用vector） 或 std::deque 实现**，**默认情况下是一个最大堆（即队首元素是最大的元素）**。

*   对于异常处理，**空队列**一般自定义，但是**priority\_queue 会使用std：：out\_of\_range异常**。
*   时间复杂度
    *   插入操作：O(log n)
    *   删除操作：O(log n)
    *   **访问队首元素**：O(1)

> ### 常见问题
> 
> *   ### 1、如何从给定的无序数组中找到第 K 大的元素？
>     
>     *   [参考题目力扣215数组中的第k个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/)  
>         ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/12b3d22eb5474a5aa2ca399d77c68337.png)
> *   ### 2、实现一个[优先队列](https://so.csdn.net/so/search?q=%E4%BC%98%E5%85%88%E9%98%9F%E5%88%97&spm=1001.2101.3001.7020)，并解释它的插入和删除操作的时间复杂度。
>     
>     *   优先队列是堆结构
>     *   **插入：把新元素放到最后，再与其他元素对比后上浮到合适位置。**
>     *   **删除：删除的是堆顶。删除后，把最后的元素移到最上面，然后再下浮到合适位置。**
>     *   插入操作时间复杂度：O(log n)
>     *   删除操作时间复杂度：O(log n)
> *   ### 3、什么是堆排序？它的时间复杂度和空间复杂度是多少？
>     
>     *   堆排序：通过构建堆，将数组中的元素按升序或降序排列。
>     *   时间复杂度：O(n log n) 空间复杂度：O(1)（就地排序）
> *   ### 4、二叉堆和斐波那契堆有什么区别？它们的操作的时间复杂度有何不同？
>     
>     *   二叉堆和斐波那契堆主要区别在于他们的操作有不同时间复杂度。
>     *   二叉堆：**插入和删除**的时间复杂度是O（log n）
>     *   菲波那切堆：**插入和减小**都是O（1），**删除最小元素**是O（log n）但是均摊时间复杂度。
> *   ### 5、如何在 O(n) 时间复杂度内构建一个堆？
>     
>     *   **找到最后一个非叶子节点**：最后一个非叶子节点的索引为 **n/2 - 1（其中 n 是数组的长度）**。
>     *   **从后向前调整**：从最后一个非叶子节点开始，**向前遍历每个节点**，**对每个节点执行“下沉”操作**，确保**以该节点为根**的子树满足堆的性质。

//使用的头文件

```cpp
#include<queue>
```

二、初始化
-----

> priority< typename> name;  
> priority<typename,sequence >name;

```cpp
priority_queue<int>q1;//最大堆，堆顶是最大值
priority_queue<int,vector<int>>q2;//最小堆，堆顶是最小值
```

三、常用函数
------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1608048129794253811762632afab22c.png#pic_center)

### 2、例子

首先是这里用到的头文件

> #include< iostream>  
> #include< cstdio>  
> #include< queue>  
> using namespace std;

#### 2.1、插入操作

*   q.push(value)

```cpp
int main(){
    priority_queue<int>q1;//最大堆，堆顶是最大值
    q1.push(1);
    q1.push(4);
    q1.push(2);
    q1.push(3);
    while(q1.empty()==0){
        cout<<q1.top()<<" ";//4 3 2 1
        q1.pop();
    }
}
```

*   q.pop()

```cpp
int main(){
    priority_queue<int>q1;//最大堆，堆顶是最大值
    q1.push(1);
    q1.push(4);
    q1.push(2);
    q1.push(3);
    q1.pop();
    while(q1.empty()==0){
        cout<<q1.top()<<" ";//3 2 1
        q1.pop();
    }
}
```

#### 2.2、访问操作

*   q.top()

```cpp
int main(){
    priority_queue<int>q1;//最大堆，堆顶是最大值
    q1.push(1);
    q1.push(4);
    q1.push(2);
    q1.push(3);
    
    printf("%d",q1.top());//4
}
```

#### 2.3、容量操作

*   q.empty()

```cpp

int main(){
    priority_queue<int>q1;//最大堆，堆顶是最大值
    q1.push(1);
    q1.push(4);
    q1.push(2);
    q1.push(3);
    if(q1.empty()==1){
        cout<<"q1是空的"<<endl;
    }else{
        cout<<"q1不是空的"<<endl;
    }
}
```

*   q.size()

```cpp
priority_queue<int>q;
q.push(1);
int num =q.size();
cout<<num<<endl;//1
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146077378?spm=1001.2014.3001.5502>，如有侵权，请联系删除。